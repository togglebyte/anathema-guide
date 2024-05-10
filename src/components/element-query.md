# Element query

Both component events and messages provide an element query that can be used to
access elements and attributes in the component.

Attributes can be read or written to.
Since attribute values may originate from the component state it is not possible
to access the state and the attributes at the same time. 

For this reason the element query prevents any use of state via the `query`
function:

## Example

```rust,ignore
fn on_mouse(
    &mut self,
    mouse: MouseEvent,
    state: Option<&mut Self::State>,
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
        state: Option<&mut Self::State>,
        elements: Elements<'_, '_>,
    ) {
        .query(&state)
            .by_position(mouse.pos())
            .each(|el, attributes| {
                attributes.set("background", "red");
            });
    }

```

## TODO explain how to insert state values into attributes 
