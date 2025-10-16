# Actions

The `actions` section of the configuration table allows you to configure any
number of "actions," which are arbitrary Lua functions that get executed when
a key or button combo is pressed.

## Default values

```lua
local config = {
    actions = {},
}

return config
```

## Configuration

The `actions` table should contain a list of key-value pairs where each key is
a string describing the input (containing a key or button and any number of
modifiers) and the value is a function to be executed when the input is
received. For example:

```lua
local config = {
    actions = {
        -- This will run if you press T with no modifier keys held.
        ["T"] = function() end,

        -- This will run if you press T with only Shift held.
        ["Shift-T"] = function() end,

        -- This will run if you press Button 4 (side button) with only Control
        -- held.
        ["Ctrl-MB4"] = function() end,
    },
}

return config
```

The full lists of keysyms, mouse buttons, and modifiers can be found in the
[Lookup Tables] section.

<div class="warning">

An action will only trigger if your pressed modifiers exactly match those
specified. For example, an action for `"T"` will only trigger if you press T
while Shift, Control, etc. are not pressed. However, Caps Lock and Num Lock
(mod2) will be ignored unless the action explicitly requires them.

The `*` modifier acts as a wildcard, allowing the action to run while other
modifier keys are pressed. An action for `"*-T"` will trigger if you press T,
regardless of what modifier keys (Shift, Control, etc.) are pressed.

This also works in combination with other modifiers: `"*-Shift-T"` will trigger
as long as Shift is pressed, but other modifiers will not prevent the action
from being run.

</div>

### Input consumption

By default, if waywall finds and runs an action, the input which triggered the
action will be silently dropped and not passed on to the Minecraft instance.

However, in some cases (e.g. if an action fails to run), you may want the input
to be passed along to the Minecraft instance as normal. You can make this happen
by returning `false` from your action's function.

> If your action pauses execution at any point (e.g. by calling `waywall.sleep()`)
> it will always consume the input, even if you return `false`.

[Lookup Tables]: 03_lookup_tables.md
