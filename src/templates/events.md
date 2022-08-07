# Events

TODO: incomplete docs

Events are sent to the user model.

If the default user model is used the closure provided to the runtime is called,
otherwise the `event` method on the custom user model is called.

An event can be one of the following:

* A key press 
* Mouse was moved, button pressed etc.
* User defined data sent to the runtime
* Resize event as a result of the window being resized
* The widgets tree is replaced
* Quit, as in stop the runtime

## Utility functions for working with events

Ctrl+c was pressed:

```rust
if event.ctrl_c() {
}
```

Get the [key code](https://docs.rs/anathema/latest/anathema/runtime/enum.KeyCode.html) and any additional
[modifiers](https://docs.rs/anathema/latest/anathema/runtime/struct.KeyModifiers.html):

```rust
if let Some(keycode) = event.get_keycode() {
    match keycode {
        KeyCode::Char('x') => {},
        KeyCode::Escape => {},
        _ => {}
    }
}
```

Mouse up / down

Get the [screen pos](https://docs.rs/anathema/latest/anathema/display/struct.ScreenPos.html),
[mouse button](https://docs.rs/anathema/latest/anathema/display/events/enum.MouseButton.html)
and any additional [modifiers](https://docs.rs/anathema/latest/anathema/runtime/struct.KeyModifiers.html).

```rust
if let Some((screen_pos, btn, modifiers)) = event.mouse_down() {
}

if let Some((screen_pos, btn, modifiers)) = event.mouse_up() {
}
```

Mouse move. Note that if a button is pressed this event will not return
anything, instead the `mouse_drag` should be used

```rust
if let Some((screen_pos, modifiers)) = event.mouse_move() {
}
```

Mouse dragged.

```rust
if let Some((screen_pos, button, modifiers)) = event.mouse_drag() {
}
```
