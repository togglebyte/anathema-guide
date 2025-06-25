# Templates

Anathema has a template language to describe user interfaces.
This makes it possible to ship the template(s) together with the compiled binary so the
application can be customised without having to be recompiled.

Do note that AML (Anathema Markup Language) is **not** a programming language,
therefore control flow is limited.

The element syntax looks as follows: `<element> <attributes> <value>`

```
   Element name                                         
   |    Start of (optional) attributes
   |    |    Attribute name                            
   |    |    |          Attribute value
   |    |    |          |     Attribute separator      End of attributes
   |    |    |          |     |                        | Values / Text
   |    |    |          |     |                        | |
   |    |    |          |     |                        | |------+-+-+
   |    |    |          |     |                        | |      | | |
element [optional: "attribute", separated: "by a comma"] "text" 1 2 ident
```

Elements don't receive events (only `Component`s do) and should be thought of as
something that is only drawn to screen.

There is for instance no "input" element.
To represent an input the component would handle the key press events and 
simply pass the collected values as a string via state.

## Comments

Use `//` to add comments to a template:
```
    text "I will render"
    // text "I will not"
```

## Attributes

Element attributes are optional.

Attributes consists of a collection of `ident`, `:`, and `value`.

An `ident` has to start with a letter (`a-zA-Z`) and can contain one or more `_`.

Example:
```
element [attribute_a: "some string", attribute_b: false]
```

An attribute name is always an ident, however the value can be anything that
implements `State`.

## Default value types:

A literal value can be one of the following:

* string:   `"Empty vessel, under the sun"`
* integer:  `123`
* float:    `1.23`
* hex:      `#fab` or `#ffaabb`
* boolean:  `true` or `false`
* list:     `[1, 2, 3]`
* map:      `{"key": "value"}`

To access a state or attribute value, use an `ident` (a string without spaces that is not
wrapped in quotes). Note that state values are under the `state` variable and attribute values are under the `attributes` variable.

```rust, ignore
text "string literal"
text "`ident` in the component state: " state.ident
text "`ident` in the component attributes: " attributes.ident
```

## Elements and children

Some elements can have one or more children.

Example: a border element with a text element inside

```
border
    text "look, a border"
```

* [Loops](./templates/loops.md)
* [If / Else](./templates/if-else.md)
* [Switch / Case](./templates/switch-case.md)

## Either

The `?` symbol can be used to have a fallback value.

See [Either](./templates/either.md) for specifics on how this works as the
behaviour differs between static values (defined in templates) and dynamic values (such as attributes and states).

```
text state.maybe_value ? attributes.maybe_this ? "there was no value"
```

## Constants / Globals

Constants are global and regardless of where in a template they are declared
they will be hoisted to the top.

### Example

The following example will print `1`
```
text glob

let glob = 1
```

The following example will print `1` then `2`, as the `inner` component will override the
outer components global.
```
// main.aml
let glob = 1
vstack
    text glob
    @inner

// inner.aml
let glob = 2
text glob
```

The following example will print `2`, as the `inner` component will override the
outer components global.
```
// main.aml
let glob = glob
@inner

// inner.aml
let glob = 2
text glob
```
