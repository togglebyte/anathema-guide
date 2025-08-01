## Loops

It's possible to loop over the elements of a collection.
The syntax for loops are `for <element> in <collection>`.

Example: looping over values
```
for value in [1, 2, "hello", 3]
    text value
```

Elements generated by the loop can be thought of as belonging to the parent
element as the loop it self is not a element.
See example below.

Example: mixing loops and elements
```
vstack
    text "start"
    for val in [1, 2, 3]
        text "some value: " val "."
    text "end"
```
The above example would render:
```
start
some value: 1.
some value: 2.
some value: 3.
end
```

For-loops also expose a `loop` variable that is set to the current loop iteration.

Example: `loop` variable
```
vstack
    for val in ["a", "b", "c", "d"]
        text "#" loop ": " val
```
The above example would render:
```
#0: a
#1: b
#2: c
#3: d
```

**Note**: For-loops do not work inside attributes, and can only produce
elements.
