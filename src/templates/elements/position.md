# Position (`position`)

Absolute or relative position of the child.

Accepts at most one child.

## Attributes

### `placement`

Either `relative` (default)
or `absolute`

### `left` 

Position the **left** side of the element with an offset of the given value.

### `right` 

Position the **right** side of the element with an offset of the given value.

```
border [width: 10, height: 5]
    position [top: 0, right: 0, placement: "relative"]
        text "Hi"
```
```
┌────────┐
│      Hi│
│        │
│        │
└────────┘
```

### `top` 

Position the **top** side of the element with an offset of the given value.

### `bottom` 

Position the **bottom** side of the element with an offset of the given value.
