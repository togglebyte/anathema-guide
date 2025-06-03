# Text (`text`)

Displays text.

String literals can be wrapped in either `"` or `'`.

To add styles to text in the middle of a string use a `span` element as the 
`text` element only accepts `span`s as children. Any other element will be
ignored. 

## Example

```
text [foreground: "red"] "I'm a little sausage"
```

## Attributes

### `wrap`

Default is to wrap on word boundaries such as space and hyphen, and this method
of wrapping will be used if no `wrap` attribute is given.

Valid values:
* `"break"`: the text will wrap once it can no longer fit

### `text_align`

Note that text align will align the text within the element.
The text element will size it self according to its constraint.

To right align text to the right side of the screen therefore requires the use
of the alignment widget in combination with the text align attribute.

Default: `left`

Valid values:
* `"left"`
* `"right"`
* `"centre"` | `"center"`

Example of right aligned text
```
border [width: 5 + 2]
    text [text_align: "right"] "hello you"
```

```
┌─────┐
│hello│
│  you│
└─────┘
```

Example of centre aligned text
```
border [width: 5 + 2]
    text [text_align: "centre"] "hello you"
```

```
┌─────┐
│hello│
│ you │
└─────┘
```
