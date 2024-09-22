# Expand (`expand`)

Expand the element to fill the remaining space.

Accepts one child.

The layout process works as follows:

First all elements that are not `expand` or `spacer` will be laid out.
The remaining space will be distributed between `expand` then `spacer` in that
order, meaning if one `expand` exists followed by a `spacer` the `expand` will
consume all remaining space, leaving the `spacer` zero sized.

The size is distributed evenly between all `expand`s.

To alter the distribution factor set the `factor` attribute.

## Example

```
border [width: 10, height: 11]
    vstack
        expand
            border
                expand
                    text "top"
        expand
            border
                expand
                    text "bottom"
        text "footer"
```
```
┌────────┐
│┌──────┐│
││top   ││
││      ││
│└──────┘│
│┌──────┐│
││bottom││
││      ││
│└──────┘│
│footer  │
└────────┘
```

## Attributes

### `factor`

The factor decides the amount of space to distribute between the `expand`s.

Given a height of `3` and two `expand` widgets, the height would be divided by
two.

If one of the `expand` widgets had a `factor` of two, then it would receive `2`
of the total height, and the remaining widget would receive `1`.

### `axis`

Expand along an axis.

Valid values:

* `"horz"` | `"horizontal"`
* `"vert"` | `"vertical"`