
# PoB Lua Coding Style Guide

---
>
>**RFC/DRAFT ONLY**
>---
---

This guide is for **Path Of Building** Lua development.  It is based on the "Lua Style Guide" from the LuaRocks project.


## Indentation and Formatting

* Indent style: `TAB`.
* Line endings: `LF`.


## Strings

* `"double quotes"`, with `'single quotes'` for strings containing `'"double" "quotes"'`.


## Maximum Line length

* Try to avoid lines over 200 characters in length, but otherwise there's no hard rule.


## Naming

* **Classes**, **Methods**, and **Functions**: `UpperCamelCase`.  Acronyms (e.g. XML) are fully capitalised (`ImportCodeXML`).
* **Variables**: `lowerCamelCase`.  Acronyms (e.g. XML) obey `lowerCamelCase` (`xmlText`).
* **Booleans**: '`is`' prefix preferable (`isCodeValid`).
* **Constants**: `ALL_CAPS`.
* A single underscore (`_`) may be used for ignored variables (i.e. `for` loops).
* Avoid all-caps names starting with `_` as they are reserved by Lua (e.g. `_VERSION`).
* Avoid single-letter names (e.g. `i`), except for iterators, or very small scopes (less than ten lines or so).
* Avoid words that need an apostrophe (e.g. `dont`, `wont`, `cant`).
* Ideally, variables with larger scope should be given more descriptive names than those with smaller scope.

## Variable Declaration and Scope

* Always use `local` to declare variables.
* Assign variables using the smallest possible scope.
* Use the local versions of built-in globals.  They use `lower_snake_case` (e.g. `s_format` for `string.format`).

## Comments

* Use a single space after `--`.
```lua
-- good ✔️
--bad
```
* Put long comments on a separate line.

## Spacing

* Always put a space after commas, between operators, and between assignment signs.
```lua
-- good ✔️
local x = y * 9
local numbers = {1, 2, 3}
local strings = {
	"hello",
	"Lua",
	"world",
}
dog.Set("attr", {
	age = "1 year",
	breed = "Bernese Mountain Dog",
})

-- bad
local x=y*9
local numbers={1,2,3}
numbers={1 , 2 , 3}
numbers={1 ,2 ,3}
local strings = { "hello"
                , "Lua"
                , "world"
                }
dog.Set( "attr",{
	age="1 year",
 breed="Bernese Mountain Dog"
})
```

* Indent tables and functions according to the start of the line, not the construct:
```lua
-- good ✔️
local myTable = {
	"hello",
	"world",
}
UsingACallback(x, function(...)
	print("hello")
end)

-- bad
local myTable = {
                    "hello",
                    "world",
                 }
UsingACallback(x, function(...)
                       print("hello")
                    end)
```


* No spaces after the name of a function in a declaration or in its arguments:
```lua
-- good ✔️
local function Hello(name, language)
	-- code --
end

-- bad
local function Hello ( name, language )
	-- code --
end
```

* Add a blank line between functions:
```lua
-- good ✔️
local function Foo()
	-- code --
end

local function Bar()
	-- code --
end

-- bad
local function Foo()
	-- code --
end
local function Bar()
	-- code --
end
```

* Avoid aligning variable declarations:
```lua
-- good ✔️
local a = 1
local longIdentifier = 2

-- bad
local a              = 1
local longIdentifier = 2
```

* Alignment is occasionally useful when logical correspondence is to be highlighted:
```lua
-- okay ✔️
SysCommand(form, UI_FORM_UPDATE_NODE, "a",      FORM_NODE_HIDDEN,  false)
SysCommand(form, UI_FORM_UPDATE_NODE, "sample", FORM_NODE_VISIBLE, false)
```


## Tables

* When creating a table, prefer populating its fields all at once, if possible:
```lua
-- good ✔️
local player = {
	name = "Jack",
	class = "Rogue",
}
```
* Add a trailing comma to all fields including the last one.


## Functions

* Perform validation early and return as early as possible.

```lua
-- good ✔️
local function isGoodName(name, options, args)
	if #name < 3 or #name > 30 then
		return false
	end
	-- code --
	return true
end

-- bad
local function isGoodName(name, options, arg)
	local isGood = #name > 3
	isGood = isGood and #name < 30
	-- code --
	return isGood
end
```


## Blocks

* Only use single-line blocks for "`then return`", "`then break`" and "`function return`" (a.k.a "lambda") constructs:
```lua
-- good ✔️
if test then break end

-- good ✔️
if not test then return nil, "this failed for this reason: " .. reason end

-- good ✔️
UseCallback(x, function(k) return k.last end)

-- good ✔️
if test then
	return false
end

-- good ✔️
if test < 1 and DoComplicated(test) == false or seven == 8 and nine == 10 then
	DoOtherComplicated() 
	return false 
end

-- bad
if test < 1 and DoComplicated(test) == false or seven == 8 and nine == 10 then DoOtherComplicated() end
```
* Do not use semicolons as statement terminators, separate statements onto multiple lines.
```lua
-- good ✔️
a = 1
b = 2

-- bad
a = 1; b = 2
```


## Conditional expressions

* `false` and `nil` are falsy in conditional expressions. Use shortcuts when you can, unless you need to know the difference between `false` and `nil`.
```lua
-- good ✔️
if name then
	-- code --
end

-- bad
if name ~= nil then
	-- code --
end
```
* Avoid designing APIs which depend on the difference between `nil` and `false`.

* Use the `and`/`or` idiom for the pseudo-ternary operator when it results in more straightforward code. When nesting expressions, use parentheses to make it easier to scan visually:
```lua
-- good ✔️
local function DefaultName(name)
	-- return the default "Waldo" if name is nil
	return name or "Waldo"
end

-- good ✔️
local function BrewCoffee(machine)
	return (machine and machine.isLoaded) and "coffee brewing" or "fill your water"
end
```
Note that using "`x and y or z`" as a substitute for "`x ? y : z`" does not work if `y` may be `nil` or `false` so avoid it altogether for returning booleans or values which may be `nil`.


## Typing

* Use standard functions for type conversion, avoid coercion:
```lua
-- good ✔️
local totalScore = tostring(reviewScore)

-- bad
local totalScore = reviewScore .. ""
```
