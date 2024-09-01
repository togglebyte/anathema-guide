# Event handling

For a component to receive events it has to enable event handling by
implementing one or more of the following methods:

### `on_key`

Accept a key event and a mutable reference to the state (if one is
associated with the component).

```rust,ignore
fn on_key(
    &mut self,
    key: KeyEvent,
    state: &mut Self::State,
    mut elements: Elements<'_, '_>,
    mut context: Context<'_, Self::State>,
) { }
```

### on_mouse

Accept a mouse event and a mutable reference to the state (if one is
associated with the component).

```rust,ignore
fn on_mouse(
    &mut self,
    mouse: MouseEvent,
    state: &mut Self::State,
    mut elements: Elements<'_, '_>,
    mut context: Context<'_, Self::State>,
) { }
```

### on_focus

The component gained focus.

```rust,ignore
fn on_focus(
    &mut self, 
    state: &mut Self::State, 
    mut elements: Elements<'_, '_>,
    mut context: Context<'_, Self::State>,
) {}
```

### on_blur

The component lost focus.

```rust,ignore
fn on_blur(
    &mut self, 
    state: &mut Self::State, 
    mut elements: Elements<'_, '_>,
    mut context: Context<'_, Self::State>,
) {}
```

### Example

```rust,ignore
use anathema::widgets::components::events::{KeyCode, KeyEvent, MouseEvent};
use anathema::widgets::Elements;

impl Component for MyComponent {
    type Message = ();
    type State = MyState;

    fn on_key(
        &mut self,
        key: KeyEvent,
        state: &mut Self::State,
        mut elements: Elements<'_, '_>,
        mut context: Context<'_, Self::State>,
    ) {
        // Get mutable access to the name
        let mut name = state.name.to_mut();

        if let Some(c) = key.get_char() {
            name.push(c);
        }

        if let KeyCode::Enter = key.code {
            name.clear();
        }
    }

    fn on_mouse(
        &mut self,
        mouse: MouseEvent,
        state: &mut Self::State,
        mut elements: Elements<'_, '_>,
        mut context: Context<'_, Self::State>,
    ) {
        // Mouse event
    }
}
```
