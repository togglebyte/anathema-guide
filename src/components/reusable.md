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

Components can publish events that will bubble up to all parents. They can be intercepted and stopped from bubbling up using the `on_event` function on the component trait.
Templates can map an incoming event to an internal event id, which is required to receive the event.
The "internal", mapped event id can be accessed using `event.internal_ident`. The external event id, the one that was emitted from the component, can be accessed using `event.name()`.
The component that handles the event has to know what type the event body is to handle it correctly.
The name of the sender component can be accessed using `event.sender` and the event data using `event.data()` or `event.data_unchecked()`.
Note that unlike in std fashion, the `unchecked` refers to the function panicking, not the function causing ub should the data be incorrect.

For example, assuming some input widget emits the event `text_change`. If the input is being used as a username input, it is possible to rename the `text_change` event to `change_username`:

```
@input (text_change->update_username)
```

The `@input` component can now call `context.publish` with owned data:

```rust,ignore
#[derive(State)]
struct InputState {
    text: Value<String>,
}

//...

impl Component for Input {
    fn on_key(
        &mut self,
        key: KeyEvent,
        state: &mut Self::State,
        mut elements: Children<'_, '_>,
        mut context: Context<'_, '_, Self::State>,
    ) {
        context.publish("text_change", state.text.to_ref().clone());
    }
}
```

The parent component will receive the associated ident "update_username" and 
an immutable reference to the components state.

```rust,ignore
impl Component for Parent {
    fn on_event(
        &mut self,
        event: &mut Event<'_>,
        state: &mut Self::State,
        mut elements: Children<'_, '_>,
        mut context: Context<'_, '_, Self::State>,
    ) {
        if event.internal_ident == "update_username" {
            if let None = event.data::<String>() {
                panic!("oh no the event is not a string");
            }
            let value: &String = event.data_unchecked();
        }
    }
}
```
