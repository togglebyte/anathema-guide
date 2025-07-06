# Variables

Template variables are closer to constants than variables, since they can not be reassigned.

There are two kinds: 
* Global
* Local

A local value will always shadow a global value with the same name, and `with` / `for` will always shadow local values.

## Global

Globals are defined using the `global` keyword: `global <ident> = <expression>`.

Once a global is defined it can not be re-defined.

### Example

```
global value = 1
text value
```

### Notes 

Global values can be defined anywhere in the template, but it's best practice to
place them at the to of the template.

## Local

Locals are defined using the `local` keyword: `local <ident> = <expression>`.
The old `let` keyword is now an alias for `local`.

A `local` value is only scoped to the component 

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

The following will display:

```
1
2
```

```
local x = 1
vstack
    text x
    local x = 2
    text x
```

