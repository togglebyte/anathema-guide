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

## Attributes

A component can have state (mutable access) and attributes.

Passing attributes to a component is done the same way as for elements:

```
@my_comp [key: "this is a value"]
```

To use the attributes in the component refer to the keys passed into the component:
```
// component template
text "the value is " attributes.key
```

### Accessing attributes from a component

It's possible to access attributes from within the component, using `context.attributes`.

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

## State

State is anything that implements the `State` trait. It can be accessed in the template using `state.<value>`.

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

runtime.component("my_comp", "path/to/template.aml", MyComponent, my_state);
```

## Ignore fields

To store fields on the state that should be ignored by templates and widgets,
decorate the fields with `#[anathema(ignore)]`.

### Example

```rust
#[derive(State)]
struct MyState {
    word: Value<String>,

    #[anathema(ignore)]
    ignored_value: usize,
}
```
