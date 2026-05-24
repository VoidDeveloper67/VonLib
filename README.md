# ✨ VonLib

## 📌 About

- **VonLib** is a rebuilt and optimized Roblox UI library.
- Clean, minimal aesthetic with powerful options API.
- 🔹 Made by **von63rd**
- 🔹 Designed for use in **VonLib Hub** scripts
- 🔹 Open-Source, Lightweight, and Optimized

-----

## 🚀 Getting Started

```lua
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/VoidDeveloper67/VonLib/refs/heads/main/main"))()
```

### Creating a Window

```lua
local Window = Library:MakeWindow({
  Title = "Nice Hub : Cool Game",
  SubTitle = "by von63rd",
  ScriptFolder = "vonlib"
})
```

-----

## 🪟 Window API

|Method                             |Description                                 |
|-----------------------------------|--------------------------------------------|
|`NewMinimizer(config)`             |Create a keyboard minimizer                 |
|`MakeTab(config)`                  |Create a new tab                            |
|`Notify(config)`                   |Show a notification                         |
|`NewNotifyGroup(config)`           |Create a notification group                 |
|`Dialog(config)`                   |Show a dialog modal                         |
|`SelectTab(tab/number)`            |Switch to a tab                             |
|`SetUIScale(value)`                |Set UI scale (0.6–1.6)                      |
|`Tag(config)`                      |Add a colored tag to the topbar ⭐ NEW       |
|`SaveConfig(name)`                 |Save flagged elements to a config file ⭐ NEW|
|`LoadConfig(name)`                 |Load a config file and apply values ⭐ NEW   |
|`ListConfigs()`                    |List all saved config files ⭐ NEW           |
|`DeleteConfig(name)`               |Delete a config file ⭐ NEW                  |
|`ResetConfig(name?)`               |Reset elements to default ⭐ NEW             |
|`SetBackgroundVideo(url, overlay?)`|Set a video background ⭐ NEW                |
|`PauseBackgroundVideo()`           |Pause the background video ⭐ NEW            |
|`PlayBackgroundVideo()`            |Resume the background video ⭐ NEW           |
|`SetBackgroundVideoOverlay(t)`     |Adjust overlay transparency ⭐ NEW           |
|`GetVersion()`                     |Returns version string                      |
|`GetAuthor()`                      |Returns author name                         |
|`DeleteFlags()`                    |Delete saved flags                          |
|`GetFlag(flag)`                    |Get a saved flag value                      |
|`SetFlag(flag, value)`             |Save a flag value                           |
|`SetTitle(title)`                  |Update window title                         |
|`SetSubTitle(subtitle)`            |Update window subtitle                      |
|`GetTitle()`                       |Get current title                           |
|`GetSubTitle()`                    |Get current subtitle                        |
|`MinimizeButton()`                 |Toggle minimize                             |
|`Destroy()`                        |Destroy the UI                              |

-----

## 🏷️ Tags (NEW)

Add colored label badges to the window topbar.

```lua
local MyTag = Window:Tag({
  Title = "v1.1.0",
  Color = "Amber",   -- or Color3 / hex not supported yet
  Icon  = "rbxassetid://...",  -- optional
})

-- Update dynamically
MyTag:SetTitle("v1.2.0")
MyTag:SetColor("Green")
MyTag:SetIcon("")
MyTag:Destroy()
```

**Available tag colors:** `Amber`, `Red`, `Green`, `Blue`, `Purple`, `Pink`, `Cyan`, `Orange`, `Gray`, `White`, `Default`

-----

## 🎨 Themes

**All available themes:**

|Theme     |Description            |
|----------|-----------------------|
|`Darker`  |Classic dark (default) |
|`Midnight`|Deep violet-blue ⭐ NEW |
|`Ocean`   |Teal / deep sea ⭐ NEW  |
|`Rose`    |Dark rose-red ⭐ NEW    |
|`Emerald` |Deep forest green ⭐ NEW|
|`Sunset`  |Dark warm orange ⭐ NEW |

```lua
Library:SetTheme("Midnight")
Library:SetTheme("Ocean")
Library:SetTheme("Rose")
Library:SetTheme("Emerald")
Library:SetTheme("Sunset")
```

-----

## 🎬 Background Video (NEW)

Add a video to the window background. Supports `rbxassetid://` or a direct `.webm` URL.

