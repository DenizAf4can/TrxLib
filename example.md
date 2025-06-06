# TrxLib UI Library Documentation

TrxLib is a versatile UI library for Roblox, designed to create feature-rich and themeable interfaces for your scripts.

## Table of Contents

1.  [Loading the Library](#loading-the-library)
2.  [Initialization (`TrxLib.New`)](#initialization-trxlibnew)
    *   [Options](#options)
3.  [Core Structure](#core-structure)
    *   [Creating Categories (`Window:CreateCategory`)](#creating-categories-windowcreatecategory)
    *   [Creating SubTabs (`Category:CreateSubTab`)](#creating-subtabs-categorycreatesubtab)
4.  [Adding Controls (Elements)](#adding-controls-elements)
    *   [Label (`SubTab:AddLabel`)](#label-subtabaddlabel)
    *   [Button (`SubTab:AddButton`)](#button-subtabaddbutton)
    *   [Toggle (`SubTab:AddToggle`)](#toggle-subtabaddtoggle)
    *   [Text Input (`SubTab:AddTextInput`)](#text-input-subtabaddtextinput)
    *   [Slider (`SubTab:AddSlider`)](#slider-subtabaddslider)
    *   [Dropdown (`SubTab:AddDropdown`)](#dropdown-subtabadddropdown)
    *   [Range Slider (`SubTab:AddRangeSlider`)](#range-slider-subtabaddrangeslider)
    *   [Keybind (`SubTab:AddKeybind`)](#keybind-subtabaddkeybind)
    *   [Separator (`SubTab:AddSeparator`)](#separator-subtabaddseparator)
    *   [Color Picker (`SubTab:AddColorPicker`)](#color-picker-subtabaddcolorpicker)
    *   [Section (`SubTab:AddSection`)](#section-subtabaddsection)
5.  [Tooltips (`ControlAPI:AddTooltip`)](#tooltips-controlapiaddtooltip)
6.  [Themes](#themes)
    *   [Applying a Theme (`Window:ApplyTheme`)](#applying-a-theme-windowapplytheme)
    *   [Available Themes (`TrxLib.Themes`)](#available-themes-trxlibthemes)
7.  [Notifications (`Window:Notification`)](#notifications-windownotification)
8.  [Full Example](#full-example)

## Loading the Library

To use TrxLib, load it into your script environment like this:

```lua
local TrxLib = loadstring(game:HttpGet("https://trxdent.com/dashboard/xLib.lua"))()
```

## Initialization (`TrxLib.New`)

The first step is to create a new UI window instance.

```lua
local Window = TrxLib.New({
    HubName = "My Awesome Hub",
    ScriptName = "Cool Script",
    Version = "v1.0.2",
    WindowSize = UDim2.new(0, 750, 0, 500),
    InitialTheme = "mintyfresh"
})
```

### Options

The `TrxLib.New()` function accepts a table of options:

| Option             | Type                                   | Default                                                                       | Description                                                                 |
| :----------------- | :------------------------------------- | :---------------------------------------------------------------------------- | :-------------------------------------------------------------------------- |
| `HubName`          | `string`                               | `"TrxLib Hub"`                                                                | The main title displayed on the top-left of the window.                     |
| `ScriptName`       | `string`                               | `nil`                                                                         | Name of your script, displayed as a pill next to the HubName.               |
| `Version`          | `string`                               | `nil`                                                                         | Version string, displayed next to the ScriptName pill.                      |
| `WindowSize`       | `UDim2`                                | `UDim2.new(0, 800, 0, 550)`                                                   | The size of the main UI window.                                             |
| `UserInfo`         | `table`                                | `{ Name = LocalPlayer.Name, Tag = "@" .. LocalPlayer.Name }`                  | User information displayed at the bottom-left. `{ Name = "User", Tag = "" }` |
| `InitialTheme`     | `string`                               | `"default"`                                                                   | The key of the theme to apply on startup (e.g., "default", "mintyfresh"). |
| `MiniLogoImage`    | `string` (Roblox Asset ID)             | Theme-dependent (`"rbxassetid://1818497369"` for default)                     | Asset ID for the image shown when the UI is minimized.                      |
| `MiniLogoSize`     | `UDim2`                                | `UDim2.new(0, 45, 0, 45)`                                                     | Size of the minimized logo button.                                          |
| `MiniLogoPosition` | `UDim2`                                | Centered on screen                                                            | Position of the minimized logo button.                                      |
| `CloseAction`      | `string`                               | `"destroy"`                                                                   | Action on close button click: `"destroy"` (removes UI) or `"hide"`.         |
| `DisplayOrder`     | `number`                               | `999`                                                                         | `DisplayOrder` for the ScreenGui.                                           |
| `HighestZIndex`    | `number`                               | `20000`                                                                       | Base ZIndex for notifications to ensure they appear on top.                 |

## Core Structure

The UI is organized into Categories and SubTabs.

### Creating Categories (`Window:CreateCategory`)

Categories are the main collapsible sections in the left navigation panel.

```lua
local myCategory = Window:CreateCategory("Main Features")
```

*   **Parameters:**
    *   `categoryName` (`string`): The name of the category.
*   **Returns:** `Category` object.

### Creating SubTabs (`Category:CreateSubTab`)

SubTabs are pages within a category. Selecting a SubTab displays its content in the main content area.

```lua
local mainFeaturesTab = myCategory:CreateSubTab("General")
local combatTab = myCategory:CreateSubTab("Combat")
```

*   **Parameters:**
    *   `subTabName` (`string`): The name of the subtab.
*   **Returns:** `SubTab` object, which has methods to add controls.

## Adding Controls (Elements)

Controls are added to `SubTab` objects. Most `Add<Control>` methods return an API object for that specific control, allowing you to interact with it later (e.g., get/set its value).

### Label (`SubTab:AddLabel`)

Displays text. Can be used as a simple label or a title.

```lua
mainFeaturesTab:AddLabel("Welcome!", "This is a description for the welcome message.", true) -- Title style
mainFeaturesTab:AddLabel("Informational Text", "Some more details here.")
```

*   **Parameters:**
    | Parameter                | Type      | Default | Description                                  |
    | :----------------------- | :-------- | :------ | :------------------------------------------- |
    | `labelTextContent`       | `string`  | `""`    | The main text of the label.                  |
    | `descriptionTextContent` | `string`  | `nil`   | Optional smaller text below the main label.  |
    | `isTitle`                | `boolean` | `false` | If `true`, applies a bolder, larger font.    |
*   **Returns:** `LabelAPI` object.
*   **`LabelAPI` Methods:**
    *   `SetText(text)`: Updates the main label text.
    *   `SetDescription(desc)`: Updates the description text.

### Button (`SubTab:AddButton`)

A standard clickable button.

```lua
mainFeaturesTab:AddButton("Click Me", "This button does something.", function()
    print("Button clicked!")
    Window:Notification({ Title = "Action", Desc = "Button was clicked!", Type = "success" })
end)
```

*   **Parameters:**
    | Parameter                | Type       | Default | Description                                                     |
    | :----------------------- | :--------- | :------ | :-------------------------------------------------------------- |
    | `buttonTextContent`      | `string`   | `""`    | The text displayed on the button.                               |
    | `descriptionTextContent` | `string`   | `nil`   | Optional description text to the left of the button.            |
    | `callbackFunction`       | `function` | `nil`   | Function to execute when the button is clicked.                 |
*   **Returns:** `ButtonAPI` object.
*   **`ButtonAPI` Methods:**
    *   `SetText(text)`: Updates the button's text.

### Toggle (`SubTab:AddToggle`)

A checkbox for boolean (on/off) states.

```lua
local myToggle = mainFeaturesTab:AddToggle("Enable Feature", "Toggles a cool feature.", true, function(newValue)
    print("Toggle changed to:", newValue)
    if newValue then
        -- Feature enabled
    else
        -- Feature disabled
    end
end)

-- Later, you can get or set its value:
-- local currentValue = myToggle:GetValue()
-- myToggle:SetValue(false)
```

*   **Parameters:**
    | Parameter                | Type       | Default | Description                                                          |
    | :----------------------- | :--------- | :------ | :------------------------------------------------------------------- |
    | `labelTextContent`       | `string`   | `""`    | Label text for the toggle.                                           |
    | `descriptionTextContent` | `string`   | `nil`   | Optional description.                                                |
    | `initialValue`           | `boolean`  | `false` | The starting state of the toggle.                                    |
    | `callbackFunction`       | `function` | `nil`   | Function called when the toggle state changes. Receives `newValue` (boolean). |
*   **Returns:** `ToggleAPI` object.
*   **`ToggleAPI` Methods:**
    *   `GetValue()`: Returns the current boolean state.
    *   `SetValue(value)`: Sets the boolean state and triggers the callback.

### Text Input (`SubTab:AddTextInput`)

A field for users to enter text.

```lua
local nameInput = mainFeaturesTab:AddTextInput("Player Name", "Enter target player's name.", "Username", "", function(textEntered)
    print("Text input focused lost / enter pressed:", textEntered)
end)

-- nameInput:SetValue("Trx")
```

*   **Parameters:**
    | Parameter                   | Type       | Default   | Description                                                                  |
    | :-------------------------- | :--------- | :-------- | :--------------------------------------------------------------------------- |
    | `labelTextContent`          | `string`   | `""`      | Label for the text input.                                                    |
    | `descriptionTextContent`    | `string`   | `nil`     | Optional description.                                                        |
    | `placeholderTextContent`    | `string`   | `"..."`   | Placeholder text shown when the input is empty.                              |
    | `initialValue`              | `string`   | `""`      | The initial text in the input field.                                         |
    | `callbackOnAnyFocusLost`    | `function` | `nil`     | Function called when focus is lost or Enter is pressed. Receives `text` (string). |
*   **Returns:** `TextInputAPI` object.
*   **`TextInputAPI` Methods:**
    *   `GetValue()`: Returns the current text content.
    *   `SetValue(text)`: Sets the text content.

### Slider (`SubTab:AddSlider`)

Allows selection of a numerical value within a range.

```lua
local speedSlider = mainFeaturesTab:AddSlider("Speed", "Adjust movement speed.", 0, 200, 100, function(value)
    print("Speed slider set to:", value)
end)

-- speedSlider:SetValue(150)
```

*   **Parameters:**
    | Parameter       | Type       | Default | Description                                                          |
    | :-------------- | :--------- | :------ | :------------------------------------------------------------------- |
    | `labelText`     | `string`   | `""`    | Label for the slider.                                                |
    | `descriptionText`| `string`   | `nil`   | Optional description.                                                |
    | `minValue`      | `number`   | `0`     | The minimum value of the slider.                                     |
    | `maxValue`      | `number`   | `100`   | The maximum value of the slider.                                     |
    | `initialValue`  | `number`   | `minValue` | The starting value of the slider.                                  |
    | `callback`      | `function` | `nil`   | Function called when the slider value changes. Receives `value` (number). |
*   **Returns:** `SliderAPI` object.
*   **`SliderAPI` Methods:**
    *   `GetValue()`: Returns the current numerical value.
    *   `SetValue(value)`: Sets the numerical value and triggers the callback.

### Dropdown (`SubTab:AddDropdown`)

Allows selection from a list of items. Can be single-select or multi-select.

```lua
local items = {"Option A", "Option B", "Another Option", "The Last One"}
local singleDropdown = mainFeaturesTab:AddDropdown("Choose One", "Select a single option.", items, items[2], function(selectedValue)
    print("Single dropdown selected:", selectedValue)
end)

local multiDropdown = mainFeaturesTab:AddDropdown("Choose Many", "Select multiple options.", items, {items[1], items[3]}, function(selectedTable)
    print("Multi dropdown selected:")
    for _, v in pairs(selectedTable) do
        print("- " .. v)
    end
end, { MultiSelect = true, SearchableThreshold = 3 })

-- singleDropdown:SetValue(items[3])
-- multiDropdown:SetItems({"New Item 1", "New Item 2"}, {"New Item 1"})
```

*   **Parameters:**
    | Parameter             | Type                          | Default              | Description                                                                                                                                    |
    | :-------------------- | :---------------------------- | :------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------- |
    | `labelText`           | `string`                      | `""`                 | Label for the dropdown.                                                                                                                        |
    | `descriptionText`     | `string`                      | `nil`                | Optional description.                                                                                                                          |
    | `itemsArray`          | `array` of `string` or `any`  | `{"(No Items)"}`     | An array of items to display. Items will be converted to strings for display.                                                                  |
    | `initialValueOrValues`| `any` or `array` of `any`     | First item / `nil`   | The initial selected item (for single-select) or an array of initial selected items (for multi-select). Values should match items in `itemsArray`. |
    | `callbackFunction`    | `function`                    | `nil`                | Function called on selection change. Receives `selectedValue` or `selectedTable` (array).                                                      |
    | `options`             | `table`                       | `{}`                 | Additional options (see below).                                                                                                                |
*   **Dropdown `options` Table:**
    | Option                      | Type      | Default               | Description                                                                       |
    | :-------------------------- | :-------- | :-------------------- | :-------------------------------------------------------------------------------- |
    | `MultiSelect`               | `boolean` | `false`               | If `true`, allows multiple items to be selected.                                    |
    | `SearchableThreshold`       | `number`  | `10`                  | Minimum number of items for the search box to appear.                             |
    | `MaxVisibleItems`           | `number`  | `6`                   | Maximum number of items visible in the dropdown list before scrolling.            |
    | `NoSelectionTextSingle`     | `string`  | `"Select"`            | Text displayed on the button when no item is selected (single-select).            |
    | `NoSelectionTextMulti`      | `string`  | `"No selected items"` | Text displayed on the button when no items are selected (multi-select).           |
    | `ItemsSelectedTextMulti`    | `string`  | `" item(s) selected"` | Suffix for multi-select button text when multiple items are selected (e.g., "3 item(s) selected"). |
    | `MaxItemsInButtonTextMulti` | `number`  | `2`                   | Max number of selected item names to show directly in button text for multi-select. |
    | `NoResultsMessage`          | `string`  | `"(Not Found)"`       | Message shown in dropdown list when search yields no results.                     |
    | `EmptyListMessage`          | `string`  | `"(List Empty)"`      | Message shown if `itemsArray` is empty.                                           |
*   **Returns:** `DropdownAPI` object.
*   **`DropdownAPI` Methods:**
    *   `GetValue()`: Returns the selected item or an array of selected items (if multi-select).
    *   `SetValue(valueOrValues)`: Sets the selected item(s) and triggers the callback.
    *   `SetItems(newItemsArray, newInitialValueOrValues)`: Replaces the dropdown's items and optionally sets a new initial selection.
    *   `Close()`: Closes the dropdown if it's open.
    *   `IsOpen()`: Returns `true` if the dropdown list is currently open.

### Range Slider (`SubTab:AddRangeSlider`)

Allows selection of a minimum and maximum value within a defined range.

```lua
local priceRange = mainFeaturesTab:AddRangeSlider("Price Range", "Filter by price.", 0, 1000, 100, 500, function(minVal, maxVal)
    print("Price range set to:", minVal, "-", maxVal)
end)

-- priceRange:SetValues(200, 750)
```

*   **Parameters:**
    | Parameter         | Type       | Default      | Description                                                                |
    | :---------------- | :--------- | :----------- | :------------------------------------------------------------------------- |
    | `labelText`       | `string`   | `""`         | Label for the range slider.                                                |
    | `descriptionText` | `string`   | `nil`        | Optional description.                                                      |
    | `minValue`        | `number`   | `0`          | The absolute minimum value of the range.                                   |
    | `maxValue`        | `number`   | `100`        | The absolute maximum value of the range.                                   |
    | `initialMin`      | `number`   | `minValue`   | The starting minimum value of the selected range.                          |
    | `initialMax`      | `number`   | `maxValue`   | The starting maximum value of the selected range.                          |
    | `callback`        | `function` | `nil`        | Function called on value change. Receives `minVal` (number), `maxVal` (number). |
*   **Returns:** `RangeSliderAPI` object.
*   **`RangeSliderAPI` Methods:**
    *   `GetValues()`: Returns `currentMin, currentMax`.
    *   `SetValues(newMin, newMax)`: Sets the selected range and triggers the callback.

### Keybind (`SubTab:AddKeybind`)

Allows users to bind a keyboard key or mouse button to an action.

```lua
local jumpKeybind = mainFeaturesTab:AddKeybind("Jump Key", "Set your jump key.", Enum.KeyCode.Spacebar, function(newKeyCode)
    print("Jump keybind set to:", newKeyCode.Name)
end)

-- local currentBind = jumpKeybind:GetKeybind()
-- jumpKeybind:SetKeybind(Enum.KeyCode.LeftControl)
-- jumpKeybind:ClearKeybind()
```

*   **Parameters:**
    | Parameter         | Type            | Default              | Description                                                        |
    | :---------------- | :-------------- | :------------------- | :----------------------------------------------------------------- |
    | `labelText`       | `string`        | `""`                 | Label for the keybind.                                             |
    | `descriptionText` | `string`        | `nil`                | Optional description.                                              |
    | `initialKeyCode`  | `Enum.KeyCode`  | `Enum.KeyCode.Unknown` | The starting keybind.                                            |
    | `callback`        | `function`      | `nil`                | Function called when keybind changes. Receives `newKeyCode` (Enum.KeyCode). |
*   **Returns:** `KeybindAPI` object.
*   **`KeybindAPI` Methods:**
    *   `GetKeybind()`: Returns the current `Enum.KeyCode`.
    *   `SetKeybind(keyCode)`: Sets the keybind.
    *   `ClearKeybind()`: Sets the keybind to `Enum.KeyCode.Unknown`.
    *   `IsBinding()`: Returns `true` if the keybind is currently waiting for user input.

### Separator (`SubTab:AddSeparator`)

Adds a visual horizontal line to separate controls.

```lua
mainFeaturesTab:AddSeparator()
```

*   **Parameters:** None.
*   **Returns:** A simple API object primarily containing the `Gui` element.

### Color Picker (`SubTab:AddColorPicker`)

Allows users to pick a color using RGB or Hex input.

```lua
local primaryColor = mainFeaturesTab:AddColorPicker("Primary Color", "Select the main theme color.", Color3.fromRGB(0, 122, 204), function(newColor)
    print("Primary color changed to:", newColor)
    -- Window:ApplyTheme(...) or update elements with newColor
end)

-- local currentColor3 = primaryColor:GetColor()
-- primaryColor:SetColor(Color3.new(1, 0, 0))
```

*   **Parameters:**
    | Parameter                | Type         | Default             | Description                                                            |
    | :----------------------- | :----------- | :------------------ | :--------------------------------------------------------------------- |
    | `labelTextContent`       | `string`     | `""`                | Label for the color picker.                                            |
    | `descriptionTextContent` | `string`     | `nil`               | Optional description.                                                  |
    | `initialColor`           | `Color3`     | `Color3.new(1,1,1)` | The starting color.                                                    |
    | `callbackFunction`       | `function`   | `nil`               | Function called when color changes (after valid input). Receives `newColor` (Color3). |
*   **Returns:** `ColorPickerAPI` object.
*   **`ColorPickerAPI` Methods:**
    *   `GetColor()`: Returns the current `Color3` value.
    *   `SetColor(color3Value)`: Sets the color.
    *   `IsOpen()`: Returns `true` if the mode selection (RGB/Hex) list is open.
    *   `Close()`: Closes the mode selection list.

### Section (`SubTab:AddSection`)

Creates a collapsible section within a SubTab, which can contain its own set of controls.

```lua
local advancedSettingsSection = mainFeaturesTab:AddSection("Advanced Settings")

-- Now you can add controls to advancedSettingsSection just like a SubTab:
advancedSettingsSection:AddToggle("Expert Mode", "Enables expert configurations.", false, function(val)
    print("Expert mode:", val)
end)
advancedSettingsSection:AddButton("Reset Advanced", nil, function()
    print("Advanced settings reset!")
end)
```

*   **Parameters:**
    | Parameter    | Type     | Default | Description                    |
    | :----------- | :------- | :------ | :----------------------------- |
    | `sectionTitle` | `string` | `""`    | The title of the collapsible section. |
*   **Returns:** `SectionContentAPI` object. This object has the same `Add<Control>` methods as a `SubTab` object (e.g., `AddToggle`, `AddButton`).
*   **`SectionContentAPI` Properties:**
    *   `IsExpanded` (`boolean`): Controls if the section is currently expanded or collapsed. You can read/write this.
    *   All `Add<Control>` methods are available on this returned object.

## Tooltips (`ControlAPI:AddTooltip`)

Most controls (Button, Toggle, Slider, etc.) will return an API object when created. This API object has an `AddTooltip` method to attach a descriptive tooltip that appears on mouse hover.

```lua
local myButton = mainFeaturesTab:AddButton("Hover Me!", nil, function() end)
myButton:AddTooltip("This is a helpful tooltip for the button.\nIt can even have multiple lines!")

local myToggle = mainFeaturesTab:AddToggle("Info Toggle", nil, false, function() end)
myToggle:AddTooltip("Toggle this to see something happen.")
```

*   **Parameters:**
    *   `tooltipText` (`string`): The text to display in the tooltip.
*   **Returns:** The `ControlAPI` object itself (for chaining, though not typically used).

## Themes

TrxLib comes with several built-in themes and allows you to switch between them.

### Applying a Theme (`Window:ApplyTheme`)

You can change the active theme at any time.

```lua
Window:ApplyTheme("oceanbreeze") -- Applies the "Ocean Breeze" theme
-- Wait a bit for theme application if needed for subsequent UI updates
task.wait(0.1)
Window:ApplyTheme("default")    -- Switches back to the default dark theme
```

*   **Parameters:**
    *   `themeName` (`string`): The key of the theme to apply (e.g., `"default"`, `"mintyfresh"`). Case-insensitive. If the theme is not found, it defaults to "default".

### Available Themes (`TrxLib.Themes`)

The `TrxLib.Themes` table contains all available themes. You can iterate over it to get theme names or inspect their properties.

```lua
print("Available themes:")
for themeKey, themeData in pairs(TrxLib.Themes) do
    print("- " .. themeKey .. " (" .. themeData.Name .. ")")
end
```

**Predefined Theme Keys (and Names):**

*   `default` (Default Dark)
*   `mintyfresh` (Minty Fresh)
*   `oceanbreeze` (Ocean Breeze)
*   `sakuradream` (Sakura Dream)
*   `goldenhour` (Golden Hour)
*   `amethystglow` (Amethyst Glow)
*   `enhancedlight` (Enhanced Light)
*   `forestnight` (Forest Night)
*   `charcoalslate` (Charcoal Slate)
*   `sunsetorange` (Sunset Orange)
*   `cyberpunknight` (Cyberpunk Night)
*   `monochromeclassic` (Monochrome Classic)

## Notifications (`Window:Notification`)

Display temporary pop-up messages (toasts) at the top-right of the screen.

```lua
Window:Notification({
    Title = "Success!",
    Desc = "Your action was completed successfully.",
    Duration = 5, -- seconds
    Type = "success" -- "success", "error", "warning", "info", or "default"
})

Window:Notification({
    Title = "Error Occurred",
    Desc = "Something went wrong. Please try again.",
    Type = "error"
})
```

*   **Parameters:**
    *   `data` (`table`): A table containing notification details:
        | Key        | Type     | Default        | Description                                                        |
        | :--------- | :------- | :------------- | :----------------------------------------------------------------- |
        | `Title`    | `string` | "Notification" | The main title of the notification.                                |
        | `Desc`     | `string` | `nil`          | Optional detailed description.                                     |
        | `Duration` | `number` | `3`            | How long the notification stays visible (in seconds).              |
        | `Type`     | `string` | `"default"`    | Type of notification: `"success"`, `"error"`, `"warning"`, `"info"`, or `"default"`. This affects color and icon. |

## Full Example

Here's a more complete example putting several pieces together:

```lua

local TrxLib = loadstring(game:HttpGet("https://trxdent.com/dashboard/xLib.lua"))()

-- Wait for game to be fully loaded
if not game:IsLoaded() then
    game.Loaded:Wait()
end
task.wait(1) -- Extra delay for services

-- Initialize TrxLib
local Lib = TrxLib.New({
    HubName = "TrxLib Demo",
    ScriptName = "Test Script",
    Version = "v1.2.3",
    WindowSize = UDim2.new(0, 850, 0, 600),
    UserInfo = {
        Name = game:GetService("Players").LocalPlayer.DisplayName,
        Tag = "@" .. game:GetService("Players").LocalPlayer.Name
    },
    InitialTheme = "default", -- Try "mintyfresh", "oceanbreeze", "sakuradream", "cyberpunknight", etc.
    MiniLogoImage = "rbxassetid://1818497369", -- Default Roblox Studio icon, replace if you have a custom one
    MiniLogoSize = UDim2.new(0, 50, 0, 50),
    CloseAction = "destroy", -- "hide" or "destroy"
    DisplayOrder = 1000,
    HighestZIndex = 25000
})

if not Lib then
    warn("Failed to initialize TrxLib.")
    return
end

-- === CATEGORY 1: MAIN FEATURES ===
local mainFeaturesCat = Lib:CreateCategory("Main Features")

-- SUBTAB 1.1: Basic Controls
local basicControlsTab = mainFeaturesCat:CreateSubTab("Basic Controls")

basicControlsTab:AddLabel("Welcome to TrxLib!", "This tab demonstrates basic controls.", true)
basicControlsTab:AddSeparator()

local demoLabel = basicControlsTab:AddLabel("Dynamic Label:", "This label's text can be changed.")
demoLabel:AddTooltip("This is a tooltip for the dynamic label!")

local demoButton = basicControlsTab:AddButton("Click Me", "This button changes the label above.", function()
    local R,G,B = math.random(0,255), math.random(0,255), math.random(0,255)
    demoLabel:SetText("Button Clicked! Color: " .. R .. "," .. G .. "," .. B)
    Lib:Notification({
        Title = "Button Clicked!",
        Desc = "The dynamic label was updated.",
        Type = "info",
        Duration = 2
    })
end)
demoButton:AddTooltip("Clicking this button will trigger an action and a notification.")

basicControlsTab:AddSeparator()

local demoToggle = basicControlsTab:AddToggle("Enable Feature", "A simple toggle switch.", false, function(value)
    print("Toggle changed to:", value)
    demoLabel:SetDescription("Toggle is now: " .. (value and "ON" or "OFF"))
    Lib:Notification({Title = "Toggle Update", Desc = "Feature is " .. (value and "Enabled" or "Disabled"), Type = value and "success" or "warning"})
end)
demoToggle:AddTooltip("This toggle controls a hypothetical feature.")

local demoTextInput = basicControlsTab:AddTextInput("User Name", "Enter your desired username.", "Type here...", "", function(text)
    print("Text input changed to:", text)
    if text == "" then
        demoLabel:SetText("User Name cleared.")
    else
        demoLabel:SetText("User Name: " .. text)
    end
end)
demoTextInput:AddTooltip("Enter text here. It updates the label on focus lost.")

basicControlsTab:AddSeparator()

local demoSlider = basicControlsTab:AddSlider("Volume", "Adjust the volume level.", 0, 100, 50, function(value)
    print("Slider value:", value)
    demoLabel:SetText("Volume: " .. string.format("%.0f", value))
end)
demoSlider:AddTooltip("Drag or type to change the slider value.")

-- SUBTAB 1.2: Advanced Controls
local advancedControlsTab = mainFeaturesCat:CreateSubTab("Advanced Controls")

advancedControlsTab:AddLabel("Advanced Controls", "Demonstrating dropdowns, range sliders, keybinds, and color pickers.", true)
advancedControlsTab:AddSeparator()

local singleDropdown = advancedControlsTab:AddDropdown(
    "Select Fruit",
    "Choose your favorite fruit.",
    {"Apple", "Banana", "Cherry", "Date", "Elderberry", "Fig", "Grape"},
    "Banana",
    function(selectedItem)
        print("Single Dropdown selected:", selectedItem)
        Lib:Notification({Title = "Fruit Selected", Desc = "You chose: " .. tostring(selectedItem)})
    end
)
singleDropdown:AddTooltip("A single-select dropdown menu.")

local multiDropdown = advancedControlsTab:AddDropdown(
    "Select Toppings (Multi)",
    "Choose multiple toppings for your pizza.",
    {"Pepperoni", "Mushrooms", "Onions", "Olives", "Peppers", "Sausage", "Bacon", "Pineapple"},
    {"Mushrooms", "Peppers"},
    function(selectedItems)
        print("Multi Dropdown selected:", selectedItems)
        local itemsStr = table.concat(selectedItems, ", ")
        if #selectedItems == 0 then itemsStr = "None" end
        Lib:Notification({Title = "Toppings Updated", Desc = "Selected: " .. itemsStr, Type = "info"})
    end,
    {MultiSelect = true, MaxItemsInButtonTextMulti = 2}
)
multiDropdown:AddTooltip("A multi-select dropdown. Try searching!")

advancedControlsTab:AddButton("Update Single Dropdown Items", "Dynamically changes items in the fruit dropdown.", function()
    local newItems = {"Orange", "Lemon", "Lime", "Grapefruit"}
    local newInitial = "Lemon"
    singleDropdown:SetItems(newItems, newInitial)
    Lib:Notification({Title = "Dropdown Updated", Desc = "Fruit dropdown items changed.", Type = "info"})
end)

advancedControlsTab:AddSeparator()

local demoRangeSlider = advancedControlsTab:AddRangeSlider(
    "Price Range",
    "Select a minimum and maximum price.",
    0, 500, 50, 250,
    function(minVal, maxVal)
        print("Range Slider:", minVal, maxVal)
        Lib:Notification({Title = "Price Range", Desc = string.format("Min: %.0f, Max: %.0f", minVal, maxVal)})
    end
)
demoRangeSlider:AddTooltip("Adjust the minimum and maximum values of the range.")

advancedControlsTab:AddSeparator()

local demoKeybind = advancedControlsTab:AddKeybind("Action Keybind", "Set a key for this action.", Enum.KeyCode.E, function(key)
    print("Keybind set to:", key.Name)
    Lib:Notification({Title = "Keybind Set", Desc = "Action key is now: " .. key.Name})
end)
demoKeybind:AddTooltip("Click to set or change the keybind.")

advancedControlsTab:AddSeparator()

local demoColorPickerLabel = advancedControlsTab:AddLabel("Selected Color Preview", "")
local demoColorPicker = advancedControlsTab:AddColorPicker("Choose Color", "Select a color using RGB or Hex.", Color3.fromRGB(255, 0, 127), function(color)
    print("Color picked:", color)
    demoColorPickerLabel:SetText(string.format("Color: R:%.2f, G:%.2f, B:%.2f", color.R, color.G, color.B))
    -- Note: TextLabel does not have BackgroundColor3 directly, need to put it in a frame or use the color elsewhere.
    -- For simplicity, just updating text.
    Lib:Notification({Title = "Color Changed", Desc = "New color selected.", Type = "info"})
end)
demoColorPicker:AddTooltip("Pick a color. Supports RGB and Hex input, and mode switching.")


-- SUBTAB 1.3: Sections & Tooltips
local sectionsTab = mainFeaturesCat:CreateSubTab("Sections & Tooltips")
sectionsTab:AddLabel("Sections and Tooltips", "Demonstrates collapsible sections and tooltips on various elements.", true)
sectionsTab:AddSeparator()

local section1 = sectionsTab:AddSection("Collapsible Section 1")
section1:AddLabel("Inside Section 1", "This content is within the first section.")
local section1Toggle = section1:AddToggle("Section Toggle", "A toggle inside the section.", true, function(val)
    Lib:Notification({Title = "Section 1 Toggle", Desc = "Value: " .. tostring(val)})
end)
section1Toggle:AddTooltip("This toggle is nested inside Section 1.")
section1:AddButton("Section Button", "A button within the section.", function()
    Lib:Notification({Title = "Section 1 Button", Desc = "Clicked!"})
end):AddTooltip("This button is also nested.")

local section2 = sectionsTab:AddSection("Collapsible Section 2 (Initially Collapsed)")
section2.IsExpanded = false -- Manually collapse it after creation if needed (or add option to AddSection)
-- For demo, let's assume AddSection respects IsExpanded on creation (it does based on the code)
-- To ensure it's collapsed, you might need to call its expand/collapse logic if it defaults to open
-- However, the code `Rotation = sectionContentApi.IsExpanded and 0 or -90` suggests it should respect initial `IsExpanded`.

section2:AddTextInput("Nested Input", "Text input within section 2.", "Enter data...")
section2:AddSlider("Nested Slider", "", 0, 10, 5)

sectionsTab:AddSeparator()
sectionsTab:AddLabel("More Tooltips", "Hover over controls above and in other tabs to see tooltips."):AddTooltip("Even this label has a tooltip!")


-- === CATEGORY 2: SETTINGS & SYSTEM ===
local settingsCat = Lib:CreateCategory("Settings & System")

-- SUBTAB 2.1: Themes
local themesTab = settingsCat:CreateSubTab("Themes")
themesTab:AddLabel("Theme Selector", "Change the UI theme dynamically.", true)

local themeNames = {}
for name, _ in pairs(TrxLib.Themes) do
    table.insert(themeNames, TrxLib.Themes[name].Name or name) -- Use display name if available
end
table.sort(themeNames)

local currentThemeName = Lib.Theme.Name
themesTab:AddDropdown("Select Theme", "Choose a theme for the UI.", themeNames, currentThemeName, function(selectedThemeDisplayName)
    local themeKeyToApply
    for key, themeData in pairs(TrxLib.Themes) do
        if themeData.Name == selectedThemeDisplayName then
            themeKeyToApply = key
            break
        end
    end
    if themeKeyToApply then
        print("Applying theme:", themeKeyToApply)
        Lib:ApplyTheme(themeKeyToApply)
        Lib:Notification({Title = "Theme Changed", Desc = "Applied: " .. selectedThemeDisplayName, Type = "success"})
    else
        warn("Could not find theme key for display name:", selectedThemeDisplayName)
    end
end):AddTooltip("Select a theme from this dropdown to change the UI's appearance.")

themesTab:AddSeparator()
themesTab:AddLabel("Custom Theme Example", "The themes are tables that can be merged and extended.")

-- SUBTAB 2.2: Notifications
local notificationsTab = settingsCat:CreateSubTab("Notifications")
notificationsTab:AddLabel("Notification Tester", "Trigger different types of notifications.", true)

notificationsTab:AddButton("Success Notification", "Shows a success message.", function()
    Lib:Notification({
        Title = "Success!",
        Desc = "Your operation was completed successfully.",
        Type = "success",
        Duration = 3
    })
end)

notificationsTab:AddButton("Error Notification", "Shows an error message.", function()
    Lib:Notification({
        Title = "Error Occurred",
        Desc = "Something went wrong. Please try again.",
        Type = "error",
        Duration = 4
    })
end)

notificationsTab:AddButton("Warning Notification", "Shows a warning message.", function()
    Lib:Notification({
        Title = "Warning",
        Desc = "Please be cautious with this setting.",
        Type = "warning",
        Duration = 3
    })
end)

notificationsTab:AddButton("Info Notification", "Shows an informational message.", function()
    Lib:Notification({
        Title = "Information",
        Desc = "This is some useful information for you.",
        Type = "info",
        Duration = 3
    })
end)

notificationsTab:AddButton("Default Notification", "Shows a default style message.", function()
    Lib:Notification({
        Title = "Default Message",
        Desc = "This is a standard notification without a specific type.",
        Duration = 2
    })
end)

notificationsTab:AddButton("Long Notification (No Desc)", "A notification with a longer duration and no description.", function()
    Lib:Notification({
        Title = "This will stay for 7 seconds",
        Type = "default",
        Duration = 7
    })
end)

local valueDisplayLabel = basicControlsTab:AddLabel("Toggle Value Display:", "Will show the toggle's current value.")
basicControlsTab:AddButton("Show Toggle Value", "Updates the label above with the toggle's state.", function()
    local toggleState = demoToggle:GetValue()
    valueDisplayLabel:SetText("Toggle is currently: " .. (toggleState and "ON" or "OFF"))
end)

basicControlsTab:AddButton("Set Slider to 25", "Programmatically sets the slider value.", function()
    demoSlider:SetValue(25)
    Lib:Notification({Title = "Slider Set", Desc = "Volume slider set to 25 programmatically."})
end)

if Lib.ActiveSubTab == nil and #Lib.Categories > 0 and #Lib.Categories[1].SubTabs > 0 then
    Lib:SelectSubTab(Lib.Categories[1].SubTabs[1])
end
```
