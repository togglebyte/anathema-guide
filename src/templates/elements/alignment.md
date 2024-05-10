# Alignment (`align`)

Alignment will inflate the wrapping element to use all the constraints.

This means it's not recommended to put an alignment widget inside an
unconstrained widget such as a `Viewport`.

The alignment accepts at most one child.

## Example

```
border [width: 16, height: 5]
    align [alignment: "centre"]
        text "centre"
```
```
┌──────────────┐
│              │
│    centre    │
│              │
└──────────────┘
```

## Attributes

### `alignment`

Valid values:
* `"top-left"`
* `"top"`
* `"top-right"`
* `"right"`
* `"bottom-right"`
* `"bottom"`
* `"bottom-left"`
* `"left"`
* `"centre"` or `"center"`