```lua
-- Asset ID
Window:SetBackgroundVideo("rbxassetid://123456789", 0.4)

-- URL (auto-downloads and caches as vonlib_bgvideo.webm)
Window:SetBackgroundVideo("https://files.catbox.moe/yourfile.webm", 0.4)

-- Control
Window:PauseBackgroundVideo()
Window:PlayBackgroundVideo()
Window:SetBackgroundVideoOverlay(0.6)  -- 0 = solid black, 1 = fully transparent
```

> 💡 To get a webm URL: convert your video at [convertio.co](https://convertio.co) then host it free at [catbox.moe](https://catbox.moe).

-----

## 💾 Config System (NEW)

Save and restore element values using `Flag` keys.

```lua
local Window = Library:MakeWindow({
  Title = "My Hub",
  SubTitle = "by von63rd",
  ScriptFolder = "vonlib"
})

-- Elements with Flag are tracked automatically
Tab:AddToggle({ Name = "Auto Farm", Flag = "auto_farm", Default = false, Callback = function(v) end })
Tab:AddSlider({ Name = "Speed", Flag = "speed", Min = 0, Max = 100, Default = 16, Callback = function(v) end })

-- Save / Load
Window:SaveConfig("slot1")
Window:LoadConfig("slot1")

-- List & delete
local configs = Window:ListConfigs()  -- { "Default", "slot1" }
Window:DeleteConfig("slot1")

-- Reset
Window:ResetConfig()          -- reset values only
Window:ResetConfig("slot1")   -- reset + delete file
```

-----

## 🔖 Badge (NEW)

Universal property for any element — shows a colored status pill next to the title.

```lua
Tab:AddToggle({ Name = "Auto Farm", Badge = "New", Default = false, Callback = function(v) end })
Tab:AddButton({ Name = "Teleport", Badge = "Hot", Callback = function() end })
Tab:AddSlider({ Name = "Speed", Badge = "Bug", Min = 0, Max = 100, Default = 16, Callback = function(v) end })
```

**Available badges:** `Bug` 🔴 · `New` 🟢 · `Warning` 🟡 · `Fixed` 🔵 · `Beta` 🟣 · `Hot` 🟠 · `Soon` ⚫

After creating an element you can call:

```lua
local toggle = Tab:AddToggle({ Name = "ESP", Default = false, Callback = function(v) end })
toggle:ApplyBadge("New")
```

-----

## ✨ Text Gradient (NEW)

Create labels with gradient-colored text.

```lua
-- Simple (default purple-to-cyan gradient)
Tab:AddGradientLabel("VonLib v1.1.0")

-- Custom colors and rotation
Tab:AddGradientLabel({
  Text = "Rainbow Label",
  Colors = {
    Color3.fromRGB(255, 80, 80),
    Color3.fromRGB(255, 200, 50),
    Color3.fromRGB(80, 255, 120),
    Color3.fromRGB(80, 180, 255),
    Color3.fromRGB(180, 80, 255)
  },
  Rotation = 90
})

-- API
local lbl = Tab:AddGradientLabel({ Text = "Status", Colors = {Color3.fromRGB(0,200,255), Color3.fromRGB(200,0,255)} })
lbl:SetText("Online")
lbl:SetGradient({Color3.fromRGB(0,255,100), Color3.fromRGB(0,180,255)}, 45)
```

-----

## 📑 Minimizer

```lua
local Minimizer = Window:NewMinimizer({
  KeyCode = Enum.KeyCode.LeftControl
})

local MobileButton = Minimizer:CreateMobileMinimizer({
  Image = "rbxassetid://101833678008843",
  Size = UDim2.new(0, 35, 0, 35),
  Corner = { CornerRadius = UDim.new(0, 6) },
})
```

-----

## 📂 Tabs

```lua
-- Normal
local Tab = Window:MakeTab({ Title = "Main", Icon = "Home" })

-- Compact
local Tab = Window:MakeTab({ "Main", "Home" })
```

-----

## 🔔 Notifications

```lua
Window:Notify({
  Title = "Loaded!",
  Content = "VonLib v1.1.0",
  Image = "rbxassetid://101833678008843",
  Duration = 5
})
```

-----

## 💬 Dialog

```lua
Window:Dialog({
  Title = "Confirm",
  Content = "Are you sure?",
  Options = {
    { Name = "Yes", Callback = function(self) print("Yes!") end },
    { Name = "No" }
  }
})
```

-----

## ⚙️ Options API

All elements support:

|Method                |Description        |
|----------------------|-------------------|
|`SetTitle(title)`     |Update title       |
|`SetDescription(desc)`|Update description |
|`SetVisible(bool)`    |Show/hide element  |
|`Destroy()`           |Remove element     |
|`AddCallback(fn)`     |Add extra callback |
|`ApplyBadge(label)`   |Apply a badge ⭐ NEW|

-----

## 🧩 Elements

### Section

```lua
Tab:AddSection("Section Title")
```

### Toggle

```lua
Tab:AddToggle({
  Name = "Toggle",
  Badge = "New",       -- optional badge
  Default = false,
  Flag = "my_toggle",
  Callback = function(Value) end
})
```

### Button

```lua
Tab:AddButton({
  Name = "My Button",
  Badge = "Hot",       -- optional badge
  Debounce = 0.5,
  Callback = function() end
})
```

### Slider

```lua
Tab:AddSlider({
  Name = "Speed",
  Badge = "Fixed",     -- optional badge
  Min = 0, Max = 100,
  Increment = 1,
  Default = 50,
  Flag = "speed",
  Callback = function(Value) end
})
```

### Keybind

```lua
Tab:AddKeybind({
  Name = "Sprint Key",
  Default = Enum.KeyCode.LeftShift,
  Flag = "sprint_key",
  Callback = function(Key) print("Key:", Key.Name) end
})
```

### ColorPicker

```lua
Tab:AddColorPicker({
  Name = "ESP Color",
  Default = Color3.fromRGB(0, 180, 255),
  Flag = "esp_color",
  Callback = function(Color) print(Color) end
})
```

### Label

```lua
Tab:AddLabel("Status: Active")
Tab:AddLabel({ Text = "VIP Only", Color = Color3.fromRGB(255, 215, 0) })
```

### Gradient Label (NEW)

```lua
Tab:AddGradientLabel("VonLib")

Tab:AddGradientLabel({
  Text = "Epic Title",
  Colors = { Color3.fromRGB(255,80,80), Color3.fromRGB(80,180,255) },
  Rotation = 0
})
```

### Dropdown

```lua
Tab:AddDropdown({
  Name = "Mode",
  Options = { "Option A", "Option B", "Option C" },
  Default = "Option A",
  Callback = function(Value) end
})

-- MultiSelect
Tab:AddDropdown({
  Name = "Items",
  MultiSelect = true,
  Options = { "one", "two", "three" },
  Default = { "one", "two" },
  Callback = function(Value) end
})
```

### TextBox

```lua
Tab:AddTextBox({
  Name = "Enter Text",
  Placeholder = "type...",
  ClearOnFocus = true,
  Flag = "my_text",
  Callback = function(Value) end
})
```

### Paragraph

```lua
Tab:AddParagraph("Title", "Some multi-line\ndescription text.")
```

### Discord Invite

```lua
Tab:AddDiscordInvite({
  Title = "VonLib Hub | Community",
  Description = "Join us!",
  Banner = "rbxassetid://17382040552",
  Logo = "rbxassetid://17382040552",
  Invite = "https://discord.gg/your-invite",
  Members = 470000,
  Online = 20000,
})
```

-----

## 📐 UI Scale

- Min: `0.6` · Default: `1.0` · Max: `1.6`

```lua
Library:SetUIScale(1.2)
print(Library:GetMinScale(), Library:GetMaxScale())
print("Version:", Library:GetVersion())
print("Author:", Library:GetAuthor())
```

-----

## 🏁 Full Example

```lua
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/VoidDeveloper67/VonLib/refs/heads/main/main"))()

local Window = Library:MakeWindow({
  Title = "VonLib Hub : Game",
  SubTitle = "by von63rd",
  ScriptFolder = "vonlib",
  BackgroundVideo = "https://files.catbox.moe/5f9loy.webm"
})

-- Tags on topbar
Window:Tag({ Title = "v1.1.0", Color = "Amber" })
Window:Tag({ Title = "Beta", Color = "Purple" })

-- Background video (optional)
-- BackgroundVideo is set via MakeWindow config above (5f9loy.webm)

-- Minimizer
local Minimizer = Window:NewMinimizer({ KeyCode = Enum.KeyCode.LeftControl })
local MobileButton = Minimizer:CreateMobileMinimizer({
  Image = "rbxassetid://101833678008843",
  Size = UDim2.new(0, 35, 0, 35),
  Corner = { CornerRadius = UDim.new(0, 6) },
})

-- Tabs
local MainTab   = Window:MakeTab({ Title = "Main",   Icon = "Home" })
local ConfigTab = Window:MakeTab({ Title = "Config",  Icon = "Settings" })

-- ── Main Tab ───────────────────────────────────────────────────────────────

MainTab:AddSection("Info")
MainTab:AddGradientLabel({
  Text = "✦ VonLib v1.1.0",
  Colors = { Color3.fromRGB(120,80,255), Color3.fromRGB(80,200,255) },
  Rotation = 0
})

MainTab:AddSection("Actions")
MainTab:AddButton({
  Name = "Test Button",
  Badge = "Hot",
  Callback = function()
    Window:Notify({ Title = "Clicked", Content = "Button pressed!", Duration = 3 })
  end
})

MainTab:AddSection("Toggles")
MainTab:AddToggle({
  Name = "Auto Farm",
  Badge = "New",
  Default = false,
  Flag = "auto_farm",
  Callback = function(v)
    Window:Notify({ Title = "Auto Farm", Content = tostring(v), Duration = 2 })
  end
})
MainTab:AddToggle({
  Name = "ESP",
  Badge = "Beta",
  Default = false,
  Flag = "esp_enabled",
  Callback = function(v) print("ESP:", v) end
})

MainTab:AddSection("Slider")
MainTab:AddSlider({
  Name = "Walk Speed",
  Badge = "Fixed",
  Min = 16, Max = 200, Increment = 1, Default = 16,
  Flag = "walk_speed",
  Callback = function(v) print("Speed:", v) end
})

MainTab:AddSection("Keybind")
MainTab:AddKeybind({
  Name = "Sprint Key",
  Default = Enum.KeyCode.LeftShift,
  Flag = "sprint_key",
  Callback = function(Key) print("Sprint:", Key.Name) end
})

MainTab:AddSection("Color Picker")
MainTab:AddColorPicker({
  Name = "ESP Color",
  Badge = "New",
  Default = Color3.fromRGB(0, 180, 255),
  Flag = "esp_color",
  Callback = function(c) print("Color:", c) end
})

MainTab:AddSection("Labels")
MainTab:AddLabel({ Text = "Status: Online", Color = Color3.fromRGB(80, 220, 80) })
MainTab:AddGradientLabel({
  Text = "Premium Feature",
  Colors = { Color3.fromRGB(255,200,0), Color3.fromRGB(255,100,0) },
  Rotation = 45
})

MainTab:AddSection("Dropdown")
MainTab:AddDropdown({
  Name = "Select Fruit",
  Options = { "Light", "Dough", "Leopard" },
  Default = "Light",
  Flag = "fruit_select",
  Callback = function(v) print("Fruit:", v) end
})

MainTab:AddSection("TextBox")
MainTab:AddTextBox({
  Name = "Custom Text",
  Placeholder = "type here...",
  ClearOnFocus = true,
  Callback = function(v) print("Text:", v) end
})

MainTab:AddSection("Discord")
MainTab:AddDiscordInvite({
  Title = "VoidHub",
  Description = "Best Server In the World.",
  Banner = "rbxassetid://101833678008843",
  Logo = "rbxassetid://101833678008843",
  Invite = "https://discord.gg/Wsarxj9Gzz"
})

-- ── Config Tab ─────────────────────────────────────────────────────────────

ConfigTab:AddSection("UI Scale")
ConfigTab:AddSlider({
  Name = "Scale",
  Min = 0.6, Max = 1.6, Increment = 0.1, Default = 1,
  Callback = function(v) Library:SetUIScale(v) end
})

ConfigTab:AddSection("Theme")
ConfigTab:AddDropdown({
  Name = "Theme",
  Options = Library:GetThemes(),
  Default = Library:GetTheme().Name,
  Callback = function(v) Library:SetTheme(v) end
})

ConfigTab:AddSection("Config Slots")
ConfigTab:AddButton({
  Name = "Save Config",
  Badge = "New",
  Callback = function()
    Window:SaveConfig("Default")
    Window:Notify({ Title = "Config", Content = "Saved!", Duration = 3 })
  end
})
ConfigTab:AddButton({
  Name = "Load Config",
  Callback = function()
    local ok = Window:LoadConfig("Default")
    Window:Notify({ Title = "Config", Content = ok and "Loaded!" or "No config found", Duration = 3 })
  end
})
ConfigTab:AddButton({
  Name = "Reset Config",
  Badge = "Warning",
  Callback = function()
    Window:ResetConfig()
    Window:Notify({ Title = "Config", Content = "Reset to defaults", Duration = 3 })
  end
})

-- ── Startup ────────────────────────────────────────────────────────────────

Window:Notify({
  Title = "VonLib Loaded",
  Content = "v1.1.0 by von63rd | LeftControl to Minimize",
  Image = "rbxassetid://101833678008843",
  Duration = 5
})

Window:SelectTab(1)
```
