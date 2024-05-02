# Text (`text`)

Print text.

To add styles to text in the middle of a string use a `span` widget.

## Example

```
text [foreground: "red"] "I'm a little sausage"
```

## Attributes

### `wrap`

Default is to wrap on word boundaries such as space and hyphen, and this method
of wrapping will be used if no `wrap` attribute is given.

Valid values:
* `"overflow"`: the text is truncated when it can no longer fit
* `"break"`: the text will wrap once it can no longer fit

### `text-align`

Note that text align will align the text within the text widget.
The text widget will size it self according to its constraint.

To right align a text to the right side of the screen therefore requires the use
of the alignment widget in combination with the text align attribute.

Default: `left`

Valid values:
* `"left"`
* `"right"`
* `"centre"` | `"center"`

Example of right aligned text
```
border [width: 5 + 2]
    text [text-align: "right"] "hello you"
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
    text [text-align: "centre"] "hello you"
```

```
┌─────┐
│hello│
│ you │
└─────┘
```

### Additional styles

* `bold`
* `italic`

```
text [bold: true, italic: true] "bold AND italic?!"
```
