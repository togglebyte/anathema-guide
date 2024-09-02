# Overflow (`overflow`)

The `Overflow` allows the elements to overflow along a given axis.

It's important to note that the `overflow` is an unbounded element.
This means that elements can be laid out infinitely along a given axis.

Providing the `overflow` with a very large collection would produce a very large element tree.

An `Overflow` offset has to be changed via events rather than template values (if
the offset came from a `State` it wouldn't be possible to know when the offset
was exceeded should it change as a result of a key press for example. Even though the
`Offset` could clamp the offset, the offset it self would not be clamped).

## Example

```
border [height: 4, width: 10]
    overflow
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
        state: &mut Self::State,
        mut elements: Elements<'_, '_>,
        mut context: Context<'_, Self::State>,
    ) {
        if let Some(c) = key.get_char() {
            elements.by_tag("overflow").first(|el, _| {
                let overflow = el.to::<Overflow>();
                match c {
                    'k' => overflow.scroll_up(),
                    'j' => overflow.scroll_down(),
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
    overflow [direction: "backward"]
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

### `clamp`

Clamp the offset, preventing the content to scroll out of view

Default value: `true`

### `unconstrained`

If this is set to true, both axis are unconstrained.
If this is false, only the given axis is unconstrained.

Default value: `false`


* `false`

## Methods

### `scroll_up()`

Scroll the `Overflow` forward.

This is analogous to `Overflow::scroll(Direction::Forward, 1)`.

### `scroll_up_by(n: u32)`

Scroll the `Overflow` forward `n` number of lines.

This is analogous to `Overflow::scroll(Direction::Forward, n)`.

### `scroll_down()`

Scroll the `overflow` backward.

This is analogous to `Overflow::scroll(Direction::Backward, 1)`.

### `scroll_down_by(n: u32)`

Scroll the `overflow` backward `n` number of lines.

This is analogous to `Overflow::scroll(Direction::Backward, n)`.

### `scroll_right()`

Scroll the `overflow` forward.

This is analogous to `Overflow::scroll(Direction::Forward, 1)`.

### `scroll_right_by(n: u32)`

Scroll the `overflow` forward `n` number of lines.

This is analogous to `Overflow::scroll(Direction::Forward, n)`.

### `scroll_left()`

Scroll the `overflow` backward.

This is analogous to `Overflow::scroll(Direction::Backward, 1)`.

### `scroll_left_by(n: u32)`

Scroll the `overflow` backward `n` number of lines.

This is analogous to `Overflow::scroll(Direction::Backward, n)`.

### `scroll_to(pos: Pos)`

Scroll the `overflow` to a given position.
