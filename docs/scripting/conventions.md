# Core Lua Style Guide

**Goal:** Unify Lua conventions for a consistent style

## File Structure

> Note: Implementation specifics are tbd, this just notes the desire for imports at the top of the file

* Start with any `require` calls, such as:

```lua
local myImport = require("assetID")
myImport.foo()
```

Current syntax:

```lua
local myImport = game:find_object_by_id("assetID").context
myImport.foo()
```

## Naming

* Spell out words fully! Abbreviations generally make code easier to write, but harder to read.
* Use `PascalCase` names for classes, enum-like objects, and functions (both static and member) - `Enum.ENTRY`, `MyClass`, `MyFunction`
  * e.g. `Colors.RED`, `Dog = {}`, `function SaveTheWorld()` --Note: OO syntax TBD
* Use `camelCase` names for local variables and properties - `myClassInstance`, `myProperty`
  * e.g. `local furColor = panda.furColor`
* Prefix private fields with an underscore, like `_camelCase`.
  * Lua does not have visibility rules, but using a character like an underscore helps make private access stand out.
* Use `LOUD_SNAKE_CASE` names for constants (including enum entries)
* Make class names singular, unless it is a static (helper) class or enum
  * e.g. `Person`, `Cat` vs `StringUtils`, `DogBreeds`
* Events follow the `Get{EventName}Event` syntax

Element | Styling
--- | ---
Classes | PascalCase
Functions | PascalCase
Enums | PascalCase
Properties | camelCase
Variables | camelCase
Constants | LOUD_SNAKE_CASE

Here is a tiny example with all the above:

```lua
-- Instantiate car object
local car = Car.New()
-- Set the car's color property
car.color = Colors.GREEN
-- Drive off into the sunset
car:Drive()
```

Here are some examples of code that conform to the above:

```lua
-- Spawn player 30 units higher than normal, and print out new position
function HandlePlayerJoined(player)
    player:SetPosition(Vector3.New(player:GetPosition().x, player:GetPosition().y + 30, player:GetPosition().z))
    Utils.Print("New Position: "..tostring(player:GetPosition()))
end

game:GetPlayerJoinedEvent():Connect(HandlePlayerJoined)
```

```lua
-- Handle picking up a coin
function HandleOverlap(trigger, object)
	if (object ~= nil and object:IsA("Player")) then
        object:AddResource("Manticoin", 1)
        trigger.parent:Destroy()
	end
end

script.parent:GetBeginOverlapEvent():Connect(HandleOverlap)
```

```lua
-- Cat helper script
local DEBUG_PRINT = false

local function IncreaseAge(currentAge)
    currentAge = currentAge + 1
    if DEBUG_PRINT then
        -- Note: `tostring` is native Lua so it doesn't follow Core's conventions
        print("Current age updated to" .. tostring(currentAge))
    end
    return currentAge
end

local function Main()
    local cat = Cat.New()

    for i = 1, 30 do
        local furColors = cat:GetColors()
        -- Note: table.contains is uncapitalized because Lua
        if table.contains(furColors, "grey") then
            currentAge = IncreaseAge(cat.age)
        end
        if DEBUG_PRINT then
            local meowText = cat:Meow()
            print(meowText)
        end
    end
end
```

## Dot vs Colon

The colon is only used for methods (member functions, for those of you with a C++ background). Everything else is a dot.

For more details, here is how it breaks down:

* Static (**dot**)
  * Functions
    * Constructor
  * Constants
* Instance
  * Methods (**colon**)
    * Events
  * Properties (**dot**)

For examples:

```lua
Color.New() -- static functions + constructors
Colors.RED -- constants
player.name -- properties
player:ApplyDamage() -- member functions
```

```lua
-- Casing + operations

uppercase -> Enum.ENUM_ENTRY
          -> Class.StaticFunction()
lowercase -> instance:MemberFunction()
          -> instance.property

-- You should be able to determine the type of operation by the casing alone

-- PascalCase|PascalCase
Class.StaticFunction()

-- camelCase|camelCase
instance.property

-- camelCase|PascalCase
instance:Method()
```

When making your own methods:

```lua
--[[ GOOD ]]

--Called as myClassInstance:Speak(extra)
function MyClass:Speak(extra)
    return self.speech .. extra
end

--[[ BAD ]]

--Called as MyClass.Speak(myClassInstance, extra)
function MyClass.Speak(self, extra)
    return self.speech .. extra
end
```

Both are perfectly valid, but following convention allows for the usage call to consistently use colons for clarity.

* Where possible, use getters and setters
  * Unless otherwise noted, mutating return types will affect the game object (pass by reference)
  * Properties use getter/setter methods unless you can both get _and_ set the value (in which case you can directly access it via the dot syntax)

