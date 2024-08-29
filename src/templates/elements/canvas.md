# Canvas (`canvas`)

A canvas is an element that can be drawn on.

The canvas accepts no children.

The canvas should be manipulated in Rust code:

The element will consume all available space and expand to fit inside the
parent.

## Example

```rust
fn on_key(
    &mut self,
    key: KeyEvent,
    state: &mut Self::State,
    elements: Elements<'_, '_>,
) {
    elements.query(&state).by_tag("canvas").first(|el, _| {
        let canvas = el.to::<Canvas>().unwrap();
        let mut cell_attribs = CanvasAttribs::new();
        cell_attribs.set_str("foreground", "red");
        canvas.put('a', cell_attribs.clone(), (0, 0));
        canvas.put('b', cell_attribs.clone(), (1, 1));
        canvas.put('c', cell_attribs.clone(), (2, 2));
    });
}
```

```
border [width: 16, height: 5]
    canvas
```
```
┌──────────────┐
│ a            │
│  b           │
│   c          │
└──────────────┘
```

## Attributes

### `width`

### `height`

## Methods

### `erase(pos: impl Into<LocalPos>)`

Erase a character at a given position in local canvas space

### `get(pos: impl Into<LocalPos>) -> Option<(&mut char, &mut CanvasAttribs)>`

Get a mutable reference to the character and attributes at a given position.

### `put(c: char, style: Style, pos: LocalPos)`

Put a character with a style at a given position.

### `translate(pos: Pos) -> LocalPos`

Translate global coords to local (to the canvas) coords.
