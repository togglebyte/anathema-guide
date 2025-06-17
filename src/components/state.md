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

#### The following types currently supports `get_as<T>`:

* `i8`
* `i16`
* `i32`
* `i64`
* `isize`
* `u8`
* `u16`
* `u32`
* `u64`
* `usize`
* `&String`
* `Color`
* `Hex`
* `Display`

It's possible to add additional types that can be used as attributes as long as
the type implements `TryFrom<&ValueKind<'_>> for T` and `From<T> for
ValueKind<'_>` where `T` is a custom type.

```rust,ignore
struct MyAttribute(String);

impl TryFrom<&ValueKind<'_>> for MyAttribute {
    type Error = ();

    fn try_from(value: &ValueKind<'_>) -> Result<Self, Self::Error> {
        let Some(s) = value.as_str() else { return Err(()) };
        Ok(MyAttribute(s))
    }
}

impl From<MyAttribute> for ValueKind<'_> {
    fn from(value: MyAttribute) -> Self {
        ValueKind::Str(value.0.into())
    }
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

## Rename fields

To rename fields on the state that should have a different name in the template
decorate the fields with `#[anathema(rename = "new_name")]`.

### Example

```rust
#[derive(State)]
struct MyState {
    #[anathema(rename = "string")]
    chars: Value<String>,
}
```
