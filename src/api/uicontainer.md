---
id: uicontainer
name: UIContainer
title: UIContainer
tags:
    - API
---

# UIContainer

A UIContainer is a type of UIControl. All other UI elements must be a descendant of a UIContainer to be visible. It does not have a position or size. It is always the size of the entire screen. It has no properties or functions of its own, but inherits everything from CoreObject. Inherits from [UIControl](uicontrol.md).

## Properties

| Property Name | Return Type | Description | Tags |
| -------- | ----------- | ----------- | ---- |
| `opacity` | `number` | Controls the opacity of the container's contents by multiplying the alpha component of descendants' colors. Note that other UIPanels and UIContainers in the hierarchy may also contribute their own opacity values. A resulting alpha value of 1 or greater is fully opaque, 0 is fully transparent. | Read-Write |
| `cylinderArcAngle` | `number` | When the container is rendered in 3D space, this adjusts the curvature of the canvas in degrees. Changing this value will force a redraw. | Read-Write |
| `useSafeArea` | `boolean` | When `true`, the size and position of the container is inset to avoid overlapping with a device's display elements, such as a mobile phone's notch. When `false`, the container is the same size and shape as the device's display regardless of a device's display features. This property has no effect on containers rendered in 3D space. | Read-Write |

## Functions

| Function Name | Return Type | Description | Tags |
| -------- | ----------- | ----------- | ---- |
| `GetCanvasSize()` | [`Vector2`](vector2.md) | Returns the size of the canvas when drawn in 3D space. | None |
| `SetCanvasSize(Vector2)` | `None` | Sets the size of the canvas when drawn in 3D space. | None |
| `IsCanvasReady()` | `boolean` | Returns `true` if the container has completed initialization, otherwise returns `false`. Calls to get or set the absolute position of controls within a container may not perform correctly before the container has finished initialization. | Client-Only |

## Examples

Example using:

### `SetCanvasSize`

### `GetCanvasSize`

In this example a 3D UI Container object will grow and shrink through the use of an event based animation system. For this example to work, the UI Container must have its **Is Screen Space** setting disabled.

```lua
-- Get the UI Container
local propUIContainer = script:GetCustomProperty("UIContainer"):WaitForObject()

-- Time required to scale in or scale out
local SCALE_TIME = 2

-- The calculated scale of the UI Container
local scale = 1

-- Get the original size of the canvas before scaling
local startingSize = propUIContainer:GetCanvasSize()

-- Determines whether the UI Container will be scaling in or out
-- A value of 0 means that there will be no change in scale
-- A value of 1 means that the UI Container will scale in
-- A value of -1 means that the UI Container will scale out
local scaleDirection = 0

function ScaleIn()
    scale = 0
    scaleDirection = 1
end

Events.Connect("Scale In UI", ScaleIn)

function ScaleOut()
    scale = 1
    scaleDirection = -1
end

Events.Connect("Scale Out UI", ScaleOut)

function Tick(deltaTime)
    -- If the "scaleDirection" is 0, we can immediately exit this function because no changes
    -- will be made to the scale of the UI Container.
    if scaleDirection == 0 then
        return
    end
    -- Calculate the current scale based on the "scaleDirection" and "deltaTime"
    scale = scale + (scaleDirection * deltaTime/SCALE_TIME)

    -- Clamp the calculate scale between 0 and 1
    local clampedScale = CoreMath.Clamp(scale, 0, 1)

    -- If the "clampedScale" is different from the calculated "scale" the must
    -- scaling animation must finished meaning "scaleDirection" can return to 0
    if scale ~= clampedScale then
        scaleDirection = 0
    end

    -- Update the scale of the UI Container
    propUIContainer:SetCanvasSize(startingSize * clampedScale)
end

-- Scale the UI in and then scale the UI out
Task.Spawn(function ()
    Events.Broadcast("Scale In UI")
    Task.Wait(SCALE_TIME)
    Events.Broadcast("Scale Out UI")
end)
```

See also: [Events.Broadcast](events.md) | [CoreMath.Clamp](coremath.md)

---

Example using:

### `cylinderArcAngle`

In this example a 3D UI Container will continuously look at the player and bend as players get closer. For this example to work, the UI Container must have the **Is Screen Space** setting disabled in the properties window.

```lua
-- Get the UI Container
local propContainer = script:GetCustomProperty("UIContainer"):WaitForObject()

-- Get the local player
local player = Game.GetLocalPlayer()

-- Force the UI Container to constantly look at the player
propContainer:LookAtContinuous(player)

-- The maximum distance at which the player's distance from the UI Container will affect
-- the curvature of the UI container
local MAX_DIST = 5000

function Tick(deltaTime)
    -- Determine the distance between the UI Container and the local player
    local distance = (propContainer:GetWorldPosition() - player:GetWorldPosition()).size

    -- Find the percent distance from MAX_DIST and clamp that value between 0 and 1
    local t = CoreMath.Clamp(distance/MAX_DIST)

    -- Calculate the arc angle of the UI container based on the distance from the local player to the
    -- UI Container
    local curvatureAngle = CoreMath.Lerp(180, 0, t)

    -- Update the arc angle of the UI Container
    propContainer.cylinderArcAngle = curvatureAngle
end
```

