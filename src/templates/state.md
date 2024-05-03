# State

A state, associated with a component, has to be a struct with named fields.

```rust,ignore
#[derive(State)]
struct SomeState {
    some_value: Value<u32>
}
```

The field type has to be wrapped in a `Value<T>` where `T` implements state if
the value should be displayed in the template.
However if the value does not need to be available in the template it's possible
to wrap the value in `anathema::state::Stateless<T>` instead.

This makes it possible to associate values with the state that should never be
available in the template.

**Note** However that it would make more sense to store the value directly on
the component if the value doesn't need to be associated with the state.

Most primitives and `String` implements state and can be used directly with
`Value<T>`.

For collections use `List<T>` and for maps use `Map<T>`.

State can also be nested:

```rust,ignore
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

## Value access in the state

Values can be mutated in the state by using the `to_mut()` function.
This returns a mutable guard that can be dereferenced:

```rust,ignore
let mut value = state.counter.to_mut();
*value += 1;
```

The exception to this is the `Map<T>` and `List<T>` which has functions that
should be called directly on the type. 

Note the absence of `to_mut()` in the following example:

```rust,ignore
state.list.push_back(123);
```

## List

A list is a queue that can be appended or prepended to using `push_back` or
`push_front` respectively, depending on the order the list should be displayed.

**Note** that a `List<T>` internally wraps the elements in `Value<T>` so there
is no need to push `Value<T>` to a `List<T>`.

### Example

```rust,ignore
use anathema::state::List;

#[derive(State)]
struct MyState {
    messages: Value<List<u8>>,
}

impl Component for MyComponent {
    //...
    fn on_key(
        &mut self,
        key: KeyEvent,
        state: Option<&mut Self::State>,
        elements: Elements<'_, '_>,
    ) { 
        state.unwrap().messages.push_back(1);
    }
}
```

The following functions are available on `List<T>`:

* `push_back`
* `push_front`
* `insert`
* `clear`
* `remove`
* `pop_front`
* `for_each`

## Map

A `Map<T>` type where the keys are `Rc<str>`s.

The following functions are available on `Map<T>` without having to call
`to_mut()`:

* `insert`
* `remove`

To access values from the map `to_mut()` is required as it needs exclusive access
to the map for the lifetime of the borrows:

```rust,ignore
use anathema::state::Map;

let mut map = Map::empty();
map.insert("number", 123);

let mut exclusive_map = map.to_mut();
let number = exclusive_map.get_mut("number").unwrap();

let map_ref = map.to_ref();
let number = map_ref.get("number").unwrap();
let number_again = map_ref.get("number").unwrap();
```

## Custom types

A custom type can be a `Value` in a state as long as it implements `anathema::state::State`.

To be able to display a custom value in a template the value has to implement
the `to_common` method of the `State` trait:

```rust, ignore
struct Custom {
    name: String,
}

impl State for Custom {
    fn to_common(&self) -> Option<CommonVal<'_>> {
        Some(CommonVal::Str(&self.name))
    }
    // ...
}
```

If the value should never be displayed it's safe to return `None` in the
`to_common` method, the value will still be available in any `Widget` that
expects it.

A `CommonVal` can be either of these:

* `Bool`
* `Char`
* `Int`
* `Float`
* `Hex`
* `Str`

### Example

```rust,ignore
struct Custom {
    name: String,
}

impl State for Custom {
    fn to_any_ref(&self) -> &dyn std::any::Any {
        self
    }

    fn to_any_mut(&mut self) -> &mut dyn std::any::Any {
        self
    }

    fn to_common(&self) -> Option<CommonVal<'_>> {
        Some(CommonVal::Str(&self.name))
    }
}
```
