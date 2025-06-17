# Backend

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

### `clear`

Clears the screen. Useful when not enabling an alt screen (which will be cleared
by default).
