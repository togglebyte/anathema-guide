# Padding (`padding`)

Add padding around an element

Padding only accepts a single child.

Specifying only the `padding` attribute will apply the value to all sides.
This can be overridden by specifying a side, such as `bottom`, `right` etc.

```
border
    padding [padding: 1]
        text "What a border!"
```
```
┌────────────────┐
│                │
│ What a border! │
│                │
└────────────────┘
```

## Attributes

### `padding`

Value applied to all sides of the element.
This value will be overridden by any specifics, such as `left` or `top` etc.

### `top`

Top padding

### `right`

Right side padding

### `bottom`

Bottom padding

### `left`

Left side padding
