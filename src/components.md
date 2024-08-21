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

A component has to be registered with the runtime before it can be used in the template.

```rust,ignore
let runtime = Runtime::new(document, backend);

let component_id = runtime.register_component(
    "my_comp",                                  // <- tag
    "text 'I be a component'"                   // <- template
    MyComponent,                                // <- component instance
    ComponentState,                             // <- state
);
```

## Register default component

If a `Component` and it's associated state both implements `Default` it's
shorter to use `register_default("component_name", "template_path.aml")`

## Usage 

Use a component in a template by prefixing the tag with the `@` sign:

```
border
    @my_comp
```

### Multiple instances of a component

To repeatedly use a component in a template, e.g:

```
vstack
    @my_comp
    @my_comp
    @my_comp
```

The component has to be registered as a **prototype** using `register_prototype`
(instead of `registering_comonent`):

```rust
runtime.register_prototype(
    "comp", 
    "text 'this is a template'",
    || MyComponent, 
    || ()
);
```

The main difference between registering a singular component vs a prototype is
the closure creating an instance of the component and the state, rather
than passing the actual component instance and state into the function.

### Reusable components

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

## State

### External state

A component can have internal state (mutable access) and external state
(read-only).

To pass external state to a component provide a map in the template
with a key-value pair:

```
@my_comp { key: "this is a value" }
```

To use the external state in the component refer to the keys in map passed into the
component:

```
// component template
text "the value is " key
```

### Internal state

<div class="warning">
<h4>Important note about state values</h4>

When assigning a new value to a state, do <strong>not</strong> create a new instance of
Value<T>.

This is bad:

    my_state.value = Value::new(123)

This is how it should be done:

    my_state.some_value.set(123);
    *my_state.another_value.to_mut() = 123;

</div>

Internal state is anything that implements the `State` trait.

See [State](./components/state.md) for more information about state.

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

runtime.register_component("my_comp", template, MyComponent, my_state);
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
    state: &mut Self::State,
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
    state: &mut Self::State,
    elements: Elements<'_, '_>,
) { }
```

### on_focus

The component gained focus.

```rust,ignore
fn on_focus(&mut self, state: &mut Self::State, elements: Elements<'_, '_>) {}
```

### on_blur

The component lost focus.

```rust,ignore
fn on_blur(&mut self, state: &mut Self::State, elements: Elements<'_, '_>) {}
```

### Example

```rust,ignore
use anathema::widgets::components::events::{KeyCode, KeyEvent, MouseEvent};
use anathema::widgets::Elements;

impl Component for MyComponent {
    type Message = ();
    type State = MyState;

    fn on_blur(&mut self, state: &mut Self::State, elements: Elements<'_, '_>) {}

    fn on_focus(&mut self, state: &mut Self::State, elements: Elements<'_, '_>) {}

    fn on_key(
        &mut self,
        key: KeyEvent,
        state: &mut Self::State,
        elements: Elements<'_, '_>,
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
        elements: Elements<'_, '_>,
    ) {
        // Mouse event
    }
}
```

## Messages

To communicate with components messages can be sent to components using the 
component id as an address.

An `Emitter` is used to send any message to any recipient.
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

    fn message(&mut self, message: Self::Message, state: &mut Self::State, elements: Elements<'_, '_>) {
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
