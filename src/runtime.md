# Runtime and backend

```rust
use anathema::runtime::Runtime;
use anathema::backend::tui::TuiBackend;

let backend = TuiBackend::builder()
    .enable_alt_screen()
    .enable_raw_mode()
    .enable_mouse()
    .hide_cursor()
    .finish()
    .unwrap();
    
let mut runtime = Runtime::new("border").unwrap();
runtime.fps = 30; // default
```

## Configuring the backend

The following settings are available:

### `enable_raw_mode`

Raw mode prevents inputs from being forwarded to the screen (the event system
will pick them up but the terminal will not try to print them).

### `enable_mouse`

Enable mouse support in the terminal (if it's supported).

### `enable_alt_screen`

Creates an alternate screen for rendering. 
Restores the previous content of the terminal to its previous state 
when the runtime ends.

### `hide_cursor`

Hides the cursor.

### Quit on Ctrl-c

This is enabled by default on the TUI backend.
To disable this set `backend.quit_on_ctrl_c = false`.

## Configuring the runtime

### `fps`

Default: `30`

The number of frames to (try to) render per second.

