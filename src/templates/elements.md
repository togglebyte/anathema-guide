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

The following is a list of available widgets and their template names:

- [Text](./elements/text.md) (template name: `text`)
- [Span](./elements/span.md) (template name: `span`)
- [Border](./elements/border.md) (template name: `border`)
- [Alignment](./elements/alignment.md) (template name: `alignment`)
- [VStack](./elements/vstack.md) (template name: `vstack`)
- [HStack](./elements/hstack.md) (template name: `hstack`)
- [ZStack](./elements/zstack.md) (template name: `zstack`)
- [Expand](./elements/expand.md) (template name: `expand`)
- [Spacer](./elements/spacer.md) (template name: `spacer`)
- [Position](./elements/position.md) (template name: `position`)
- [Viewport](./elements/viewport.md) (template name: `viewport`)
