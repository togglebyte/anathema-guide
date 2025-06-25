# Routing

Routing can be done with a combination of state and `switch`.

Using a `switc / case` in the template we can <something> router.

## Component

We store the route name as a `String`.

The component key change event can be used to select the route.
In this case a route is just a `String`.

Pressing `a` will set the route to `"a"`, and pressing `b` will set the route to `"b"`.
Any other option will set it to `"home"`.

```rust,ignore
use anathema::component::*;
use anathema::prelude::*;

#[derive(Debug, Default)]
struct Router;

impl Component for Router {
    type Message = ();
    type State = String;

    fn on_key(&mut self, key: KeyEvent, state: &mut Self::State, _: Children<'_, '_>, _: Context<'_, '_, Self::State>) {
        match key.get_char() {
            Some('a') => *state = "a".to_string(),
            Some('b') => *state = "b".to_string(),
            _ => *state = "home".to_string(),
        }
    }
}
```

## Template

Since the route is the state (a string) we can use `switch` on the state
and see if it's either `"a"` or `"b"`, otherwise fallback to `@home`.

```aml
border
    switch state
        case "a": @a
        case "b": @b
        default: @home
```


## Setting up the runtime

Finally register both "home", "a" and "b" as templates.

```rust,ignore
fn main() {
    let doc = Document::new("@index");
    let mut backend = TuiBackend::full_screen();
    let mut builder = Runtime::builder(doc, &backend);

    // Add routes
    builder.component("index", "templates/index.aml", Router, String::new()).unwrap();
    builder.template("a", "text 'hello from A'".to_template()).unwrap();
    builder.template("b", "text 'hello from B'".to_template()).unwrap();
    builder.template("home", "text 'default'".to_template()).unwrap();

    let res = builder.finish(&mut backend, |runtime, backend| runtime.run(backend));

    if let Err(e) = res {
        eprintln!("{e}");
    }
}
```
