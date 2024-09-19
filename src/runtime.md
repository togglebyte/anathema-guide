# Runtime

```rust,ignore
use anathema::runtime::Runtime;
use anathema::backend::tui::TuiBackend;

let doc = Document::new("text 'hello world'");

let backend = TuiBackend::builder()
    .enable_alt_screen()
    .enable_raw_mode()
    .enable_mouse()
    .hide_cursor()
    .finish()
    .unwrap();
    
let mut runtime = Runtime::builder(doc, backend).finish().unwrap();
runtime.fps = 30; // default
```

## Registering components

Before components can be used in a template they have to be registered with the
runtime.

```rust,ignore
let runtime = Runtime::builder(document, backend);

let component_id = runtime.register_component(
    "my_comp",                                  // <- tag
    "template.aml",                             // <- template
    MyComponent,                                // <- component instance
    ComponentState,                             // <- state
);
```

## File path vs embedded template

If the template is passed as a string it is assumed to be a path and
hot reloading will be enabled by default.

To to pass a template (rather than a path to a template) call `to_template` on
the template string:

```rust,ignore
static TEMPLATE: &str = include_str!("template.aml");

let component_id = runtime.register_component(
    "my_comp",
    TEMPLATE.to_template(),
    MyComponent,
    ComponentState,
);
```

### Hot reload

To disable hot reloading set the documents `hot_reload = false`.

```rust,ignore
let mut doc = Document::new("@index");
doc.hot_reload = false;
```

### Multiple instances of a component

To repeatedly use a component in a template, e.g:

```
vstack
    @my_comp
    @my_comp
    @my_comp
```

The component has to be registered as a **prototype** using `register_prototype`
(instead of `registering_comonent`):

```rust
runtime.register_prototype(
    "comp", 
    "text 'this is a template'",
    || MyComponent, 
    || ()
);
```

The main difference between registering a singular component vs a prototype is
the closure creating an instance of the component and the state, rather
than passing the actual component instance and state into the function.

Also note that prototypes does not have a component id and can not have messages
emitted to them.

## Global Events

The global shortcuts can be changed by modifying the runtime's global event handler.
A custom global event handler is any struct that implements `GlobalEvents`.
The global event handler can be set using the `global_events` function.

The `GlobalEvents` trait has 3 functions: `handle`, `ctrl_c` and `enable_tab_navigation`.

### `enable_tab_navigation`

Default: `true`

Returning a `bool` will enable/disable tabbing.

### `ctrl_c`

```rust,ignore
fn ctrl_c(
    &mut self,
    event: Event,
    elements: &mut Elements<'_, '_>,
    global_context: &mut GlobalContext<'_>
) -> Option<Event> { }
```

Default: `Some(event)`

Returning `Some(event)` from this will cause the event to stop propagating and close down the runtime.

### `handle`

```rust,ignore
fn handle(
    &mut self,
    event: Event,
    elements: &mut Elements<'_, '_>,
    ctx: &mut GlobalContext<'_>
) -> Option<Event> { }
```

This function receives any events that weren't catched by ctrl-c or tabbing. Returning `None` will stop the propagation of the event to the components.

## Configuring the runtime

### `fps`

Default: `30`

The number of frames to (try to) render per second.