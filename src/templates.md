# Templates

The template syntax is simple:

```
  Widget name                                         
  |    Start of attributes
  |    |    attribute name                            End of attributes
  |    |    |          attribute value                |
  |    |    |          |     attribute separator      |Terminate widget
  |    |    |          |     |                        || Text (only text widget)
  |    |    |          |     |                        || |
widget [optional: "attribute", separated: "by a comma"]: "text"
```

## Some examples:

A widget with no attributes and two children, showing the data context variable
`{{ name }}` inside a text widget:

```yaml
hstack:
    text: "hello {{ name }}"
    text: "how are you?"
```

A widget with attributes:

```yaml
alignment [align: bottom-right, padding-left: 2, padding-right: 2]:
```

A text widget:

```yaml
text: "Here be text"
```

A text widget with styled sections:
```yaml
text: "a bit of "
    span [foreground: red]: "red "
    span: " and "
    span [bold:true]: "bold"
```
