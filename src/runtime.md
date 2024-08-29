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
let runtime = Runtime::new(document, backend);

let component_id = runtime.register_component(
    "my_comp",                                  // <- tag
    "text 'I be a component'"                   // <- template
    MyComponent,                                // <- component instance
    ComponentState,                             // <- state
);
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

## Configuring the runtime

### `fps`

Default: `30`

The number of frames to (try to) render per second.

