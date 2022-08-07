# Introduction

Anathema is a library for building text user interfaces.

It was developed with the intent to give developers a fast and easy way to
write text user interfaces and ship the template(s) along with the application
giving the end user the means to modify the layout.

## Why

* Templates can be shipped with the application, giving the end user the ability
  to customise the entire layout of the application.
* Easy to prototype with the markup language and runtime.

## Only include what you need:
* Renderer only: tools for drawing in the terminal
* Widgets + renderer: Ready made widgets that can be created and controlled with
  Rust.
* Templates and runtime: Write layouts in a simple to use language

## Alternatives
* [dioxus-tui](https://crates.io/crates/dioxus-tui)
* [tuirealm](https://crates.io/crates/tuirealm)
* [tui-rs](https://crates.io/crates/tui)
* [cursive](https://crates.io/crates/cursive)
