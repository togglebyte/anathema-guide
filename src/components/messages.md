# Messages

Communicate between components using messages and component ids.

An `Emitter` is used to send a message to any recipient.
The recipient id is returned when calling `register_component`.

The `Emitter` has two functions:
* `emit` (use in a sync context)
* `emit_async` (use in an async context)

If the recipient no longer exist the message will be lost.

Implement the `message` method of the `Component` trait for a component to be
able to receive messages.

The following example sets the component to accept `String`s and starts a thread
that sends a `String` to the component every second.

### Example

```rust,ignore
use anathema::runtime::{Emitter, Runtime};

impl Component for MyComponent {
    type Message = String; // <- accept strings
    type State = MyState;

    fn message(
        &mut self,
        message: Self::Message, 
        state: &mut Self::State, 
        elements: Elements<'_, '_>,
        context: Context<'_, Self::State>
    ) {
        state.messages.push_back(message);
    }
}

// Send a string to a recipient every second
fn send_messages(emitter: Emitter, recipient: ComponentId<String>) {
    let mut counter = 0;

    loop {
        emitter.emit(recipient, format!("{counter} message"));
        counter += 1;
        std::thread::sleep_ms(1000);
    }
}

// Get the component id when registering the component
let recipient = runtime.register_component(
    "my_comp", 
    component_template,
    MyComponent::new(),
    MyState::new()
);

let emitter = runtime.emitter();
std::thread::spawn(move || {
    send_messages(emitter, recipient);
});
```
