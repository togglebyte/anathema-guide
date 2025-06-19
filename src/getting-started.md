# Getting started

## Install
Add `anathema` to your Cargo.toml file.

```toml
...
[dependencies]
anathema = { git = "https://github.com/togglebyte/anathema/" }
```

### Note

Even though efforts are made to keep this guide up to date there are
possibilities of changes being made and published before they reach
this guide.

At the time of writing, Anathema should be considered alpha (but almost beta!).

## A basic example

Render a border around three lines of text, placing the text in the middle of the
terminal.

```rust,ignore
// src/main.rs
use anathema::prelude::{Backend, Document, TuiBackend};
use anathema::runtime::Runtime;

fn main() {
    let doc = Document::new("@index");

    let mut backend = TuiBackend::builder()
        .enable_alt_screen()
        .enable_raw_mode()
        .hide_cursor()
        .finish()
        .unwrap();
    backend.finalize();

    let mut builder = Runtime::builder(doc, &backend);
    builder.default::<()>("index", "templates/index.aml").unwrap();
    builder
        .finish(&mut backend, |mut runtime, backend| runtime.run(backend))
        .unwrap();
}

```

```
// templates/index.aml
align [alignment: "center"]
    border
        vstack
            text [foreground: "yellow"] "You"
            text [foreground: "red"] "are"
            text [foreground: "blue"] "great!"
```

## Prelude

Anathema has two convenience modules:

The `prelude` can be used to bootstrap your application, as it contains the
`Runtime`, `Document` etc.

```rust
use anathema::prelude::*;
```

and `components` is suitable for modules that only contains `Component`s.

```rust
use anathema::components::*;
```

See the [crate](https://docs.rs/anathema/latest/anathema/) documentation for more information.
