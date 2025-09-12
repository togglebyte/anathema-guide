# Attributes
 
Both components and elements have attributes.

These can be accessed through events.

## Example

```rust
fn on_key(
    &mut self,
    key: KeyEvent,
    state: &mut Self::State,
    mut children: Children<'_, '_>,
    mut context: Context<'_, '_, Self::State>,
) { 
    // Components attributes
    context.attributes.get_as::<u32>("int").unwrap();
    
    // Element attributes
    children.elements().by_tag("text").first(|el, attr| {
        attr.get_as::<bool>("is_true").unwrap();
    });

}
```

## Attribute functions

These functions are available on attribute collections for both components and
elements:


### `get_as<T>(&str)`

Get a value as a given type.

```
text [my_attribute: 123, is_true: false] "..."
```

```rust
attributes.get_as::<u32>("my_attribute").unwrap();
attributes.get_as::<u8>("my_attribute").unwrap();
attributes.get_as::<bool>("is_true").unwrap();
```

### `get(&str)`

Get a value as a
[`ValueKind`](https://docs.rs/anathema-value-resolver/latest/anathema_value_resolver/enum.ValueKind.html#variants).
This is only useful if the value type of the attribute can change or is unknown.


### `set(&str, impl Into<ValueKind>)`

Set an attribute value.

```rust
attributes.set("key", 123).unwrap();
attributes.set("other_key", false).unwrap();
```

### `remove(&str)`

Remove an attribute value.

```rust
attributes.remove("key");
```

### `iter_as::<T>(&str)`

Get an iterator over values of an attribute

```
@my_component [list: [1, 2, 3]]
```

```rust
let mut iter = attributes.iter_as::<u8>("list");
for val in iter {
    // val is a u8
}
```

### `value_as<T>(&str)`

Get the value of an element / component as a `T`.

```rust
attributes.value_as::<&str>().unwrap();
```

### `value(&str)`

Get the value of an element / component.


### `set_value(&str, value)`

Set the value of an element / component.

```rust
attributes.set_value(123);
```

