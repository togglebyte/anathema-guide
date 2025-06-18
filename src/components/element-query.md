# Element and Component query

Both component events and messages provide an element query that can be used to
access elements and attributes of child components.

Attributes can be read and written to.

## Querying elements

There are currently three element queries:

* `by_tag`
* `by_attribute`
* `at_position`

A query can be followed by an additional query, or `first` or `each`.

`first` will stop at the first found element, whereas `each` will call the
closure for each element.

### Example

```rust,ignore
children.elements()
    .by_tag("border")
    .by_attribute("button", true).
    .as_position(pos)
    .each(|element, attributes| {
        // Each element and it's associated attributes
    });
```

### Cast an element

It's possible to cast an element to a specific widget using the `to` method.

The `to` method returns a `&mut T`.

For an immutable reference use `to_ref`.

If the element type is unknown use `try_to` and `try_to_ref` respectively.

#### Example

```rust,ignore

fn on_key(
    &mut self,
    key: KeyEvent,
    state: &mut Self::State,
    mut children: Children<'_, '_>,
    mut context: Context<'_, '_, Self::State>,
) {
    children
        .elements()
        .by_tag("overflow")
        .by_attribute("abc", 123)
        .first(|el, _| {
            let overflow = el.to::<Overflow>();
            overflow.scroll_up();
        });
}
```

### Filter elements

#### Example

```rust,ignore
fn on_mouse(
    &mut self,
    mouse: MouseEvent,
    state: &mut Self::State,
    mut children: Children<'_, '_>,
    mut context: Context<'_, '_, Self::State>,
) {
    children
    .elements()
        .by_attribute("abc", 123)
        .each(|el, attributes| {
            attributes.set("background", "green");
        });
}
```

There are three methods to query the elements inside the component:

#### `by_tag`

This is the element tag name in the template, e.g `text` or `overflow`.

```rust, ignore
children
    .elements()
    .by_tag("text")
    .each(|el, attributes| {
        attributes.set("background", "green");
    });
```

#### `by_attribute`

This is an attribute with a matching value on any element.

```rust, ignore
children
    .elements()
    .by_attribute("background", "green")
    .each(|el, attributes| {
        attributes.set("background", "red");
    });
```

#### `at_position`

```rust, ignore
    fn on_mouse(
        &mut self,
        mouse: MouseEvent,
        state: &mut Self::State,
        mut children: Elements<'_, '_>,
        context: Context<'_, Self::State>,
    ) {
        children
            .elements()
            .at_position(mouse.pos())
            .each(|el, attributes| {
                attributes.set("background", "red");
            });
    }

```

## Setting attribute values

It is possible to assign an attribute to an element using the `set` function on the attributes.

```rust,ignore
// Component event
fn on_key(
    &mut self,
    key: KeyEvent,
    state: &mut Self::State,
    mut children: Children<'_, '_>,
    mut context: Context<'_, '_, Self::State>,
) { 
    children
        .elements()
        .by_tag("position")
        .each(|el, attrs| {
            attrs.set("background", Color::Red);
        });
}
```

## Getting values from attributes

To get values from attributes use the `get_as` function.

E.e `context.attributes.get_as::<&str>("name").unwrap();`

Note that integers in the template can be auto cast to any integer type.

E.g `i64` can be cast to a `u8`.
However integers can not be cast to floats, and floats can not be cast to integers.

[See supported types](./state.html#the-following-types-currently-supports-get_ast)

```rust,ignore
// Component event
fn on_key(
    &mut self,
    key: KeyEvent,
    state: &mut Self::State,
    mut children: Children<'_, '_>,
    mut context: Context<'_, '_, Self::State>,
) { 
    children.elements()
        .by_tag("position")
        .each(|el, attrs| {
            let boolean = attrs.get_as::<bool>("is_true").unwrap();
            let string = attrs.get("background").unwrap().as_str().unwrap();
        });
}
```

## Querying components

There are currently four component queries:

* `by_name`
* `by_id`
* `by_attribute`
* `at_position`

```rust,ignore
// Component event
fn on_key(
    &mut self,
    key: KeyEvent,
    state: &mut Self::State,
    mut children: Children<'_, '_>,
    mut context: Context<'_, '_, Self::State>,
) { 
    children
    .component()
        .by_name("my_component")
        .each(|id, comp, attrs| {
            let boolean = attrs.get_as::<bool>("is_true").unwrap();
            let string = attrs.get("background").unwrap().as_str().unwrap();
        });
}
```
