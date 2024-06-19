# Element query

Both component events and messages provide an element query that can be used to
access elements and attributes in the component.

Attributes can be read or written to.
Since attribute values may originate from the component state it is not possible
to access the state and the attributes at the same time. 

For this reason the element query prevents any use of state via the `query`
function.

## Cast an element

It's possible to cast an element to a specific widget using the `to` method.

The `to` method returns an `Option<&mut T>`.

For an immutable reference use `to_ref`.

### Example

```rust,ignore

fn on_key(
    &mut self,
    key: KeyEvent,
    state: &mut Self::State,
    mut elements: Elements<'_, '_>,
) {
    elements
        .query(&state)
        .by_attribute("abc", 123)
        .first(|el, _| {
            let viewport = el.to::<Viewport>().unwrap();
            viewport.scroll_up();
        });
}
```

## Filter elements

### Example

```rust,ignore
fn on_mouse(
    &mut self,
    mouse: MouseEvent,
    state: &mut Self::State,
    elements: Elements<'_, '_>,
) { 
    elements
        .query(&state)
        .by_attribute("abc", 123)
        .first(|el, attributes| {
            attributes.set("background", "green");
        });
}
```

There are three methods to query the elements inside the component:

### `by_tag`

This is the element tag name in the template, e.g `text` or `viewport`.

```rust, ignore
.query(&state)
    .by_tag("text")
    .each(|el, attributes| {
        attributes.set("background", "green");
    });
```

### `by_attribute`

This is an attribute with a matching value on any element.

```rust, ignore
.query(&state)
    .by_attribute("background", "green")
    .each(|el, attributes| {
        attributes.set("background", "red");
    });
```

### `at_position`

```rust, ignore
    fn on_mouse(
        &mut self,
        mouse: MouseEvent,
        state: &mut Self::State,
        elements: Elements<'_, '_>,
    ) {
        .query(&state)
            .at_position(mouse.pos())
            .each(|el, attributes| {
                attributes.set("background", "red");
            });
    }

```

## Insert state value into attributes

It is possible to assign a value from the state to attributes by taking a
pending value from the state.

A pending value is resolved to an actual value by setting the attribute.

```rust,ignore
#[derive(State)]
struct MyState {
    number: Value<u8>
}

// Component event
fn on_key(
    &mut self,
    key: KeyEvent,
    state: &mut Self::State,
    elements: Elements<'_, '_>,
) { 
    let s = state.as_mut().unwrap();
    let number = s.number.to_pending();
    
    elements
        .query(&state)
        .by_tag("position")
        .each(|el, attrs| {
            attrs.set_pending(number);
        });
}

```
