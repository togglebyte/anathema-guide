# Getting started

## Requirements
* Rust 1.62 or newer.
* Some understanding of the Rust programming language.

## Install
Add `anathema` to your Cargo.toml file.

```toml
...
[dependencies]
anathema = "<insert latest version here>"
```

### Note

* If all that is required is a way to place characters in the terminal, with
no need for layouts or widgets then see [display only](./display-only.md).
* If the template system and runtime is not needed but the widgets are, then see
  [widgets only](./widgets-only.md).

## A basic example

Render a border around the edges, placing the text in the middle of the
terminal.

```rust
use anathema::runtime::{Event, Runtime};
use anathema::templates::DataCtx;

fn main() {
    let runtime = Runtime::<()>::new();
    let template = r#"
    border:
        alignment [align: centre]: // "center" is also fine
            text: "In the middle of the terminal"
    "#;

    runtime.start(template, DataCtx::empty(), |ev, _root, _ctx, tx| {
        if ev.ctrl_c() {
            let _ = tx.send(Event::Quit);
        }
    }).unwrap();
}
```

## Features

Only use what you need:

By default everything but `logging` and `serde-json` is enabled.

* `default`: includes the entire runtime and template language.
* `widgets`: includes widgets and rendering.
* `templates`: templates, widgets and rendering (but no runtime).
* `runtime`: runtime and templates.

### Extras
* `serde-json`: this feature enables convert json to `Value` which is the inner
  data type for templates.
* `metrics`: this feature will populate the `DataCtx` with metrics such as
  layout time, update time etc.
