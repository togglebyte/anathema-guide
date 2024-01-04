# User model

Implementing the trait `UserModel` [TODO]

### `Message`

The `type` that can be sent to the user model via the event system.

```rust
impl UserModel for Model {
    type Message = String;
    ...
}

let runtime_tx = runtime.tx();

runtime_tx.send(
```

### `event(&mut self, Event<Self::Message>, &mut WidgetContainer)`

Called by the runtime for every event, such as key press, mouse (if mouse is
enabled) and user generated events (data send to the runtime via the `Sender`).

### `data`
The `data` function is called by the runtime once every frame.



```rust
use anathema::runtime::{Event, Sender};
use anathema::runtime::UserModel;
use anathema::templates::DataCtx;
use anathema::widgets::WidgetContainer;

struct Model {
    data: DataCtx,
    tx: Sender<()>,
}

impl UserModel for Model {
    type Message = ();

    fn event(&mut self, event: Event<Self::Message>, root: &mut WidgetContainer) {
        if event.ctrl_c() {
            self.tx.send(Event::Quit).unwrap();
        }
    }

    fn data(&mut self) -> &mut DataCtx {
        &mut self.data
    }
}
```