See also: [CoreObject.LookAtContinuous](coreobject.md) | [CoreMath.Lerp](coremath.md)

---

Example using:

### `opacity`

UI transitions provide a great way for you to show your creativity to players. This example will show you how to create a simple fade in. This script will cause the UI Container and all of the children of the UI Container to fade into view over 4 seconds by using the `opacity` property of the UI Container.

```lua
-- "timePassed" will keep track of the number of seconds that have passed since
-- the script began running
local timePassed = 0

-- Get the UIContainer object
local propUIContainer = script:GetCustomProperty("UIContainer"):WaitForObject()

function Tick(deltaTime)
    -- Update the "timePassed" to keep track of the number of seconds that have passed
    timePassed = timePassed + deltaTime

    -- Pick a value between 0 and 1 based on a percent (timepassed * 0.25)
    -- If the expression (timepassed * 0.25) is less than or equal to 0, the "Lerp" function will output 0
    -- If the expression (timepassed * 0.25) is greater than or equal to 1, the "Lerp" function will output 0
    -- If the expression (timepassed * 0.25) is in between 0 and 1, the "Lerp" function will output a value between 0 and 1
    local newOpacity = CoreMath.Lerp(0, 1, timePassed * 0.25)

    -- Update the opacity of the UIContainer and all of its children
    propUIContainer.opacity = newOpacity
end
```

See also: [CoreMath.Lerp](coremath.md) | [CoreObject.GetCustomProperty](coreobject.md)

---

Example using:

### `opacity`

This example will fade a UI Container in and out using events.

```lua
-- Get the UI Container
local propUIContainer = script:GetCustomProperty("UIContainer"):WaitForObject()

-- Time required to fade in or fade out
local FADE_TIME = 2

-- The calculated opacity of the UI Container
local opacity = 1

-- Determines whether the UI Container will be fading in or out
-- A value of 0 means that there will be no change in opacity
-- A value of 1 means that the UI Container will fade in
-- A value of -1 means that the UI Container will fade out
local fadeDirection = 0

-- This function will prepare the UI Container to be faded in
function FadeIn()
    opacity = 0
    fadeDirection = 1
end
-- Bind the "FadeIn" function to the "Fade In UI" event
Events.Connect("Fade In UI", FadeIn)

-- This function will prepare the UI Container to be faded out
function FadeOut()
    opacity = 1
    fadeDirection = -1
end
-- Bind the "FadeOut" function to the "Fade Out UI" event
Events.Connect("Fade Out UI", FadeOut)

function Tick(deltaTime)
    -- If the "fadeDirection" is 0, we can immediately exit this function because no changes
    -- will be made to the opacity of the UI Container.
    if(fadeDirection == 0) then
        return
    end
    -- Calculate the current opacity based on the "fadeDirection" and "deltaTime"
    opacity = opacity + (fadeDirection * deltaTime/FADE_TIME)

    -- Clamp the calculate opacity between 0 and 1
    local clampedOpacity = CoreMath.Clamp(opacity, 0, 1)

    -- If the "clampedOpacity" is different from the calculated "opacity" the must
    -- fading animation must finished meaning "fadeDirection" can return to 0
    if(opacity ~= clampedOpacity) then
        fadeDirection = 0
    end

    -- Update the opacity of the UI Container
    propUIContainer.opacity = clampedOpacity
end

-- Fade the UI in and then fade the UI out
Task.Spawn(function ()
    Events.Broadcast("Fade In UI")
    Task.Wait(FADE_TIME)
    Events.Broadcast("Fade Out UI")
end)
```

See also: [Events.Broadcast](events.md) | [CoreMath.Clamp](coremath.md)

---

Example using:

### `useSafeArea`

Some devices (such as mobile phones) may have regions at the edges of their screen where interactive elements should not be placed. While this can be handled automatically by the `UI Container` object, some advanced projects may need to incorporate these limits into their logic. In this example we figure out if the container cares about using the safe area and, if so, where is the "unsafe" area and how wide is it. This can be simulated in preview mode with options provided in the editor (next to play button).

```lua
local UI_CONTAINER = script.parent
local notchWidth = nil

if UI_CONTAINER.useSafeArea then
    local safeArea = UI.GetSafeArea()
    local screenSize = UI.GetScreenSize()
    
    if safeArea.left > 0 then
        notchWidth = safeArea.left
        print("UI Container is using safe zone.")
        print("There is a notch of " .. notchWidth .. " on the LEFT side.")
    end
    if safeArea.right < screenSize.x then
        notchWidth = screenSize.x - safeArea.right
        print("UI is using safe zone.")
        print("There is a notch of " .. notchWidth .. " on the RIGHT side.")
    end
end
if not notchWidth then
    print("The whole screen is safe for UI =)")
end
```

See also: [UI.GetSafeArea](ui.md) | [Rectangle.left](rectangle.md)

---

## Tutorials

[UI in Core](../references/ui.md)
