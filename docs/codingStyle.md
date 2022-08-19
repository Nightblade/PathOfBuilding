
# PoBC Lua Coding Style Guide

---
>
>**DRAFT ONLY**
>---
---

This guide is specifically for **Path Of Building Community** development, originally based on the "Lua Style Guide" from the LuaRocks project.


## Indentation and Formatting

* Indent style: `tab` only.
* Line endings: `LF` (Unix).


## Strings

* Use `"double quotes"`.
* Use `'single quotes'` for strings containing double quotes.


## Maximum Line length

* No hard rule, but try to avoid lines over 200 characters.


## Naming

* Variables with larger scope should be given more descriptive names than those with smaller scope.
* Avoid one-letter names except for iterators or very small scopes (less than ten lines).
* **Classes**, **Methods**, and **Functions**: `UpperCamelCase`.  Acronyms (e.g. XML) are fully capitalised (`ImportCodeXML`).
* **Variables**: `lowerCamelCase`.  Acronyms (e.g. XML) obey `lowerCamelCase` (`xmlText`).
* **Booleans**: `is` prefix preferable (`isCodeValid`).
* **Constants**: `ALLCAPS`.
* `_` may be used for ignored variables (for loops).
* Uppercase names starting with `_`, are reserved by Lua (e.g. `_VERSION`).


## Variable Declaration and Scope

* Always use `local` to declare variables.
* Assign variables using the smallest possible scope.
* Many globals have local versions already defined and use `snake_case` (e.g. `s_format` for `string.format`).


## Spacing

* Use a space after `--`.
```lua
--bad
-- good ✔️
```
* Always put a space after commas and between operators and assignment signs.
```lua
-- bad
local x = y*9
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
```
* The concatenation operator does not need spaces.
```lua
-- okay ✔️
local message = "Hello, "..user.."! This is your day # "..day.." in our platform!"
```

* Indent tables and functions according to the start of the line, not the construct:
```lua
-- bad
local myTable = {
                    "hello",
                    "world",
                 }
UsingACallback(x, function(...)
                       print("hello")
                    end)

-- good ✔️
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
-- bad
local function Hello ( name, language )
	-- code
end

-- good ✔️
local function Hello(name, language)
	-- code
end
```

* Add blank lines between functions:
```lua
-- bad
local function Foo()
	-- code
end
local function Bar()
	-- code
end

-- good ✔️
local function Foo()
	-- code
end

local function Bar()
	-- code
end
```

* Avoid aligning variable declarations:
```lua
-- bad
local a              = 1
local longIdentifier = 2

-- good ✔️
local a = 1
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
local player = {
	name = "Jack",
	class = "Rogue",
}
```
* Add a trailing comma to all fields including the last one.


## Functions

* Perform validation early and return as early as possible.

```lua
-- bad
local function isGoodName(name, options, arg)
	local isGood = #name > 3
	isGood = isGood and #name < 30

	-- ...stuff...

	return isGood
end

-- good ✔️
local function isGoodName(name, options, args)
	if #name < 3 or #name > 30 then
		return false
	end

	-- ...stuff...

	return true
end
```


## Blocks

* Only use single-line blocks for `then return`, `then break` and `function return` (a.k.a "lambda") constructs:
```lua
-- good ✔️
if test then break end

-- good ✔️
if not ok then return nil, "this failed for this reason: "..reason end

-- good ✔️
UseCallback(x, function(k) return k.last end)

-- good ✔️
if test then
	return false
end

-- bad
if test < 1 and DoComplicated(test) == false or seven == 8 and nine == 10 then DoOtherComplicated() end

-- good ✔️
if test < 1 and DoComplicated(test) == false or seven == 8 and nine == 10 then
	DoOtherComplicated() 
	return false 
end
```
* Separate statements onto multiple lines -- do not use semicolons as statement terminators.
```lua
-- bad
local whatever = "sure";
a = 1; b = 2

-- good ✔️
local whatever = "sure"
a = 1
b = 2
```


## Conditional expressions

* `false` and `nil` are falsy in conditional expressions. Use shortcuts when you can, unless you need to know the difference between `false` and `nil`.
```lua
-- bad
if name ~= nil then
	-- ...stuff...
end

-- good ✔️
if name then
	-- ...stuff...
end
```
* Avoid designing APIs which depend on the difference between `nil` and `false`.

* Use the `and`/`or` idiom for the pseudo-ternary operator when it results in more straightforward code. When nesting expressions, use parentheses to make it easier to scan visually:
```lua
local function DefaultName(name)
	-- return the default "Waldo" if name is nil
	return name or "Waldo"
end

local function BrewCoffee(machine)
	return (machine and machine.isLoaded) and "coffee brewing" or "fill your water"
end
```
Note that using `x and y or z` as a substitute for `x ? y : z` does not work if `y` may be `nil` or `false` so avoid it altogether for returning booleans or values which may be `nil`.


## Typing

* Use the standard functions for type conversion, avoid relying on coercion:
```lua
-- bad
local totalScore = reviewScore..""

-- good ✔️
local totalScore = tostring(reviewScore)
```
