# Components

A component exposes event handling and state management.
Any type that implements the `anathema::widgets::components::Component` trait is a component.

This trait has no required methods, however note that
there are two required associated types:

* State
* Message

If the component does not need to handle state or receive messages
these can be set to the unit type.

```rust,ignore
use anathema::widgets::components::Component;

struct MyComponent;

impl Component for MyComponent {
    type State = ();    // <-\ 
                        //    -- Set to () to ignore
    type Message = ();  // <-/
}
```

A component has to be compiled by a `Document` and registered with the runtime before it can be used in the
template.

This is done in two steps:
1. Add the component template and associate a tag with the template
2. Register the component with the runtime

```rust,ignore
let mut document = Document::new(template);

//                                                 template
//                                        tag         |
//                                         |          |
let component_id = document.add_component("my-comp", "text 'I be a component'");

//                                  component instance
//                    component id        |         state
//                         |              |           |
runtime.register_component(component_id, MyComponent, ());
```

Use a component in a template by prefixing the tag with the `@` sign:

```
border
    @my-comp
```

Registering a component is taking ownership of a single instance of that
component.
To repeatedly use a component in a template, e.g:

```
for i in [1, 2, 3]
    @my-comp
```

The component has to be registered as a prototype rather than a component:

```
runtime.register_prototype(comp, || MyComponent, || ());
```

The main difference between registering a singular component vs a prototype is
the closure creating an instance of the component and the state, rather
than passing the actual component instance and state into the function.

See [State](./templates/state.md) for more information about state.

## State

### External state

A component can have internal state (mutable access) and external state
(read-only).

To pass external state to a component provide a map in the template
with a key-value pair:

```
@my-comp { key: "this is a value" }
```

To use the external state in the component refer to the keys in map passed into the
component:

```
// component template
text "the value is " key
```

### Internal state

Internal state is anything that implements the `State` trait.

See [State](./templates/state.md) for more information about state.

#### Example 

```rust,ignore
use anathema::state::{State, Value, List};

#[derive(State)]
struct MyState {
    name: Value<String>,
    numbers: Value<List<usize>>,
}

impl MyState {
    pub fn new() -> Self {
        Self {
            name: String::from("Lilly").into(),
            numbers: List::empty(),
        }
    }
}

let mut my_state = MyState::new();
my_state.numbers.push_back(1);
my_state.numbers.push_back(2);

runtime.register_component(my_comp, MyComponent, my_state);
```

## Event handling

For a component to receive events it has to enable event handling by
implementing one or more of the following methods:

### `on_key`

Accept a key event and a mutable reference to the state (if one is
associated with the component).

```rust,ignore
fn on_key(
    &mut self,
    key: KeyEvent,
    state: Option<&mut Self::State>,
    elements: Elements<'_, '_>,
) { }
```

### on_mouse

Accept a mouse event and a mutable reference to the state (if one is
associated with the component).

```rust,ignore
fn on_mouse(
    &mut self,
    mouse: MouseEvent,
    state: Option<&mut Self::State>,
    elements: Elements<'_, '_>,
) { }
```

### on_focus

The component gained focus.

```rust,ignore
fn on_focus(&mut self, state: Option<&mut Self::State>) {}
```

### on_blur

The component lost focus.

```rust,ignore
fn on_blur(&mut self, state: Option<&mut Self::State>) {}
```

### Example

```rust,ignore
use anathema::widgets::components::events::{KeyCode, KeyEvent, MouseEvent};
use anathema::widgets::Elements;

impl Component for MyComponent {
    type Message = ();
    type State = MyState;

    fn on_blur(&mut self, state: Option<&mut Self::State>) {}

    fn on_focus(&mut self, state: Option<&mut Self::State>) {}

    fn on_key(
        &mut self,
        key: KeyEvent,
        state: Option<&mut Self::State>,
        elements: Elements<'_, '_>,
    ) {
        let state = state.unwrap();
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
        state: Option<&mut Self::State>,
        elements: Elements<'_, '_>,
    ) {
        // Mouse event
    }
}
```

## Messages

To communicate with components from outside of the runtime messages can be sent
to components using the component id as an address.

An `Emitter` is used to send any message to any recipient.
The recipient id is the id generated when calling `register_component`.

If the recipient does not exist or if the message type is incorrect the message
will be lost.

**Note**: This means that when sending a `u32` to a component that expects `i32` the
message will be lost.

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

    fn message(&mut self, message: Self::Message, state: Option<&mut Self::State>) {
        state.unwrap().messages.push_back(message);
    }
}

// Send a string to a recipient every second
fn send_messages(emitter: Emitter, recipient: usize) {
    let mut counter = 0;

    loop {
        emitter.emit(format!("{counter} message"), recipient);
        counter += 1;
        std::thread::sleep_ms(1000);
    }
}

let recipient = doc.add_component("my_comp", component_template);

let emitter = runtime.emitter();
std::thread::spawn(move || {
    send_messages(emitter, recipient);
});
```
