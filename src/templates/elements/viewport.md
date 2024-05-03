# Viewport (`viewport`)

The viewport allows the elements to overflow in along a given axis.

It's important to note that the `viewport` is an unbounded element.
This means that elements can be laid out infinitely along a given axis.

Providing the viewport with a very large collection would produce a very large element tree.

A `Viewport` offset has to be changed via events rather than template values (if
the offset came from a `State` it wouldn't be possible to know when the offset
was exceeded should it change as a result of a key press. Even though the
viewport could clamp the offset, the offset it self would not be clamped).

## Example

```
border [height: 4, width: 10]
    viewport
        text "1"
        text "2"
        text "3"
        text "4"
```
```
┌────────┐
│1       │
│2       │
└────────┘
```

```rust,ignore
struct MyComponent {
    // ...
    fn on_key(
        &mut self,
        key: KeyEvent,
        state: Option<&mut Self::State>,
        mut elements: Elements<'_, '_>,
    ) {
        if let Some(c) = key.get_char() {
            elements
                .query(state)
                .by_tag("viewport")
                .first(|el| {
                    let (viewport, _) = el.to_inner_mut::<Viewport>().unwrap();
                    match c {
                        'k' => viewport.scroll_up(),
                        'j' => viewport.scroll_down(),
                        _ => {}
                    }
                });
        }
    }
}
```

## Attributes

### `direction`

Specifies the direction to lay out the elements.

Default value: `"forwards"`

Valid values:
* `"back"` or `"backwards"` or `"backward"`
* `"fwd"` or `"forwards"` or `"forward"`

```
border [height: 5, width: 10]
    viewport [direction: "backward"]
        text "1"
        text "2"
```
```
┌────────┐
│        │
│2       │
│1       │
└────────┘
```

### `axis`

Specify along which axis to layout the children.

Valid values:

* `"horz"` | `"horizontal"`
* `"vert"` | `"vertical"`
