# With

A `with` makes it possible to scope an expression for nodes

## Example

```
let THEME = [
    { fg: "red", bool: true },
    { fg: "green", bool: false },
    { fg: "blue", bool: true },
];


border
    with theme as colors[state.some_count % 2 == 0]
        // Refering to `theme` instead of repeating `colors[state.some_count % 2 == 0]`
        
        text [foreground: theme.fg] "hello "
            span [bold: theme.bold] "world"

```
