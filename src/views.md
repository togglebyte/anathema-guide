# Views

A view is any type that implements the `anathema::core::View` trait.

This trait has no required methods that has to be implemented, however note that
without implementing the `fn state(&self) -> &dyn State` method, no internal
state will be exposed to the template.

See [State](./templates/state.md) for more information about state.

## Example

```rust
use anathema::core::{Event, KeyCode, Nodes, View};

impl View for MyView {
    fn on_event(&mut self, event: Event, _: &mut Nodes<'_>) {
        match event {
            Event::KeyPress(KeyCode::Char(c), ..) => {
            }
            Event::KeyPress(KeyCode::Backspace, ..) => {
            }
            _ => {}
        }
    }

    fn state(&self) -> &dyn State {
        &self.state
    }

    fn tick(&mut self) {
        *self.state.counter += 1;
    }

    fn focus(&mut self) {
        *self.state.color = Color::Reset;
    }

    fn blur(&mut self) {
        *self.state.color = Color::Magenta;
    }
    
}
```
