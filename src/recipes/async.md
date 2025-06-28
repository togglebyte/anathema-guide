# Async and Anathema

Since the UI `Runtime` is blocking, spawn a secondary thread for our async
runtime to run on.

The `Runtime` can not move between threads as the `Value`s are not send.
That's why it's easier, if access to component ids and emitters are required, to
run the async code on a separate thread from the main thread.

## Setting up for async

Create a function that starts an async runtime in a new thread.

Using a component id and an emitter it is possible to send messages into
the Anathema runtime.

In this example we assume there is a component that accepts a `usize` as its
`Message` type.

```rust,ignore
pub fn run_async(emitter: Emitter, component: ComponentId<usize>) {
    thread::spawn(move || {
        tokio::runtime::Builder::new_multi_thread()
            .enable_all()
            .build()
            .unwrap()
            .block_on(async move {
                // async code here
                emitter.emit_async(component, 123).await;
            });
    });
}
```

## Setup the UI

Setup the Anathema runtime on the main thread,
generate component ids and emitters and pass them to the async runtime via the
aforementioned function.

```rust,ignore
use anathema::component::*;
use anathema::prelude::*;

fn main() {
    // -----------------------------------------------------------------------------
    //   - Setup UI -
    // -----------------------------------------------------------------------------
    let doc = Document::new("@index");

    let mut backend = TuiBackend::full_screen();
    let mut builder = Runtime::builder(doc, &backend);
    
    let component_id = builder.default::<MyComponent>("index", "templates/index.aml").unwrap();
    let emitter = builder.emitter();

    // -----------------------------------------------------------------------------
    //   - Setup async -
    // -----------------------------------------------------------------------------
    run_async(emitter, component_id);

    // -----------------------------------------------------------------------------
    //   - Start UI -
    // -----------------------------------------------------------------------------
    builder.finish(&mut backend, |runtime, backend| runtime.run(backend)).unwrap();
}
```
