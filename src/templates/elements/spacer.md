# Spacer (`spacer`)

A spacer should only be used inside an `hstack` or a `vstack`.

The `spacer` element will push the size value along a given axis to consume the
remaining available space.

A vertical spacer has no horizontal size, and a horizontal spacer has no
vertical size.

The `spacer` accepts no children and has no visible rendering.

## Example

```
border
    hstack
        text "Hi"
        spacer
```
```
┌─────────────────────────┐
│Hi                       │
└─────────────────────────┘
```
without the spacer:
```
border
    hstack
        text "Hi"
```
```
┌──┐
│Hi│
└──┘
```
