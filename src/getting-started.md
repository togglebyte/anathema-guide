# Getting started

## Requirements
* Rust 1.62 or newer
* Some understanding of the Rust programming language

## Download and install
TODO:

## Choosing features

Only use what you need.

By default everything but `logging` and `serde-json` is enabled.

* `default`: includes the entire runtime and template language
* `widgets`: includes widgets and rendering
* `templates`: templates, widgets and rendering (but no runtime)
* `runtime`: runtime and templates

* `serde-json`: this feature enables convert json to `Value` which is the inner
  data type for templates.
