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

## Associated functions

These look almost like callbacks however they run at the end of the update
cycle, and not instantly.

To associate a function with the parent use the following template syntax:

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

The parent component will receive the associated ident "update_username" and 
an immutable reference to the components state.

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
            let value: &String = *value.to::<InputState>().value.to_ref();
        }
    }
}
```
