# State

A state is a struct with named fields. 

Each field has to be wrapped in a `Value`, e.g 
```rust
name: Value<String>
``` 
or 
```rust
names: Value<List<String>>
```

## Updating state

There are two ways to update a field on `State`.

1. `state.field.set(new_value)`
2. `*state.field.to_mut() = new_value`
 
<div class="warning">
<h4>Important note about state values</h4>

When assigning a new value to a state, do **not** create a new instance of
Value&lt;T&gt;.

This is bad:
```rust,ignore
    my_state.value = Value::new(123)
```

This is how it should be done:

```rust,ignore
    my_state.some_value.set(123);
    // or
    *my_state.another_value.to_mut() = 123;
```

</div>

## External state

A component can have internal state (mutable access) and external state
(read-only).

To pass external state to a component provide a map in the template
with a key-value pair:

```
@my_comp [key: "this is a value"]
```

To use the external state in the component refer to the keys in the map passed into the
component:
```
// component template
text "the value is " attributes.key
```

### Accessing external state from a component

It's possible to access external state from within the component, using
`context.attributes`.

### Example

```
@comp [key: 123]
```

```rust
fn on_key(
    &mut self,
    key: KeyEvent,
    state: &mut Self::State,
    mut elements: Elements<'_, '_>,
    mut context: Context<'_, Self::State>,
) { 
    let v: i64 = context.attributes.get_as::<i64>("key").unwrap();
}
```

## Internal state

Internal state is anything that implements the `State` trait. It can be accessed in the template using `state.<value>`.
If the state is an enum, you can compare `state` to a string matching the variants name to check if the variant is active.

### Example 

```rust,ignore
use anathema::state::{State, Value, List};

#[derive(State)]
struct MyState {
    name: Value<String>,
    numbers: Value<List<usize>>,
}

impl MyState {
    pub fn new() -> Self {
        Self {
            name: String::from("Lilly").into(),
            numbers: List::empty(),
        }
    }
}

let mut my_state = MyState::new();
my_state.numbers.push_back(1);
my_state.numbers.push_back(2);

runtime.component("my_comp", template, MyComponent, my_state);
```

## Ignore fields

To store fields on the state that should be ignored by templates and widgets,
decorate the fields with `#[state_ignore]`.

### Example

```rust
#[derive(State)]
struct MyState {
    word: Value<String>,

    #[state_ignore]
    ignored_value: usize,
}
```
