# Messages

Communication between components is done either with component queries or an
`Emitter`.

An `Emitter` is used to send a message to any recipient and can be used outside
of the runtime (e.g from another thread etc.).
The recipient id is returned when calling `component`.

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
        mut elements: Children<'_, '_>,
        mut context: Context<'_, '_, Self::State>,
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
let recipient = runtime.component(
    "my_comp", 
    "path/to/template.aml",
    MyComponent::new(),
    MyState::new()
);

let emitter = runtime.emitter();
std::thread::spawn(move || {
    send_messages(emitter, recipient);
});
```

## Internal messaging

It is possible to send a message from one component to another without using an
emitter.

Using the `Context` it's possible to send a message to the first components that fits
the query. Note that only one instance of the value will be sent, even if the
query matches multiple components.

### Example

An example of a component sending a `String` to another component.

```rust,ignore
// Component accepting messages
impl Component for ReceiverComponent {
    type Message = String; // <- accept strings
    type State = MyState;

    fn message(
        &mut self,
        message: Self::Message,
        state: &mut Self::State,
        mut elements: Children<'_, '_>,
        mut context: Context<'_, '_, Self::State>,
    ) {
        state.messages.push_back(message);
    }
}

// Component sending messages
impl Component for SenderComponent {
    type Message = ();
    type State = ();

    fn message(
        &mut self,
        message: Self::Message,
        state: &mut Self::State,
        mut elements: Children<'_, '_>,
        mut context: Context<'_, '_, Self::State>,
    ) {
        context.components.by_name("receiver").send(String::from("What a lovely sweater you have"));
    }
}

runtime.component("receiver", "path/to/template.aml", ReceiverComponent, ()).unwrap();
runtime.component("sender", "path/to/template.aml", SenderComponent, ()).unwrap();
```
