# ✨ VonLib

**VonLib** is a Roblox UI library — a single `main.luau` file with a full elements API, six built-in colour themes plus support for your own custom ones, collapsible/pop-out groupboxes, a live search box, a draggable stats overlay, and working save/load config slots.

- 🔹 Made by **von63rd**
- 🔹 Open-source · Lightweight · DPI-aware

---

## 🚀 Quick Start

```lua
local Library = loadstring(game:HttpGet(
  "https://raw.githubusercontent.com/VoidDeveloper67/VonLib/refs/heads/main/main.luau"
))()

local Window = Library:MakeWindow({
  Title        = "My Hub : Game Name",
  SubTitle     = "by von63rd",
  ScriptFolder = "vonlib", -- optional; enables persisted settings, flags and configs
})

local MainTab = Window:MakeTab({ "Main" })

MainTab:AddToggle({
  Name     = "Auto Farm",
  Default  = false,
  Flag     = "auto_farm",
  Callback = function(value) print("Auto Farm:", value) end,
})
```

`ScriptFolder` is optional. Without it the UI still works, but nothing is written to disk — no remembered window size/theme, no persisted flags, and `SaveConfig`/`LoadConfig` have nowhere to write. Set it to get all of that for free.

---

## 🧭 How it fits together

```
Library
  └─ Window   (Library:MakeWindow)
       └─ Tab   (Window:MakeTab)
            ├─ Elements   (Tab:AddToggle, Tab:AddButton, ...)
            └─ Groupbox   (Tab:AddGroupbox)
                 └─ Elements / nested Groupbox
```

`Library` is the thing `loadstring(...)` returns. It owns global concerns: themes, search, the stats HUD, and window creation. Calling `Library:MakeWindow(...)` builds the actual UI and gives you back a **Window** — everything from tabs to notifications to config saving hangs off that. Each **Tab** is a scrollable page; **Groupbox** is an optional bordered card inside a tab that groups related elements and supports every element method a Tab does, including nesting another groupbox inside it.

Only one window can exist per script (`MakeWindow` errors on a second call).

---

## 📚 Library API

Called directly on the value returned by `loadstring(...)`.

