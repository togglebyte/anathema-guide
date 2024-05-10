# Getting started

## Install
Add `anathema` to your Cargo.toml file.

```toml
...
[dependencies]
anathema = { git = "https://github.com/togglebyte/anathema/", branch = "dev" }
```

### Note

Even though efforts are made to keep this guide up to date there are
possibilities of changes being made and published before they reach
this guide.

At the time of writing, Anathema should be considered alpha.

## A basic example

Render a border around three lines of text, placing the text in the middle of the
terminal.

```rust,ignore
// src/main.rs
use std::fs::read_to_string;

use anathema::runtime::Runtime;
use anathema::templates::Document;
use anathema::backend::tui::TuiBackend;

fn main() {
    let template = read_to_string("template.aml").unwrap();

    let mut doc = Document::new(template);

    let mut backend = TuiBackend::builder()
        .enable_alt_screen()
        .enable_raw_mode()
        .hide_cursor()
        .finish()
        .unwrap();

    let mut runtime = Runtime::new(doc, backend).unwrap();

    runtime.run();
}
```

```
// template.aml
align [alignment: "center"]
    border
        vstack
            text [foreground: "yellow"] "You"
            text [foreground: "red"] "are"
            text [foreground: "blue"] "great!"
```
