# SolixHub UI Library — Documentation
# ⚠️ WARNING: This documentation is AI made. Maybe errors.

> Load the library with:
> ```lua
> local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/wrexlua/SOLIXHUB/refs/heads/retard/SolixUI.lua"))()
> ```

---

## Table of Contents

1. [Window](#window)
2. [Page](#page)
3. [Section](#section)
4. [Toggle](#toggle)
5. [Checkbox](#checkbox)
6. [Button](#button)
7. [Slider](#slider)
8. [Dropdown](#dropdown)
9. [ToggleDropdown](#toggledropdown)
10. [Colorpicker](#colorpicker)
11. [Keybind](#keybind)
12. [Textbox](#textbox)
13. [Label](#label)
14. [Notification](#notification)
15. [Watermark](#watermark)
16. [KeybindList](#keybindlist)
17. [Settings Page](#settings-page)
18. [Config System](#config-system)
19. [Theme System](#theme-system)
20. [Flags](#flags)
21. [Example](#example)

---

## Window

Creates the main UI window.

```lua
local Window = Library:Window({
    Name        = "My Hub",       -- Title displayed in the top-left
    Size        = UDim2.new(0, 770, 0, 526), -- Optional, default 770x526
    FadeSpeed   = 0.25,           -- Animation speed for dropdowns/pickers
    BackgroundIcon = "rbxassetid://..." -- Optional background image
})
```

### Methods

| Method | Description |
|--------|-------------|
| `Window:SetBackgroundTransparency(value)` | Set background opacity (0–1) |
| `Window:SetBackgroundImage(assetId)` | Change background image |
| `Window:SetOpen(bool)` | Show or hide the window |
| `Window:Minimize(bool)` | Collapse or expand the window |

> **Menu Keybind:** Default is `RightControl`. Change via `Library.MenuKeybind = "Enum.KeyCode.X"`.

---

## Page

Adds a navigation tab to the window.

```lua
local Page = Window:Page({
    Name    = "Combat",  -- Tab label
    Columns = 2          -- Number of content columns (default: 2)
})
```

### Key Page (for license keys)

```lua
local KeyPage = Window:Page({ Name = "Key", IsKeyPage = true })

KeyPage:AddKey("Enter your key", "MYKEY123", "https://getkey.link", function()
    -- runs on successful key entry
end)
```

---

## Section

Groups elements inside a page column.

```lua
local Section = Page:Section({
    Name = "Aimbot",
    Side = 1  -- 1 = left column, 2 = right column
})
```

### Special Sections

```lua
-- Displays a selectable image from a table
Page:ImageSection({
    Name = "Gallery",
    Side = 1,
    Images = { ["Cat"] = "115002736787206" }
})

-- Displays a rotating 3D viewport of a Part
Page:ViewportSection({
    Name = "Character",
    Side = 2,
    Part = workspace.SomePart
})
```

---

## Toggle

A binary on/off switch.

```lua
local MyToggle = Section:Toggle({
    Name     = "Silent Aim",
    Flag     = "SilentAim",       -- Unique identifier for saving/loading
    Default  = false,
    Disabled = false,
    Tooltip  = "Enables silent aim",
    Callback = function(value) end
})
```

### Methods

| Method | Description |
|--------|-------------|
| `MyToggle:Set(bool)` | Set value programmatically |
| `MyToggle:SetText(text)` | Change label |
| `MyToggle:SetDisabled(bool)` | Enable/disable interaction |
| `MyToggle:SetVisible(bool)` | Show/hide the element |
| `MyToggle:OnChanged(callback)` | Subscribe to value changes |

### Sub-elements

Toggles can have inline **Colorpickers** and **Keybinds**:

```lua
MyToggle:Colorpicker({ Flag = "AimColor", Default = Color3.fromRGB(255,0,0) })
MyToggle:Keybind({ Flag = "AimKey", Default = Enum.KeyCode.X, Mode = "Toggle" })
```

---

## Checkbox

Visually identical to a Toggle but uses a checkmark indicator instead of a pill.

```lua
local MyCheckbox = Section:Checkbox({
    Name     = "Show ESP",
    Flag     = "ESP",
    Default  = false,
    Callback = function(value) end
})
```

Supports the same methods as Toggle, plus `:Colorpicker()` and `:Keybind()` sub-elements.

---

## Button

A row of clickable buttons (auto-fills width equally).

```lua
local Buttons = Section:Button()

local Btn = Buttons:Add(
    "Teleport",           -- Label
    function() end,       -- Callback
    false,                -- Disabled
    "Teleports you"       -- Tooltip
)
```

### Button Methods

| Method | Description |
|--------|-------------|
| `Btn:SetText(text)` | Change button label |
| `Btn:SetDisabled(bool)` | Enable/disable |
| `Btn:SetVisible(bool)` | Show/hide |
| `Btn:OnPressed(callback)` | Subscribe to press event |
| `Buttons:SetVisible(bool)` | Show/hide the whole row |

---

## Slider

A draggable numeric input.

```lua
local MySlider = Section:Slider({
    Name     = "Walk Speed",
    Flag     = "WalkSpeed",
    Min      = 16,
    Max      = 500,
    Default  = 16,
    Decimals = 1,        -- Precision (e.g. 0.01 for 2 decimal places)
    Suffix   = " ws",    -- Text appended to the value
    Callback = function(value) end
})
```

### Methods

| Method | Description |
|--------|-------------|
| `MySlider:Set(value)` | Set value programmatically |
| `MySlider:SetMin(value)` | Change minimum |
| `MySlider:SetMax(value)` | Change maximum |
| `MySlider:SetText(text)` | Change label |
| `MySlider:SetSuffix(text)` | Change suffix |
| `MySlider:SetDisabled(bool)` | Enable/disable |
| `MySlider:SetVisible(bool)` | Show/hide |
| `MySlider:OnChanged(callback)` | Subscribe to value changes |

---

## Dropdown

A selectable list of options, with optional multi-select.

```lua
local MyDropdown = Section:Dropdown({
    Name    = "Hit Part",
    Flag    = "HitPart",
    Items   = { "Head", "Torso", "Arms" },
    Default = "Head",     -- Single: string. Multi: table of strings
    Multi   = false,      -- true to allow multiple selections
    Callback = function(value) end
})
```

### Methods

| Method | Description |
|--------|-------------|
| `MyDropdown:Set(value)` | Set selected value(s) |
| `MyDropdown:Get()` | Returns current value |
| `MyDropdown:Add(option, icon)` | Add an option (icon = asset ID string, optional) |
| `MyDropdown:Remove(option)` | Remove an option by name |
| `MyDropdown:Clear()` | Remove all options |
| `MyDropdown:Refresh(list)` | Replace all options with a new table |
| `MyDropdown:SetText(text)` | Change label |
| `MyDropdown:SetMulti(bool)` | Toggle multi-select mode |
| `MyDropdown:SetDisabled(bool)` | Enable/disable |
| `MyDropdown:SetVisible(bool)` | Show/hide |
| `MyDropdown:OnChanged(callback)` | Subscribe to changes |

---

## ToggleDropdown

A dropdown where each option has a built-in checkmark indicator (useful for feature lists).

```lua
local MyToggleDropdown = Section:ToggleDropdown({
    Name    = "Features",
    Flag    = "FeatureList",
    Items   = { "Speed", "Fly", "Noclip" },
    Multi   = true,
    Callback = function(value) end
})
```

Supports the same methods as Dropdown.

---

## Colorpicker

A full HSV color picker with alpha, hex input, and animation modes.

```lua
-- Standalone (attach to a Label or use inside Toggle/Checkbox)
local MyColorpicker = MyToggle:Colorpicker({
    Flag     = "AimColor",
    Default  = Color3.fromRGB(255, 0, 0),
    Alpha    = 0,    -- 0 = fully opaque, 1 = fully transparent
    Callback = function(color, alpha) end
})
```

### Methods

| Method | Description |
|--------|-------------|
| `MyColorpicker:Set(color, alpha)` | Set color. Accepts `Color3`, hex string `"#FF0000"`, or `{r,g,b}` table |
| `MyColorpicker:SetOpen(bool)` | Open/close the picker window |

### Animations

The colorpicker includes built-in **Rainbow** and **Breathing** animation modes accessible from inside the picker UI.

---

## Keybind

A key binding button. Left-click to pick a key, right-click to open the mode menu.

```lua
local MyKeybind = MyToggle:Keybind({
    Flag     = "AimKey",
    Default  = Enum.KeyCode.X,
    Mode     = "Toggle",   -- "Toggle", "Hold", or "Always"
    Callback = function(isActive) end
})
```

### Modes

| Mode | Behavior |
|------|----------|
| `Toggle` | Flips on/off each key press |
| `Hold` | Active only while key is held |
| `Always` | Always active once set |

### Methods

| Method | Description |
|--------|-------------|
| `MyKeybind:Set(key)` | Set key. Accepts `Enum.KeyCode`, `Enum.UserInputType`, or `{Key=..., Mode=...}` table |
| `MyKeybind:SetMode(mode)` | Change mode |
| `MyKeybind:Press(bool)` | Manually trigger the keybind |
| `MyKeybind:SetOpen(bool)` | Open/close the mode menu |

### Flag Value

`Library.Flags["MyFlag"]` returns `{ Key, Mode, Toggled }`.

---

## Textbox

A single-line text input.

```lua
local MyTextbox = Section:Textbox({
    Name        = "Target Name",
    Flag        = "TargetName",
    Default     = "",
    Placeholder = "Enter name...",
    Callback    = function(value) end  -- fires on focus lost
})
```

### Methods

| Method | Description |
|--------|-------------|
| `MyTextbox:Set(value)` | Set text programmatically |
| `MyTextbox:SetText(text)` | Alias for Set |
| `MyTextbox:SetDisabled(bool)` | Enable/disable |
| `MyTextbox:SetVisible(bool)` | Show/hide |

---

## Label

A styled text label. Supports rich text via `RichText`.

```lua
local MyLabel = Section:Label(
    "Status: Active",  -- Text
    "Left",            -- Alignment: "Left", "Center", "Right"
    "Tooltip text",    -- Optional tooltip
    false              -- Outline (bool)
)
```

### Methods

| Method | Description |
|--------|-------------|
| `MyLabel:SetText(text)` | Update label text |
| `MyLabel:SetTextColor(color)` | Override text color |
| `MyLabel:Colorpicker(props)` | Attach an inline colorpicker |
| `MyLabel:Keybind(props)` | Attach an inline keybind |

---

## Notification

Shows a timed toast notification in the top-right corner.

```lua
Library:Notification(
    "Success!",            -- Title
    "Config loaded.",      -- Description
    5                      -- Duration in seconds
)
```

---

## Watermark

A floating FPS/ping display bar.

```lua
local Watermark = Library:Watermark("My Hub | v1.0")

Watermark:SetVisible(true)   -- Show
Watermark:SetVisible(false)  -- Hide
```

Automatically updates with current **FPS** and **Ping** every frame.

---

## KeybindList

A draggable overlay listing all active keybinds.

```lua
local KeybindList = Library:KeybindList()

KeybindList:SetVisible(true)
```

Keybinds automatically register themselves when created. Status highlights in accent color when active.

---

## Settings Page

Automatically generates a full Settings tab with config management, theme editing, and UI tweaks.

```lua
Library:CreateSettingsPage(Window, Watermark, KeybindList)
```

Includes:
- **Configs** — Create, load, save, delete, and autoload configs
- **Themes** — Create, load, save, delete theme files; built-in presets
- **Theme Editor** — Per-color colorpickers for every theme key
- **Settings** — Menu keybind, background opacity, tween time/style/direction
- **Watermark & Keybind List** toggles

---

## Config System

```lua
-- Get current config as JSON string
local json = Library:GetConfig()

-- Load a config from JSON string
local success, err = Library:LoadConfig(json)

-- Write/read from file
writefile("solixhub/Configs/myconfig123456.json", Library:GetConfig())
Library:LoadConfig(readfile("solixhub/Configs/myconfig123456.json"))

-- Delete a config file
Library:DeleteConfig("filename.json")

-- Autoload on script start
Library:CheckForAutoLoad()
```

> Config files are stored in `solixhub/Configs/`. The file name includes the `game.GameId` to separate configs per game.

---

## Theme System

```lua
-- Change a single theme color at runtime
Library:ChangeTheme("Accent", Color3.fromRGB(100, 200, 255))

-- Available theme keys:
-- "Background", "Inline", "Border", "Shadow",
-- "Text", "Inactive Text", "Accent", "Element", "Gradient"

-- Built-in presets: Default, Halloween, Aqua, Onetap, Bitchbot, Gamesense
```

---

## Flags

Every element with a `Flag` property writes its value to `Library.Flags[flag]`.

```lua
print(Library.Flags["SilentAim"])     -- boolean
print(Library.Flags["WalkSpeed"])     -- number
print(Library.Flags["HitPart"])       -- string
print(Library.Flags["AimColor"])      -- { Color = "rrggbb", Alpha = 0..1 }
print(Library.Flags["AimKey"])        -- { Key = "Enum.KeyCode.X", Mode = "Toggle", Toggled = bool }
```

You can also set any flag programmatically:

```lua
Library.SetFlags["SilentAim"](true)
Library.SetFlags["WalkSpeed"](200)
```

---

## Unloading

```lua
Library:Unload()
-- Disconnects all connections, destroys GUI, clears getgenv().Library
```

---

## Folder Structure

```
solixhub/
├── autoload.json       ← Active autoload config
├── Assets/             ← Fonts, images (auto-downloaded)
├── Configs/            ← Saved configs (per GameId)
└── Themes/             ← Saved themes
```

---

## Example

```lua
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/wrexlua/SOLIXHUB/refs/heads/retard/SolixUI.lua"))()

local Window = Library:Window({
    Name = "Example Hub",
    Size = UDim2.new(0, 770, 0, 526),
    FadeSpeed = 0.25
})

local Watermark = Library:Watermark("Example Hub | v1.0")
local KeybindList = Library:KeybindList()

local MainPage = Window:Page({
    Name = "Main",
    Columns = 2
})

local CombatSection = MainPage:Section({
    Name = "Combat",
    Side = 1
})

CombatSection:Toggle({
    Name = "Silent Aim",
    Flag = "SilentAim",
    Default = false,
    Callback = function(Value)
        print("Silent Aim:", Value)
    end
}):Keybind({
    Flag = "SilentAimKey",
    Default = Enum.KeyCode.X,
    Mode = "Toggle"
})

CombatSection:Slider({
    Name = "FOV",
    Flag = "FOVSlider",
    Min = 1,
    Max = 360,
    Default = 90,
    Decimals = 1,
    Suffix = "°",
    Callback = function(Value)
        print("FOV:", Value)
    end
})

CombatSection:Dropdown({
    Name = "Hit Part",
    Flag = "HitPart",
    Items = {"Head", "Torso", "Random"},
    Default = "Head",
    Callback = function(Value)
        print("Hit Part:", Value)
    end
})

local VisualSection = MainPage:Section({
    Name = "Visuals",
    Side = 2
})

VisualSection:Checkbox({
    Name = "ESP",
    Flag = "ESP",
    Default = false,
    Callback = function(Value)
        print("ESP:", Value)
    end
}):Colorpicker({
    Flag = "ESPColor",
    Default = Color3.fromRGB(255, 0, 0),
    Alpha = 0,
    Callback = function(Color, Alpha)
        print("ESP Color:", Color, Alpha)
    end
})

local ChamsToggle = VisualSection:Toggle({
    Name = "Chams",
    Flag = "Chams",
    Default = false,
    Callback = function(Value)
        print("Chams:", Value)
    end
})

ChamsToggle:Colorpicker({
    Flag = "ChamsColor",
    Default = Color3.fromRGB(0, 255, 0),
    Alpha = 0
})

VisualSection:Textbox({
    Name = "Custom Text",
    Flag = "CustomText",
    Placeholder = "Enter text...",
    Callback = function(Value)
        print("Text:", Value)
    end
})

local ButtonRow = VisualSection:Button()
ButtonRow:Add("Teleport", function()
    print("Teleporting...")
end)
ButtonRow:Add("Reset", function()
    print("Resetting...")
end)

VisualSection:Label("Made by Example", "Center")

local PlayerPage = Window:Page({
    Name = "Player",
    Columns = 2
})

local PlayerSection = PlayerPage:Section({
    Name = "Player",
    Side = 1
})

PlayerSection:Slider({
    Name = "Walk Speed",
    Flag = "WalkSpeed",
    Min = 16,
    Max = 500,
    Default = 16,
    Decimals = 1,
    Suffix = " ws",
    Callback = function(Value)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = Value
    end
})

PlayerSection:Slider({
    Name = "Jump Power",
    Flag = "JumpPower",
    Min = 50,
    Max = 500,
    Default = 50,
    Decimals = 1,
    Suffix = " jp",
    Callback = function(Value)
        game.Players.LocalPlayer.Character.Humanoid.JumpPower = Value
    end
})

Library:CreateSettingsPage(Window, Watermark, KeybindList)
Library:CheckForAutoLoad()

Watermark:SetVisible(false)
KeybindList:SetVisible(false)

Library:Notification("Welcome!", "Example Hub loaded.", 5)
```
