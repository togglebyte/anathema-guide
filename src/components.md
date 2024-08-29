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

A component has to be [registered](./runtime.md#registering-components) with the runtime before it can be used in the template.

## Usage 

Use a component in a template by prefixing the tag with the `@` sign:

```
border
    @my_comp
```
