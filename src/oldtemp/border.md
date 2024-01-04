# Border

```yaml
border [border-style: thick]
    text: "That's one thick border!"
```

## Attributes

### `border-style`

Border style can be one of the following values:

#### `thin`
```
┌───────────────────────────┐
│Would you like some coffee?│
└───────────────────────────┘
```

#### `thick`
```
╔══════════╗
║I like tea║
╚══════════╝
```

#### A custom value as a string where each character represents an edge: 
```
border [border-style: "01234567", padding: 1]:
    text: "hi"
```
output
```
011112
7    3
7 hi 3
7    3
655554
```
### `width`
If the width is less than the maximum width constraint the border
will size it self according to the width.

```yaml
border [border-style: thin, width: 20]:
    text: "hi"
```
output
```
┌──────────────────┐
│hi                │
└──────────────────┘
```

### `height`

Height works like width, in that it will size it self according to the height as
long as it fits.

### `sides`

Determines which sides of the border should be drawn.
If the sides are joined, e.g top and left, then the top left corner will be
drawn as well.

```yaml
border [border-style: thin, width: 10, sides: top|bottom]:
    text: "hi"
```
output
```
──────────
hi
──────────
```

```yaml
border [border-style: thin, width: 10, sides: top|right]:
    text: "hi"
```
output
```
─────────┐
hi       │
```

{{#include padding.md}}

{{#include min-height-width.md}}
