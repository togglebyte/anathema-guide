# Views be viewing

A view is a type that implements `anathema::core::View`.

This trait has no required methods that has to be implemented.

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
