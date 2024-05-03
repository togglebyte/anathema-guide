# Alignment (`alignment`)

Alignment will inflate the wrapping element to use all the constraints.

The alignment accepts at most one child.

## Example

```
border [width: 16, height: 5]
    alignment [align: "centre"]
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

### `align`

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
