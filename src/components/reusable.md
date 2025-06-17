# Third party components

Components can be made reusable by implementing placeholders and associated
functions.

A component can have named placeholders for external children:

```
// The template for @mycomponent
border
    $my_children
```

When using a component with named children use the placeholder name
to inject the elements:

```
for x in values
    @mycomponent
        $my_children
            text "hello world"
```

Note that if no placeholder name is given a default of `$children` is assumed:

```
for x in values
    @mycomponent
        text "hello world"
```

```
// The template for @mycomponent
border
    $children
```

## Events

Publishing events from a component is done with `context.publish(ident, T)`
where `ident` is a string slice with the name of the event and `T` is the data
that should be sent.

```rust,ignore
fn on_key(
    &mut self,
    key: KeyEvent,
    state: &mut Self::State,
    mut children: Children<'_, '_>,
    mut context: Context<'_, '_, Self::State>,
) {
    // Third party component emitting an event
    context.publish("my_event", String::from("hello parents"));
}
```

Given the following template example:

```ignore
vstack
    @thirdparty (my_event->event_a)
    @thirdparty (my_event->event_b)
```

And in the component using the third party component:

```rust,ignore
fn on_event(
    &mut self,
    event: &mut UserEvent<'_>,
    state: &mut Self::State,
    mut children: Children<'_, '_>,
    mut context: Context<'_, '_, Self::State>,
) {
    match event.name() {
        "event_a" => {
            let data: &String = event.data::<String>();
            // all parents can get this event
        }
        "event_b" => {
            let data: &String = event.data::<String>();
            
            // no parent will get this event
            event.stop_propagation();
        }
        _ => () // ignored
    }
}
```

In the above example the component is emitting an event named "my_event".
This is mapped to either "event_a" or "event_b" depending on which component is
publishing the event.

To see the emitting components event name (in this case "my_event") use
`event.internal_ident`.
