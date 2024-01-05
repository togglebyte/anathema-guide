# Widgets

Even though it's possible to create your own widgets Anathema aims to provide
the necessary building blocks to represent almost any layout.

Widgets that draw them selves (e.g `text` and `border`) support these default
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

- [Text](./widgets/text.md) (template name: `text`)
- [Border](./widgets/border.md) (template name: `border`)
