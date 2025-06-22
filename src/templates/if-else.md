## If / Else

It's possible to use `if` and `else` to determine what elements to render.

Example:
```
if true
    text "It's true!"
    
if value > 10
    text "Larger than ten"
else if value > 5
    text "Larger than five but less than ten"
else
    text "It's a small value..."
```

### Conditional theming

Changing colors and style depending on a condition can be achieved by declaring
a constant. All constants are global.

```
let THEME = {
    enabled: { bg: "grey" },
    disabled: { bg: "reset" },
}

text [background: THEME["disabled"].bg] "Hello"
```

Since boolean values can translate to numbers it's also possible to use
them as index lookups.

```
let THEME = [
    // False
    { bg: "grey" },
    // True
    { bg: "reset" },
]

text [background: THEME[state.some_bool].bg] "Hello, tea"
```

### Operators

* Equality `==` 
* Greater than `>`
* Greater than or equals `>=`
* Less than `<`
* Less than or equals `<=`
* And `&&`
* Or `||`
* Addition `+`
* Subtraction `-`
* Multiplication `*`
* Division `/`
* Remainder (Modulo) `%`
* Negation `!`
* Either `?` (Uses the right value if the left value does not exist)

Note: just like for-loops it's not possible to use if / else with attributes.

To determine the value of an attribute depending on some condition this should
be handled by the state.

