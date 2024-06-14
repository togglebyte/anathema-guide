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

## Custom fields

Any type can be added as a field to a state as long as it implements `anathema::state::State`.

The only required function to implement is `fn to_common(&self) -> Option<CommonVal<'_>>` 
(without this the template has no way to render the value).

Types from third party crates can be wrapped using the new-type pattern.

### Example

```rust
use anathema::component::*;
use anathema::state::CommonVal;

struct MyValue {
    a: i64,
    b: i64,
}

impl State for MyValue {
    fn to_common(&self) -> Option<CommonVal<'_>> {
        let total = self.a + self.b;
        Some(CommonVal::Int(total))
    }
}

#[derive(State)]
pub(super) struct MyState {
    my_val: Value<MyValue>,
}
```