| Method | Description |
|---|---|
| `MakeWindow(config)` | Create the window (only once per script) |
| `MakeStatsHUD(config)` | Create a draggable live-stats overlay, independent of the window |
| `Search(query)` | Filter the current tab's elements by title |
| `ClearSearch()` | Clear the active search filter |
| `CreateTheme(name, config)` | Register a theme (built-in name or a fully custom one) |
| `SetCustomTheme(config)` | Register and immediately apply a one-off custom theme |
| `SetTheme(name)` | Switch to a registered theme by name |
| `GetTheme(name?)` | Get a theme table by name, or the current one if omitted |
| `GetThemes()` | List every registered theme name |
| `IsValidTheme(name)` | Check whether a theme name is registered |
| `SetUIScale(scale)` | Manually set the UI scale (`GetMinScale()`–`GetMaxScale()`) |
| `GetMinScale()` / `GetMaxScale()` | The UI scale clamp range (`0.6`–`1.6`) |
| `GetCurrentTheme()` | The active theme table (errors if the window hasn't been made yet) |
| `GetVersion()` | Returns the version string |
| `GetAuthor()` | Returns the author name |
| `GetIconByName(name)` | Resolve a fuzzy icon name (or a raw `rbxassetid://...`) to an asset ID |
| `Destroy()` | Destroy the entire UI |

---

## 🔍 Search

Every window ships with a live search box above the tab list. Typing filters the **currently selected tab** down to elements whose title matches — groupboxes stay visible if their own title matches *or* any element inside them does, so you never see an empty shell. Switching tabs re-applies whatever's currently typed instead of resetting it.

```lua
Library:Search("aim")
Library:ClearSearch()
```

Pass `DisableSearch = true` to `MakeWindow` to remove the search box entirely (see Window creation below).

---

## 🎨 Themes

Six built-in colour themes, each built around a unique neon accent hue, plus full support for defining your **own** — dark or light.

| Theme | Accent | Vibe |
|---|---|---|
| `Darker` | Electric blue | Void dark, default |
| `Midnight` | Neon violet | Deep space purple |
| `Ocean` | Neon cyan | Cyber teal on navy |
| `Rose` | Neon red | Blood moon |
| `Emerald` | Matrix green | Terminal black-green |
| `Sunset` | Hot pink / magenta | Synthwave neon |

```lua
Library:SetTheme("Midnight")

-- In a dropdown so users can pick live:
ConfigTab:AddDropdown({
  Name     = "Theme",
  Options  = Library:GetThemes(),
  Default  = Library:GetTheme().Name,
  Callback = function(v) Library:SetTheme(v) end
})
```

### Custom colour themes

Give `CreateTheme` a name and as few or as many colours as you want — anything left out is generated automatically from `Primary`/`Background`, including correct text/border contrast for **light** backgrounds, not just dark ones.

```lua
-- Minimal: pick an accent and a background, everything else is derived
Library:CreateTheme("Toxic", {
  Primary    = Color3.fromRGB(190, 255, 0),
  Background = Color3.fromRGB(6, 10, 4),
})
Library:SetTheme("Toxic")

-- A light theme works the same way
Library:CreateTheme("Paper", {
  Primary    = "#2D6CDF", -- hex strings work too
  Background = Color3.fromRGB(245, 245, 240),
})

-- Register and apply in one call
Library:CreateTheme("Blood", { Primary = "#FF1744", Background = "#120404", Apply = true })

-- Or skip registering a name and just apply a one-off theme
Library:SetCustomTheme({ Primary = Color3.fromRGB(255, 170, 0), Background = "#111318" })

-- Custom themes show up in GetThemes() too, so a theme dropdown built
-- from Library:GetThemes() works identically for presets and custom ones.
```

Every field can also be set explicitly: `OnPrimary`, `ScrollBar`, `Stroke`, `Error`, `IconColor`, `JoinButton`, `Link`, `DialogBackground`, `ButtonsHolding`, `ButtonsDefault`, `BorderHolding`, `BorderDefault`, `Text`, `TextDark`, `TextDarker`, `SliderBar`, `SliderNumber`, `DropdownHolder`, `BackgroundTransparency`. Pass a full raw `Colors` table to override the generated palette wholesale, an `IconSet`/`Font` table to override icons/fonts, and `Base` to pick which built-in theme to inherit unspecified icons/fonts from (defaults to `"Darker"`).

---

## 🪟 Window creation

```lua
local Window = Library:MakeWindow({
  Title         = "My Hub : Game Name", -- [1] required
  SubTitle      = "by von63rd",         -- [2] required
  ScriptFolder  = "vonlib",             -- [3] optional, no "/" allowed
  DisableSearch = false,                -- optional, default false
})
```

## 🪟 Window API

Called on the value returned by `Library:MakeWindow(...)`.

| Method | Description |
|---|---|
| `MakeTab(config)` | Create a new tab |
| `SelectTab(tab \| number)` | Switch to a tab |
| `GetTabByTitle(title)` | Find a tab by its title |
| `NewMinimizer(keycode)` | Bind a keyboard shortcut (+ mobile button) to minimize the window |
| `MinimizeButton()` | Toggle minimize state |
| `Notify(config)` | Show a notification toast |
| `NewNotifyGroup(config)` | Create a reusable notification template |
| `Dialog(config)` | Show a modal dialog with option buttons |
| `Tag(config)` | Add a coloured badge to the topbar |
| `SetFlag(name, value)` / `GetFlag(name)` | Read/write a raw flag value directly |
| `DeleteFlags()` | Clear every persisted flag |
| `SaveConfig(name?)` | Save every flagged element's current value to a named slot |
| `LoadConfig(name?)` | Apply a saved config slot to the live UI |
| `ListConfigs()` | List saved config slot names |
| `DeleteConfig(name?)` | Delete a config slot file |
| `ResetConfig(name?)` | Reset every flagged element to its script-defined default |
| `ReadFile(path)` / `WriteFile(path, content?)` | Read/write a file under `ScriptFolder` (no `content` deletes it) |
| `SetBackgroundVideo(url, overlay?)` | Set a video background |
| `PauseBackgroundVideo()` / `PlayBackgroundVideo()` | Control the background video |
| `SetBackgroundVideoOverlay(t)` | Adjust the dimming overlay transparency |
| `SetNotifyDefaultIcon(icon)` | Fallback icon for `Notify` calls that don't specify one |
| `SetTitle(title)` / `SetSubTitle(subtitle)` | Update the topbar text |
| `GetTitle()` / `GetSubTitle()` | Read the topbar text |
| `Flags` | The live flag table (auto-persists to `ScriptFolder/ScriptFlags.json` as you set it) |

`Library:Destroy()` tears down the whole UI, window included.

---

## 🗂️ Tabs

```lua
local MainTab = Window:MakeTab({
  Name = "Main", -- [1], also accepts Title
  Icon = "home", -- [2], also accepts Image — fuzzy-matched, see Icons below
})

Window:SelectTab(MainTab)
Window:SelectTab(1)
```

Every `Tab:AddX(...)` call below returns an element object supporting the common element methods described further down.

---

## 🧩 Groupbox

A bordered, titled card that groups related elements. It supports every element a `Tab` does (`AddToggle`, `AddButton`, `AddSlider`, `AddDropdown`, `AddSection`, a nested `AddGroupbox`, even `AddTabbox`) — just call them on the groupbox instead of the tab.

```lua
local Combat = Tab:AddGroupbox({
  Name        = "Combat",
  Description = "Aim and damage related settings", -- optional
})

Combat:AddToggle({ Name = "Silent Aim", Default = false, Callback = function(v) end })
Combat:AddSlider({ Name = "FOV", Min = 10, Max = 200, Default = 90, Callback = function(v) end })

-- Nest a groupbox inside another one
local SubGroup = Combat:AddGroupbox({ Name = "Advanced" })
SubGroup:AddToggle({ Name = "Prediction", Default = true, Callback = function(v) end })

-- Rename / redescribe it later
Combat:SetTitle("Combat Settings")
Combat:SetDescription("Updated description")

Combat:SetVisible(false)
Combat:Destroy()
```

**Collapsing.** A titled groupbox is collapsible by default — click its header to expand/collapse, a chevron shows the current state. Pass `Collapsed = true` to start collapsed, or `DisableCollapsing = true` to remove the click-to-collapse behaviour entirely (a groupbox with no title is never collapsible).

```lua
local Combat = Tab:AddGroupbox({ Name = "Combat", Collapsed = true })

Combat:SetCollapsed(false)
Combat:ToggleCollapsed()
print(Combat.Collapsed)
```

**Popping out.** Undock a groupbox into a draggable floating panel that stays on screen regardless of which tab is selected. The panel gets its own scrollbar and clamps to the screen height; click the small close button on it (or call `SetPoppedOut(false)`) to dock it back into place.

```lua
local Combat = Tab:AddGroupbox({ Name = "Combat", PopOut = true }) -- starts popped out

Tab:AddButton({
  Name = "Pop Out Combat",
  Callback = function() Combat:SetPoppedOut(not Combat.PoppedOut) end
})
```

**Tabs inside a groupbox.** `AddTabbox` puts a row of small icon (or text) tabs at the top of whatever it's added to, each one switching to its own set of elements underneath — handy for packing several related panels (buy / sell / auto, say) into one card instead of a long scroll. Works on a `Tab` directly too, not just a `Groupbox`.

```lua
local Economy = Tab:AddGroupbox({ Name = "Economy" })
local Tabs = Economy:AddTabbox()

local BuyTab = Tabs:AddTab({ Icon = "dollar-sign" }) -- fuzzy-matched name, or pass a raw rbxassetid://...
local SellTab = Tabs:AddTab({ Icon = "refresh-cw" })
local UpgradeTab = Tabs:AddTab({ Icon = "arrow-up" })
local AutoTab = Tabs:AddTab({ Icon = "zap" })

BuyTab:AddLabel("Planned Order")
BuyTab:AddDropdown({ Name = "Select Items", Options = { "Bisonte Giuppitere", "Esok Sekolah" }, MultiSelect = true })

AutoTab:AddToggle({ Name = "Wait for 100% Luck Boost", Default = false, Flag = "wait_luck", Callback = function(v) end })
AutoTab:AddToggle({ Name = "Auto Fuse", Default = false, Flag = "auto_fuse", Callback = function(v) end })
```

Prefer text over icons? Pass `Name`/`Title` instead of `Icon` on `AddTab` and it renders as a small label instead. The first tab added is selected by default — pass `Default = true` on a later `AddTab` call to start on that one instead. Each returned tab page supports every element method a `Tab` does, same as a groupbox.

---

## 🎛️ Elements

### Section

A plain header/divider label — not a bordered container (see Groupbox above for that).

```lua
Tab:AddSection("Section Title")
```

### Toggle

```lua
Tab:AddToggle({
  Name     = "Auto Farm", -- [1]
  Default  = false,       -- [2]
  Callback = function(value) end, -- [3]
  Flag     = "auto_farm", -- [4]
  Badge    = "New",       -- optional
})
```

### Button

```lua
Tab:AddButton({
  Name     = "My Button", -- [1]
  Callback = function() end, -- [2]
  Debounce = 0.5,          -- optional cooldown in seconds (also accepts Cooldown)
  Badge    = "Hot",        -- optional
})
```

### TextBox

```lua
Tab:AddTextBox({
  Name            = "Webhook",     -- [1]
  Default         = "",            -- [2]
  Callback        = function(text) end, -- [3], fires on focus lost
  Flag            = "webhook_url", -- [4]
  Placeholder     = "Paste URL...",
  ClearOnFocus    = false,
})
```

Methods: `SetText(str)`, `GetValue()`, `SetPlaceholder(str)`, `CaptureFocus()`, `Clear()`, `SetTextFilter(fn)` (runs on focus-lost, return a string to overwrite the text).

### Keybind

```lua
Tab:AddKeybind({
  Name     = "Toggle ESP",           -- [1]
  Default  = Enum.KeyCode.E,         -- [2]
  Callback = function(keyCode) end,  -- [3]
  Flag     = "esp_key",              -- [4]
})
```

Click the box, then press a key to rebind. `SetValue(keyCode)` sets it programmatically (expects an `EnumItem`).

### Slider

```lua
Tab:AddSlider({
  Name      = "Walk Speed", -- [1]
  Min       = 16,           -- [2]
  Max       = 200,          -- [3]
  Increment = 1,            -- [4]
  Default   = 16,           -- [5]
  Callback  = function(value) end, -- [6]
  Flag      = "walk_speed", -- [7]
  Badge     = "Fixed",
})
```

### Dropdown

```lua
Tab:AddDropdown({
  Name        = "Target Mode", -- [1]
  Options     = { "Closest", "Lowest HP", "Random" }, -- [2]
  Default     = "Closest",     -- [3], a table of names if MultiSelect
  Callback    = function(value) end, -- [4]
  Flag        = "target_mode", -- [5]
  MultiSelect = false,
})
```

Methods: `Add(...)`, `Remove(...)`, `NewOptions(...)` (clears then adds), `GetOptionsCount()`.

> Dropdown selections aren't currently restored by `LoadConfig`/`ResetConfig` — they still auto-persist to `ScriptFolder/ScriptFlags.json` and are read back on the next run like every other flag, they just aren't part of a named config slot yet.

### ColorPicker

```lua
Tab:AddColorPicker({
  Name     = "ESP Color",                 -- [1]
  Default  = Color3.fromRGB(255, 0, 0),   -- [2]
  Callback = function(color) end,         -- [3]
  Flag     = "esp_color",                 -- [4]
})
```

Click the swatch to open an inline R/G/B picker. `SetValue(color3)` sets it programmatically.

### Label

```lua
Tab:AddLabel("Just some text")
Tab:AddLabel({ Text = "Coloured text", Color = Color3.fromRGB(255, 200, 0) })
```

Methods: `SetText(str)`, `SetColor(color3)`.

### GradientLabel

```lua
Tab:AddGradientLabel("VonLib")
Tab:AddGradientLabel({
  Text    = "VonLib",
  Colors  = { Color3.fromRGB(120, 80, 255), Color3.fromRGB(80, 200, 255) },
  Rotation = 45,
})
```

Methods: `SetText(str)`, `SetGradient(colors, rotation?)`.

### Paragraph

```lua
Tab:AddParagraph("Title", "Body text that can wrap across multiple lines.")
```

Two plain string arguments — not a config table.

### DiscordInvite

```lua
Tab:AddDiscordInvite({
  Name    = "Join our Discord", -- [1], shown above the card
  Invite  = "https://discord.gg/yourcode", -- required, also accepts Link
  Icon    = "rbxassetid://...", -- optional server icon, also accepts Image/Logo
  Banner  = Color3.fromRGB(88, 101, 242), -- optional, Color3 or named colour string
  Online  = 128,  -- optional member-online count, also accepts MembersOnline
  Members = 900,  -- optional total member count, also accepts TotalMembers
})
```

---

## 🔗 Common element methods

Every element returned by `Tab:AddX(...)` (and Groupbox) supports:

| Method | Description |
|---|---|
| `SetTitle(title)` | Update displayed title |
| `SetDescription(desc)` | Update description text |
| `SetVisible(bool)` | Show or hide the element |
| `Destroy()` | Remove the element entirely |
| `AddCallback(fn)` | Attach an additional callback |
| `ApplyBadge(label)` | Apply a status badge after creation |

---

## 🔖 Badges

Attach a coloured status pill to any element title, either inline via config or after creation.

```lua
Tab:AddToggle({ Name = "Auto Farm", Badge = "New",   Default = false, Callback = function(v) end })
Tab:AddButton({ Name = "Teleport",  Badge = "Hot",   Callback = function() end })

local toggle = Tab:AddToggle({ Name = "ESP", Default = false, Callback = function(v) end })
toggle:ApplyBadge("Beta")
```

**Available badges:** `Bug` 🔴 · `New` 🟢 · `Warning` 🟡 · `Fixed` 🔵 · `Beta` 🟣 · `Hot` 🟠 · `Soon` ⚫

---

## 🏷️ Tags

Add coloured badge pills to the window topbar.

```lua
local MyTag = Window:Tag({
  Title = "v2.0.0",
  Color = "Amber",              -- named colour or Color3
  Icon  = "rbxassetid://...",   -- optional icon
})

MyTag:SetTitle("v2.0.0")
MyTag:SetColor("Green")
MyTag:SetIcon("")
MyTag:Destroy()
```

**Named colours:** `Amber` · `Red` · `Green` · `Blue` · `Purple` · `Pink` · `Cyan` · `Orange` · `Gray` · `White` · `Default`

---

## 🔔 Notifications

```lua
Window:Notify({
  Name     = "Success",       -- [1] required
  Content  = "Farm started.", -- [2] required
  Icon     = "rbxassetid://...", -- [3] optional
  Duration = 5,                -- [4] optional, seconds (default 5)
})
```

Returns an object with `:Close()`. Hover a notification to pause its countdown.

**Notify groups** let you set shared defaults once and omit them on individual calls:

```lua
local ErrorGroup = Window:NewNotifyGroup({ Name = "Error", Icon = "rbxassetid://...", Duration = 8 })
ErrorGroup:Notify({ Content = "Something broke." }) -- reuses Name/Icon/Duration
```

`Window:SetNotifyDefaultIcon(icon)` sets a fallback icon for any `Notify` call that doesn't specify one.

---

## 💬 Dialogs

A modal dialog with one or more option buttons.

```lua
Window:Dialog({
  Title   = "Are you sure?",
  Content = "This will reset all your settings.",
  Options = {
    { "Cancel", function() end },
    { "Confirm", function() print("confirmed") end },
  },
})
```

Only one dialog can be open at a time — opening a new one closes the previous.

---

## 💾 Config & Flags

Any element that accepts a `Flag` participates in two independent persistence layers:

**Auto-persisted flags.** Every value change is written to `ScriptFolder/ScriptFlags.json` automatically (debounced), and read back the next time the same flag is created — this needs no extra code and just works as long as `ScriptFolder` is set.

```lua
Window:GetFlag("auto_farm")
Window:SetFlag("auto_farm", true)
Window:DeleteFlags() -- clears everything
```

**Named config slots.** Save/load whole snapshots of every flagged element under a name, independent of the always-on autosave above — useful for "PVP" vs "Farming" presets.

```lua
Window:SaveConfig("PVP")          -- defaults to "Default" if no name given
Window:LoadConfig("PVP")          -- pushes saved values into the live UI
Window:ListConfigs()              -- {"PVP", "Farming", ...}
Window:DeleteConfig("PVP")
Window:ResetConfig()              -- reset every flagged element to its script default
Window:ResetConfig("PVP")         -- also deletes that slot
```

Config slots are written to `ScriptFolder/configs/{name}.json`. Toggle, Slider, TextBox, Keybind and ColorPicker are fully supported; Dropdown selections are covered by the auto-persist layer but not yet by named slots (see the Dropdown note above).

---

## ⌨️ Minimizer

```lua
Window:NewMinimizer(Enum.KeyCode.LeftControl)
-- or: Window:NewMinimizer({ KeyCode = Enum.KeyCode.LeftControl })
```

Also shows a minimize button automatically on mobile. `minimizer:SetKeyCode(newKeyCode)` rebinds it.

---

## 🎬 Background Video

```lua
Window:SetBackgroundVideo("rbxassetid://...", 0.4) -- url/id, dim overlay 0-1
Window:PauseBackgroundVideo()
Window:PlayBackgroundVideo()
Window:SetBackgroundVideoOverlay(0.6)
```

Or set it at creation: `Library:MakeWindow({ ..., BackgroundVideo = "rbxassetid://...", BackgroundVideoOverlay = 0.4 })`.

---

## 📊 Stats HUD

A draggable floating overlay that displays live game statistics, independent of the main window — it stays visible even when the menu is minimized, and follows the active theme (built-in or custom) like everything else.

```lua
-- Default stats: Ping, FPS, Playtime, Time
local HUD = Library:MakeStatsHUD()

-- Custom selection
local HUD = Library:MakeStatsHUD({
  Title    = "My Hub",                 -- shown on the drag handle (default "VonLib")
  Width    = 140,                      -- HUD width in pixels (default 130)
  Stats    = { "Ping", "FPS", "Playtime", "Time", "Memory", "Players" },
  Position = UDim2.new(0, 8, 0.5, 0),  -- starting position (optional)
  Visible  = true,                     -- shown by default
})

HUD:SetTitle("Renamed Hub")
print(HUD:GetTitle())

HUD:SetVisible(false)
HUD:SetVisible(true)
print(HUD:IsVisible())

-- Add a fully custom stat with any live value
HUD:AddStat("Speed", function()
  local char = game.Players.LocalPlayer.Character
  local hum = char and char:FindFirstChild("Humanoid")
  return tostring(hum and hum.WalkSpeed or 0) .. " stud/s"
end)

HUD:RemoveStat("Memory")
HUD:SetPosition(UDim2.new(1, -120, 0, 8))
HUD:Destroy()
```

**Built-in stat keys:** `Ping` · `FPS` · `Playtime` · `Time` · `Memory` · `Players` · `Game`

---

## 🖼️ Icons

Anywhere an `Icon`/`Image` parameter is accepted (tabs, buttons, tags, notifications, Discord invites), you can pass either:

- a raw asset string like `"rbxassetid://1234567"`, used as-is, or
- a short name (e.g. `"home"`, `"settings"`) that's fuzzy-matched against a large built-in icon pack, cached to `ScriptFolder/Icons.lua` after the first fetch.

`Library:GetIconByName(name)` resolves either form directly if you need the asset ID yourself.

---

## 🧵 Full example

```lua
local Library = loadstring(game:HttpGet(
  "https://raw.githubusercontent.com/VoidDeveloper67/VonLib/refs/heads/main/main.luau"
))()

local Window = Library:MakeWindow({
  Title        = "Example Hub : Game Name",
  SubTitle     = "by you",
  ScriptFolder = "examplehub",
})

Window:NewMinimizer(Enum.KeyCode.LeftControl)
Window:Tag({ Title = "v2.0.0", Color = "Amber" })

local MainTab = Window:MakeTab({ "Main", "home" })
local ConfigTab = Window:MakeTab({ "Config", "settings" })

-- Groupboxes are the main way to organize a tab: each is its own bordered
-- card, and every Tab method (AddToggle, AddSlider, even AddGroupbox
-- again for nesting) works exactly the same way when called on one.
local Combat = MainTab:AddGroupbox({
  Name        = "Combat",
  Description = "Aim and damage related settings",
})
Combat:AddToggle({ Name = "Silent Aim", Default = false, Flag = "silent_aim", Callback = function(v) end })
Combat:AddSlider({ Name = "FOV", Min = 10, Max = 200, Default = 90, Flag = "fov", Callback = function(v) end })

-- A groupbox nested inside another one, starting collapsed
local Prediction = Combat:AddGroupbox({ Name = "Prediction", Collapsed = true })
Prediction:AddToggle({ Name = "Enabled", Default = true, Flag = "prediction_enabled", Callback = function(v) end })
Prediction:AddSlider({ Name = "Strength", Min = 0, Max = 100, Default = 50, Flag = "prediction_strength", Callback = function(v) end })

local Visuals = MainTab:AddGroupbox({ Name = "Visuals", Collapsed = true })
Visuals:AddToggle({ Name = "ESP", Default = false, Flag = "esp_enabled", Callback = function(v) end })
Visuals:AddColorPicker({ Name = "ESP Color", Default = Color3.fromRGB(255, 0, 0), Flag = "esp_color", Callback = function(c) end })

-- A groupbox holding a Tabbox: several icon-switched panels in one card
local Economy = MainTab:AddGroupbox({ Name = "Economy" })
local EconomyTabs = Economy:AddTabbox()

local BuyTab = EconomyTabs:AddTab({ Icon = "dollar-sign" })
local SellTab = EconomyTabs:AddTab({ Icon = "refresh-cw" })
local UpgradeTab = EconomyTabs:AddTab({ Icon = "arrow-up" })
local AutoTab = EconomyTabs:AddTab({ Icon = "zap" })

BuyTab:AddDropdown({ Name = "Select Items", Options = { "Bisonte Giuppitere", "Esok Sekolah" }, MultiSelect = true })
SellTab:AddButton({ Name = "Sell All", Callback = function() end })
UpgradeTab:AddButton({ Name = "Upgrade Slot", Callback = function() end })
AutoTab:AddToggle({ Name = "Wait for 100% Luck Boost", Default = false, Flag = "wait_luck", Callback = function(v) end })
AutoTab:AddToggle({ Name = "Auto Fuse", Default = false, Flag = "auto_fuse", Callback = function(v) end })

-- Popped out so it stays visible on screen while you use other tabs
local Watermark = MainTab:AddGroupbox({ Name = "Watermark", PopOut = true })
Watermark:AddLabel("Example Hub | " .. game:GetService("Players").LocalPlayer.Name)

local Theming = ConfigTab:AddGroupbox({ Name = "Theme" })
Theming:AddDropdown({
  Name     = "Theme",
  Options  = Library:GetThemes(),
  Default  = Library:GetTheme().Name,
  Callback = function(v) Library:SetTheme(v) end,
})

local Configs = ConfigTab:AddGroupbox({
  Name        = "Config Slots",
  Description = "Save and load full setting presets",
})
Configs:AddButton({ Name = "Save",  Callback = function() Window:SaveConfig("Default") end })
Configs:AddButton({ Name = "Load",  Callback = function() Window:LoadConfig("Default") end })
Configs:AddButton({ Name = "Reset", Callback = function() Window:ResetConfig() end })

local HUD = Library:MakeStatsHUD({ Stats = { "Ping", "FPS", "Playtime" } })

Window:Notify({ Name = "Loaded", Content = "Example Hub is ready.", Duration = 4 })
```
