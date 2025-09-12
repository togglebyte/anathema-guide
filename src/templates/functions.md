# Functions

AML have a few functions built-in and it's possible to add custom functions.

A function should called as a free function: `to_upper("string")`

## `truncate(string, number)`

Truncate a string if it's longer than `number`.

### Example

```
text "hello world".truncate(5)
```

Will output: "hello".

## `to_upper(string)`

Convert a string to upper case.

### Example

```
text "It's teatime".to_upper()
```

Will output: "IT'S TEATIME".

## `to_lower(string)`

Convert a string to lower case.

### Example

```
text "It's teatime".to_lower()
```

Will output: "it's teatime".

## `to_str(arg)`

Convert any value to a string representation.

### Example

```
let greetings = {
    "1": "Hello",
    "2": "Hi",
}

text greetings[to_str(2)]
```

Will output: "Hi".

## `to_int(arg)`

Try to convert any value to an integer.

Boolean `true` will convert to `1`, and `false` will convert to `0`.

Floating point values will be truncated. 
`to_int(1.99999)` will be converted to `1`.

### Example

```
let greetings = [
    "Hello",
    "Hi",
]

text greetings[to_int(true)]
```

Will output: "Hi".

## `to_float(arg)`

Try to convert any value to a floating point number.

Boolean `true` will convert to `1.0`, and `false` will convert to `0.0`.

### Example

```
text to_float(123)
```

Will output: "123".

## `round(number, decimal_count=0)`

Round a floating point value. 
Trying to round any other type will return null.

If no `decimal_count` is provided `0` is the default.

### Example

```
vstack
    text round(1.1234, 2)
    text round(1.1234)
```

Will output: 
```
1.12
1
```

This function will pad with zeroes:
```
    text round(1.12, 5)
```

Will output: `1.12000`

## `contains(haystack, needle)`

Returns true if the haystack contains the needle.

### Example

```
vstack
    text contains([1, 2, 3], 2)
    text "hello world".contains("lo")
```

Will output: 
```
true
true
```

## `width(arg)`

Returns the width of the input

```
text width(111)
```
Will output: `3`

## `pad(arg, 5)`

Returns a string padded with spaces

```
border
    text pad("hi", 4)
```

Will output:
```
┌────┐
│hi  │
└────┘
```

## `len(arg)`

Returns the length of a collection or a string

```
text len(0..2)
```
Will output: `3`

