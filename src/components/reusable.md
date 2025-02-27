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

## Associated functions

These look almost like callbacks however they run at the end of the update
cycle, and not instantly.

To associate a function with the parent use the following syntax:

```
@input (text_change->update_username)
```

The `@input` component can now call `context.publish`:

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
        context.publish("text_change");
    }
}
```

The parent component will receive the associated ident ("update_username") and the value (as a
`CommonVal`).


```rust,ignore
impl Component for Parent {
    fn receive(
        &mut self,
        ident: &str,
        value: &dyn AnyState,
        state: &mut Self::State,
        mut elements: Children<'_, '_>,
        mut context: Context<'_, '_, Self::State>,
    ) {
        if ident == "update_username" {
            let value: &str = value.to_any_ref().downcast_ref::<InputState>().unwrap().value.to_ref().as_ref();
        }
    }
}
```
