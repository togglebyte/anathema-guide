# Components

A component exposes event handling and state management.
Any type that implements the `anathema::component::Component` trait is a component.

This trait has no required methods, however note that
there are two required associated types:

* State
* Message

If the component does not need to handle state or receive messages
these can be set to the unit type.

```rust,ignore
use anathema::component::Component;

struct MyComponent;

impl Component for MyComponent {
    type State = ();    // <-\
                        //    -- Set to () to ignore
    type Message = ();  // <-/
}
```

A component has to be [registered](./runtime.md#registering-components) with the runtime before it can be used in the template.

## Template syntax

The component name is prefixed with an `@` sign: `@component_name`, followed by
optional associated events and attributes.

```
@ means it's a component
|   Component name                   Attribute name
|   |         Component event        |      Attribute value
|   |         |        Parent event  |      |
V   V         V        V             V      V
@component (click->parent_click) [attrib: "value"]
```

## Usage 

Use a component in a template by prefixing the tag with the `@` sign:

```
border
    @my_comp
```

## Focus and receiving events

Assuming `.enable_mouse()` on the runtime, mouse events are sent to all components, 
however key events are only sent to the component that currently has focus.

It is possible to assign focus to a component from another component using
`context.components.by_name("my_component").set_focus()`.

For more information about querying components see [Messaging](./components/messages.md).

`set_focus` takes an attribute name and a value. It is possible to call
`set_focus` on multiple components within a single frame. This will result in
multiple components receiving both `on_focus` and `on_blur` until the last
component in the focus queue which only receives `on_focus`.

### Example

The following example would set focus on `@component_a` as it has a matching id.
It's not required to name the attribute `id`, it can be any alphanumerical value
(as long as it doesn't start with a number).

```
vstack
    @component_a [id: 1]
    @component_b [id: 2]
    @component_c [id: "not a number"]
```

```rust,ignore
fn on_key(
    &mut self,
    key: KeyEvent,
    state: &mut Self::State,
    mut elements: Children<'_, '_>,
    mut context: Context<'_, '_, Self::State>,
) {
    context.components.by_attribute("id", 1).focus();
}
```
