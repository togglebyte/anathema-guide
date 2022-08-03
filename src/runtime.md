# Runtime

TODO: Summary

## Example

```rust
use anathema::runtime::{Event, Sender};
use anathema::runtime::{Runtime, UserModel};
use anathema::templates::DataCtx;
use anathema::widgets::WidgetContainer;

struct Model {
    data: DataCtx,
    tx: Sender<()>,
}

impl Model {
    pub fn new(tx: Sender<()>) -> Self {
        Self {
            data: DataCtx::empty(),
            tx,
        }
    }
}

impl UserModel for Model {
    type Message = ();

    fn event(&mut self, event: Event<Self::Message>, _root: &mut WidgetContainer) {
        if event.ctrl_c() {
            self.tx.send(Event::Quit).unwrap();
        }
    }

    fn data(&mut self) -> &mut DataCtx {
        &mut self.data
    }
}

fn main() {
    let basic_runtime = Runtime::new();
    let user_model = Model::new(basic_runtime.tx());
    basic_runtime.start(user_model, Some("template.tiny")).unwrap();
}
```
