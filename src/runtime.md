# Runtime

## Configuring the runtime

### Changing settings for the terminal:

Example enabling mouse support:
```rust
let term_settings = &mut runtime.output_cfg;
term_settings.enable_mouse = true;
```

The following settings are available:

* `enable_mouse`. Default: `false`. Enable mouse support in the terminal (if
  it's supported).
* `alt_screen`. Default: `true`. Creates an alternate screen for rendering.
  Restores the previous content of the terminal to the state it was in before
  the application started.
* `raw_mode`. Default: `true`. If this is disabled then characters are echoed
  onto the screen and the newline characters will scroll the entire screen.

### Adding a custom widget

```rust
let lookup = &mut runtime.lookup;
lookup.register("mywidget", my_widget_builder_fn);
```

For more information on creating a custom widget, see [creating a custom widget](./templates/creating-custom-widget.md)

### Changing the frame time

This is the time between render / layout / update calls.
This will impact the appearance of animations.

```rust
use std::time::Duration;

runtime.frame_time = Duration::from_ms(40); // default is 20ms
```

### Starting the runtime

To start the runtime provide a template, a data context and finally a closure:

```rust
let runtime = Runtime::<()>::new();
let template = std::fs::read_to_string("template.tiny")?;
runtime.start(template, DataCtx::empty(), |event, root_widget, data_ctx, sender| {
});
```

The closure is called every time there is an [event](./templates/events.md) available.
`Event<T>` can be anything from a key press to a user defined type sent via the
`sender`.

The `root_widget` is the root of the widget tree.
For information on how to manipulate the widget tree see
[widgets](./widgets.md), and the `data_ctx` contains all the template variables
that can be used inside the template.

### Ending the runtime

To stop the runtime send an `Event::Quit` via the `Sender<T>`:

```rust
runtime.start(template, DataCtx::empty(), |event, root_widget, data_ctx, sender| {
    if event.ctrl_c() {
        let _ = sender.send(Event::Quit);
    }
});
```

### Passing information to the runtime

```rust
let runtime: Runtime::<String> = Runtime::new();
let sender = runtime.sender();

// Send a `String` to the runtime:
sender.send("Hello there".to_string());
```
