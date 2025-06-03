# Introduction

Anathema is a library for building text user interfaces using composable
components, with a custom markup language.

It was created with the intent to give developers a fast and easy way to
build text user interfaces (TUI) and ship the template(s) alongside the application,
giving the end user the option to customise the layout.

By separating the layout from the rest of the application, reducing the amount of
code needed to express your design, and featuring hot reloading it becomes incredibly fast 
to iterate over the design.

## Why

* Templates can be shipped with the application, giving the end user the ability
  to customise the entire layout of the application.
* Easy to prototype with the markup language and runtime.

## Why not

Anathema does not excel when it comes to building dynamic layouts such as
splitting buffers for your next Vim clone.


## Alternatives
* [dioxus-tui](https://crates.io/crates/dioxus-tui)
* [tuirealm](https://crates.io/crates/tuirealm)
* [ratatui](https://crates.io/crates/ratatui)
* [cursive](https://crates.io/crates/cursive)