Note that some implementations (e.g. with private fields) may result in a reference to self being used, so you use the dot syntax rather than colon for methods; this is fine (although passing in a dummy parameter to enable the use of `:` is ideal), really  just keep to the naming conventions.

## General

* Use tabs
* No whitespace at end of lines
* No vertical alignment

## Styling

* Use one statement per line (stay away from massive one-liners). Prefer to put function bodies on new lines.

```lua
--Good
table.sort(stuff, function(a, b)
    local sum = a + b
    return math.abs(sum) > 3
end)

--Bad
table.sort(stuff, function(a, b) local sum = a + b return math.abs(sum) > 3 end)
```

* Put a space before and after operators, except when clarifying precedence.

```lua
--Good
print(5 + 5 * 6^2)

--Bad
print(5+5* 6 ^2)
```

* Put a space after each comma in tables and function calls.

```lua
--Good
local familyNames = {"bill", "amy", "joel"}

--Bad
local familyNames = {"bill","amy" ,"joel"}
```

* When creating blocks, inline any opening syntax elements.

Good:

```lua
local foo = {
    bar = 2,
}

if foo then
    -- do something
end
```

Bad:

```lua
local foo =
{
    bar = 2,
}

if foo
then
    -- do something
end
```

* Don't put parenthesis around conditionals; they aren't necessary in Lua.

* Use double quotes for string literals (e.g. `local myMessage = "Here's a message"`)

## Comments

Use block comments for documenting larger elements:

* Use a block comment at the top of files to describe their purpose.
* Use a block comment before functions or objects to describe their intent.

```lua
--[[
    Pauses time so our protagonist has ample opportunity to train.

    Should only be used when there is a legitimate need to save the world,
    or the effectiveness will degenerate.
]]
local function saveTheWorld()
    ...
end
```

Use single line comments for inline notes.

Comments should generally focus on _why_ code is written a certain way instead of _what_ the code is doing.

* Obviously there are exceptions to this rule, the more obfuscated your execution the more true this is.

Good:

```lua
-- Without this condition, the player's state would mismatch
if playerIsAirborne() then
    enableFlying()
end
```

Bad:

```lua
-- Check if the player is in the air
if playerIsAirborne() then
    -- Set them to flying
    enableFlying()
end
```

Each line of a block comment starts with `--` and a single space (unless it is indented text inside the comment).

Inline comments should be separated by at least two spaces from the statement. They should start with `--` and a single space.

```lua
-- One space for block/single-line comments
local myNum = 2  -- Two spaces after the code, then one space for inline comments
```

---

# Quick Reference

## Casing

Element | Styling
--- | ---
Classes | PascalCase
Functions | PascalCase
Enums | PascalCase
Properties | camelCase
Variables | camelCase
Constants | LOUD_SNAKE_CASE

### Casing Calls

Casing | Example | Dot or Colon
--- | --- | ---
PascalCase -> UPPER_CASE | Enum.ENUM_ENTRY | Dot
PascalCase -> PascalCase | Class.StaticFunction() | Dot
camelCase -> camelCase | instance.property | Dot
camelCase -> PascalCase | instance:MemberFunction() | Colon

Note: Properties are used instead of getters/setters only when that element has a getter _and_ setter.

## Examples

Generic Example:

```lua
--[[
    Files start with a descriptive multi-line comment
]]--

-- Imports go next
local DogClass = require('DogClass')

-- Function are PascalCase
function GiveDog(player)

    -- Local variables use camelCase, classes use PascalCase
    local doggo = DogClass.New()

    -- Properties are camelCase, constants are UPPER_CASE
    doggo.color = Colors.BROWN

    -- Member functions are called with a ':' (while static functions, see above, are called with a '.')
    doggo:AttachToPlayer(player, PlayerSockets.RIGHT_ANKLE)
end

-- Event subscriptions are located at the end of the file
Game.onPlayerJoined:Connect(GiveDog)
```

Real example:

```lua
--[[
    When a player collides with a coin, give them the coin as a resource and remove the coin from the world
]]

-- Handle picking up a coin
function HandleOverlap(trigger, player)
    -- Check that the object colliding with the trigger is a player
    if (player ~= nil and player:IsA("Player")) then
        -- If so, increment the 'Manticoin' resource count for that player
        player:AddResource("Manticoin", 1)
        -- Destroy the object in the scene so nobody else can pick it up
        trigger.parent:Destroy()
    end
end

-- Whenever an object collides with the coin's trigger, run this function
trigger.onBeginOverlap:Connect(HandleOverlap)
```

---

* Note: Some content here is inspired from Roblox's Style Guide - all credit where it is due.
