# Viewport (`viewport`)

The viewport allows the widgets to overflow in a given direction.

It's important to note that the `viewport` is an unbounded widget.
This means that widgets can be laid out indefinitely along a given axis.

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

## Attributes

### `direction`

Specifies the direction to lay out the widgets.

Default value: `"forwards"`

Valid values:
* `"backwards"` or `"backward"`
* `"forwards"` or `"forward"`

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

Specify along which axis to layout the widgets.

Valid values:

* `"horz"` | `"horizontal"`
* `"vert"` | `"vertical"`

## Example
