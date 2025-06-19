# Runtime

```rust,ignore
use anathema::runtime::Runtime;
use anathema::backend::tui::TuiBackend;

let doc = Document::new("text 'hello world'");

let mut backend = TuiBackend::builder()
    .enable_alt_screen()
    .enable_raw_mode()
    .enable_mouse()
    .hide_cursor()
    .finish()
    .unwrap();
    
// finalize the backend (enable alt mode, ...)
backend.finalize();

let mut builder = Runtime::builder(doc, &backend);
runtime.fps(30); // default
runtime.finish(&mut backend, |rt, backend| rt.run(backend)).unwrap();
```

## Registering components

Before components can be used in a template they have to be registered with the
runtime.

```rust,ignore
let builder = Runtime::builder(document, &backend);

let component_id = builder.component(
    "my_comp",                                  // <- tag
    "template.aml",                             // <- template
    MyComponent,                                // <- component instance
    ComponentState,                             // <- state
);
```

You can use `default` if your component and its state implements Default.
`builder.default::<C>(comp, template)` is equivalent to `builder.component(comp, template, C::default(), C::State::default())`.

## File path vs embedded template

If the template is passed as a string it is assumed to be a path and
hot reloading will be enabled.

To to pass a template (rather than a path to a template) call `to_template` on
the template string:

```rust,ignore
static TEMPLATE: &str = include_str!("template.aml");

let component_id = builder.component(
    "my_comp",
    TEMPLATE.to_template(),
    MyComponent,
    ComponentState,
);
```

### Hot reload

Hot reloading only works with templates that are file paths.

<div class="warning">
<h4>Important note about state values</h4>
Hot reloading will not remember prototype components, so any component generated
with a for-loop for instance, will not have its state retained.

Hot reloading won't work on components that store internal state either, such as
the `Canvas` widget.
</div>

### Multiple instances of a component

To repeatedly use a component in a template, e.g:

```
vstack
    @my_comp
    @my_comp
    @my_comp
```

The component has to be registered as a **prototype** using `prototype`
(instead of `component`):

```rust
builder.prototype(
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
However, messages can be sent using component queries.

If a component and state is empty (zero-sized) and does not have any special functionality, it can be registered with the `template` function: `builder.template("name", "path/to/template.aml");`.

This is equivalent to `builder.prototype(comp, template, || (), || ())`

## Global Events

To register a global event handler on a runtime builder use the `with_global_event_handler` function.

```rust
Runtime::builder(doc, &backend)
    .with_global_event_handler(|event, tabindex, components| {
    Some(event)
});
```

The arguments are of types `Event`, `&mut TabIndex` and `&mut DeferredComponents`.

Return the event that is supposed to propagate to the component that currently
has focus.

When returning `None`, no event will be triggered.

By default `event.is_ctrl_c()` will return `Some(Event::Stop)`.
If a custom event handler is used this has to be added manually if the behaviour
is desired.

To move focus forwards and backwards between components use `tabindex.next()`
and `tabindex.prev()` respectively.

## Configuring the runtime

### `fps`

Default: `30`

The number of frames to (try to) render per second
