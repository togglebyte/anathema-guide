# Input

A single line input field.

```rust
builder
    .default::<Input>("input", Input::template())
    .unwrap();

```

The input component can be registered as a prototype as well. 

```rust
builder
    .prototype("input", Input::template(), Input::new, InputState::new)
    .unwrap();

```

## Supported events:

The following events are provided:

###  `on_enter` 

The enter key was pressed.
This event publishes `anathema_extras::Text`, which implements `Deref<str>`.

### `on_change` 

A change was made to the text (insert or remove).
This event publishes `anathema_extras::InputChange`.

### `on_focus` 

The input component gained focus.
This event publishes `()`.

### `on_blur` 

The input component lost focus.
This event publishes `()`.

## Attributes

### `clear_on_enter`

If the desired outcome is to retain the text when the enter key is pressed set
this attribute to false.

```
@input [clear_on_enter: false]
```

## Example

```
vstack
    @input (on_enter->update_label_a)
    @input (on_enter->update_label_b)
```

# Button
