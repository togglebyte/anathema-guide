# State

* Stateless values
    * How to use them in templates
* How changing state affects the template

A state, associated with a component, has to be a struct with named fields.

```rust
#[derive(State)]
struct SomeState {
    some_value: Value<u32>
}
```

The field type has to be wrapped in a `Value<T>` where `T` implements state.

Most primitives and `String` implements state and can be used directly with
`Value<T>`.

For collections use `List<T>` and for maps use `Map<T>`.

State can also be nested:

```rust
#[derive(State)]
struct SomeState {
    some_value: Value<u32>,
    inner: Value<InnerState>,
}

#[derive(State)]
struct InnerState {
    counter: Value<usize>,
}
```

## List

## Map
