# Adding custom functions

It's possible to add custom functions to the runtime.

All function have the same signature in Rust:

```rust, ignore
fn contains<'a>(args: &[ValueKind<'a>]) -> ValueKind<'a> {
    ... 
}
```

## Example

A function that adds two values

```rust, ignore
use anathema::resolver::ValueKind;

fn add<'a>(args: &[ValueKind<'a>]) -> ValueKind<'a> {
    if args.len() != 2 {
        return ValueKind::Null;
    }
    
    let args = args[0].to_int().zip(args[1].to_int());
    match args {
        Some((lhs, rhs)) => ValueKind::Int(lhs + rhs),
        None => ValueKind::Null
    }
}
```

## Adding the function to the runtime

Use the `register_function` method on the runtime to add your custom function.

It is not possible to overwrite existing functions, and trying to do so will
return an error.

### Example 

```rust,ignore
builder.register_function("add", add).unwrap();
```
In the template:
```
text add(1, 2)
```
