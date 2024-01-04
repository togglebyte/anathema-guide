# State

There are two types of state:
* Internal
* External 
 
## External state

External state is state that is passed to the view from the outside.

A state can either be a direct state from a parent view:
```
@chid parent_state
```
or a custom map declared in the template:
```
@myview {"bunnies": list_of_bunnies}
```

## Internal state

Internal state is provided by the `View`, by implementing the `fn state(&self) -> &dyn State` method from the `View` trait.

The state is owned by the view, and the view has mutable access to the state.

## Example

```rust
#[derive(Debug, State)]
struct MyState {
    name: StateValue<String>,
    guests: List<String>,
}
```

