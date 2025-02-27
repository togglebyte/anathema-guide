# Element query

Both component events and messages provide an element query that can be used to
access elements and attributes of the component.

Attributes can be read and written to.

## Cast an element

It's possible to cast an element to a specific widget using the `to` method.

The `to` method returns a `&mut T`.

For an immutable reference use `to_ref`.

If the element type is unknown use `try_to` and `try_to_ref` respectively.

### Example

```rust,ignore

fn on_key(
    &mut self,
    key: KeyEvent,
    state: &mut Self::State,
    mut elements: Children<'_, '_>,
    mut context: Context<'_, '_, Self::State>,
) {
    elements
        .elements()
        .by_tag("overflow")
        .by_attribute("abc", 123)
        .first(|el, _| {
            let overflow = el.to::<Overflow>();
            overflow.scroll_up();
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
    mut elements: Children<'_, '_>,
    mut context: Context<'_, '_, Self::State>,
) {
    elements
        .by_attribute("abc", 123)
        .each(|el, attributes| {
            attributes.set("background", "green");
        });
}
```

There are three methods to query the elements inside the component:

### `by_tag`

This is the element tag name in the template, e.g `text` or `overflow`.

```rust, ignore
elements
    .elements()
    .by_tag("text")
    .each(|el, attributes| {
        attributes.set("background", "green");
    });
```

### `by_attribute`

This is an attribute with a matching value on any element.

```rust, ignore
elements
    .elements()
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
        context: Context<'_, Self::State>,
    ) {
        elements
            .elements()
            .at_position(mouse.pos())
            .each(|el, attributes| {
                attributes.set("background", "red");
            });
    }

```

## Insert state value into attributes

It is possible to assign a value to an element using the `set` function on the
attributes.

```rust,ignore
// Component event
fn on_key(
    &mut self,
    mouse: MouseEvent,
    state: &mut Self::State,
    mut elements: Children<'_, '_>,
    mut context: Context<'_, '_, Self::State>,
) { 
    elements
        .elements()
        .by_tag("position")
        .each(|el, attrs| {
            attrs.set("background", "red");
        });
}
```

## Getting values from attributes

To read copy values from attributes use the `get` function.
To get a reference (like a string slice) use `get_ref`.

Note that integers in the template can be auto cast to any integer type as long
as the value is hard coded into the template (and not originating from state).

E.g `i64` can be cast to a `u8`.
However integers can not be cast to floats, and floats can not be cast to
integers.

```rust,ignore
// Component event
fn on_key(
    &mut self,
    key: KeyEvent,
    state: &mut Self::State,
    elements: Elements<'_, '_>,
) { 
    elements
        .by_tag("position")
        .each(|el, attrs| {
            let boolean = attrs.get_as::<bool>("is_true").unwrap();
            let string = attrs.get("background").unwrap().as_str().unwrap();
        });
}
```
