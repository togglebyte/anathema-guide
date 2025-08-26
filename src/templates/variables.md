# Variables

Template variables are closer to constants than variables, since they can not be reassigned.

There are two kinds: 
* Global
* Local

A local value will always shadow a global value with the same name, and `with` / `for` will always shadow local values.

## Global

Globals are defined using the `global` keyword: `global <ident> = <expression>`
and they are global across **all** templates.

Once a global is defined it can not be re-defined.
Trying to define a global twice will result in an error.

### Example

```
global value = 1
text value
```

### Notes 

Global values can be defined anywhere in the template, but it's best practice to
place them at the to of the template.

## Local

<small><i>The old `let` keyword is now an alias for `local`.</i></small>

Locals are defined using the `local` keyword: `local <ident> = <expression>`.

A `local` value is never accessible outside the current component, and is scoped
to the parent element.

### Example

```
local value = 1
text value
```

### Notes

Unlike `global`s the placement within the template is relevant for `local`s.

The following will display `1`:
```
local x = 1
vstack
    text x
```

The following template

```
local x = 1
vstack
    text x
    vstack
        local x = 2
        text x
    text x
```

will display

```
1
2
1
```

