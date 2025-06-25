# Either

The Either operator: `?` can be used to provide fallback values.

Only dynamic values (attributes and state) are subject to truthiness
checks; Static values (defined using `let` in a template) are never (even if they are false).

```
       Does this exist?
       and is it truthy?
         /  \
        /    \
       yes   no -------+
        |              |
        V              V
text state.value ? "default"
```

## Example

This will always use the static value, and print `false`:

```
text  false ? "hello"
```
However
```
text  state.maybe_false ? "hello"
```

Would print `hello` **if** `maybe_false` is any of the values in the [fallback table](./either.md#fallback-table):

### Fallback table

These values causes fallback.

* `null`
* `0`
* `""`
* `[]`
* `{}`
* `false`

This means that any state or attribute value that resolves to a value listed
above will use the fallback.
