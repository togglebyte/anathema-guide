# Elements

Even though it's possible to create custom elements Anathema aims to provide
the necessary building blocks to represent almost any layout without the need
to do so.

## Default attributes

Elements that draw them selves (e.g `text`, `span` and `border`) support these default
attributes:

### `foreground` 

Foreground colour

Valid values:
* hex: `#ffaabb`
* string: "green"

### `background` 

Background colour (see `foreground` for valid values)

### `bold`

Valid values:
`true` or `false`

### `italic`

Valid values:
`true` or `false`

## Default widgets

The following is a list of available widgets and their template tags:

- [Text](./elements/text.md) (template tag: `text`)
- [Span](./elements/span.md) (template tag: `span`)
- [Border](./elements/border.md) (template tag: `border`)
- [Alignment](./elements/alignment.md) (template tag: `alignment`)
- [VStack](./elements/vstack.md) (template tag: `vstack`)
- [HStack](./elements/hstack.md) (template tag: `hstack`)
- [ZStack](./elements/zstack.md) (template tag: `zstack`)
- [Expand](./elements/expand.md) (template tag: `expand`)
- [Spacer](./elements/spacer.md) (template tag: `spacer`)
- [Position](./elements/position.md) (template tag: `position`)
- [Viewport](./elements/viewport.md) (template tag: `viewport`)
- [Canvas](./elements/canvas.md) (template tag: `canvas`)
- [Container](./elements/container.md) (template tag: `container`)
