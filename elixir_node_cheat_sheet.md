# Elixir cheat sheet for Node developers

## [Variables](#variables)
**Elixir**
```elixir
x = 1
```
**JavaScript**
```javascript
let x = 1;
```
*Nota bene*: Elixir variables are immutable. Once assigned, they cannot be changed only reassigned.

**Elixir**
```elixir
x = 1
def foo do
  x = 2
end
x  # 1
```

*N.B.2*: the elixir `=` operator is used for assignment, but it is the match operator. Feeling like a CHAD? [Take a quick look at pattern matching](#pattern-matching) or [check the docs for a deep dive](https://hexdocs.pm/elixir/1.16/pattern-matching.html#the-match-operator).

### [Constants](#constants)
Elixir has no equivalent to `const x = 1;` JavaScript declaration, however module attributes function as constants. Module attributes are fixed in compile time thus you cannot create them dynamically. Inside a module you declare a module attribute with `@attribute_name value` syntax. You can access them with `@attribute_name` within the module. For demo purposes we define a function to provide read access to the module attribute. [More on module attributes for Queens and Kings.](#module-attributes)
**Elixir**
```elixir
defmodule Car do
  @wheels 4 # module attribute definition
  @initial_state %{wheels: @wheels, color: "red"}
  @has_windows true
  @brand "Paprika"
  def wheels, do: @wheels # function to provide read access to module attribute.
end
Car.wheels  # 4
```
## [Types](#types)
### [Primitive Types](#basic-types)
todo: table

**Elixir**
```elixir
1          # integer
0x1F       # integer -> no JS equivalent
1.0        # float
true       # boolean
:atom      # atom -> js symbols are similar, but atoms have no JS equivalent
"elixir"   # string
[1, 2, 3]  # list
{1, 2, 3}  # tuple
%{a: 1}    # map -> Object / Map
%Person{name: "John Doe", age: 30}  # struct -> Interface
def function() do ... end  # function
```
### [Atoms](#atoms)
**Elixir**
```elixir
```
#### [Elixir atoms VS JS symbols](#elixir-atoms-vs-js-symbols)
**Elixir**
```elixir
```
**JavaScript**
```javascript
```
### [Strings](#strings)
#### [Declaration](#string-declaration)
**Elixir**
```elixir
"elixir"  # "elixir"
~S(elixir \x26 #{"inter" <> "polation"}) # "elixir \\x26 #{"inter" <> "polation"}"
```
**JavaScript**
```javascript
"JavaScript"  // string
`JavaScript ${"inter" + "polation"}`  // "JavaScript interpolation"
```
#### [Interpolation](#interpolation)
**Elixir**
```elixir
"elixir" <> " " <> "rocks"  # "elixir rocks"
```
**JavaScript**
```javascript
"JavaScript" + " " + "rocks"  // "JavaScript rocks"
```
#### [Escaping](#escaping)
**Elixir**
```elixir
"we can escape \"quotes\""  # "we can escape \"quotes\""
"we can escape 'quotes'"  # "we can escape 'quotes'"
```
**JavaScript**
```javascript
'we can escape "quotes"'  // 'we can escape "quotes"'
"we can escape 'quotes'"  // "we can escape 'quotes'"
```
#### [String sigils](#string-sigils)
Sigils in general are a convenient and easy to read way to declare data types. The following examples use the `~s` and `~S` sigils to declare strings. The `~s` sigil is used to declare strings with escape codes, while the `~S` sigil is used to declare strings without escape codes. Sigil delimiters can be many special characters. Here we use `()`, `/`, `{}`, `<>`, `|`.

**Elixir**
```elixir
~s(elixir \x26 #{"inter" <> "polation"}) # "elixir & interpolation" () delimiter
~s/elixir \x26 #{"inter" <> "polation"}/ # "elixir & interpolation" / delimiter
~s{elixir \x26 #{"inter" <> "polation"}} # "elixir & interpolation" {} delimiter
~s<elixir \x26 #{"inter" <> "polation"}> # "elixir & interpolation" <> delimiter
~S|elixir \x26 #{"inter" <> "polation"}| # "elixir \\x26 #{"inter" <> "polation"}" ~S does not escape
```
### [Maps](#maps)
#### [Declaration](#map-declaration-and-basic-access)
**Elixir**
```elixir
address = %{"city" => "London", "country" => "UK"} # map with string keys
address = %{city: "London", country: "UK"} # map with atom keys and shorthand syntax
address = %{"city" => "London", country: "UK"} # map key-value declaration can be mixed, but the shorthand syntax must be on the end.
```
**JavaScript**
```javascript
let address = {city: "London", country: "UK"};
```
Elixir maps are similar to JavaScript maps, Objects, interfaces and classes. Elixir maps only hold key, value pairs, however **both** keys and values can be of any type. JavaScript maps can only hold string keys and values of any type. JavaScript objects can hold any type of key and value, however keys are coerced to strings. JavaScript interfaces and classes can hold any type of key and value, however keys are coerced to strings.

**Elixir**
```elixir
iex> a = %{1 => "b"}
%{1 => "b"}
iex> c = %{a => 2}
%{%{1 => "b"} => 2}
```
Above we create a map `a` with an integer key and a string value. Then we create a map `c` with a map `a` as a key and an integer value. Commonly map keys are [atoms](#atoms) or [strings](#strings).

Map values can be any kind as well. We can create nested data structures this way.

**Elixir**
```elixir
address = %{city: "London", country: "UK", neighbors: ["Jim", "Bob", "Alice"]}
```
#### [Reading values from a map](#reading-values-from-a-map)
**Elixir**
```elixir
army = %{infantry: 100, cavalry: 50}
army[:infantry]  # 100
army.infantry  # 100
```
**JavaScript**
```javascript
let army = {infantry: 100, cavalry: 50};
army.infantry  // 100
```
The [Map](https://hexdocs.pm/elixir/1.16/Map.html) module provides similar functionality compared to JavaScript maps and Objects. Some functions have 2 types like `Map.fetch/2` and `Map.fetch!/2`. The variant with the exclamatory mark `!` will raise an error if the function fails.

**Elixir**
```elixir
army = %{infantry: 100, cavalry: 50}
Map.get(army, :cavalry)  # 50
Map.get(army, :airforce)  # nil
Map.get(army, :airforce, 0)  # 0
Map.fetch(army, :cavalry)  # {:ok, 50}
Map.fetch(army, :airforce)  # :error
Map.fetch!(army, :airforce)  # ** (KeyError) key not found: :airforce -> this error is raised
```
**JavaScript**
```javascript
let army = {infantry: 100, cavalry: 50};
Object.get(army, "cavalry")  // 50
```
Another useful module is the [Kernel](https://hexdocs.pm/elixir/1.16/Kernel.html) module. It provides functions like `get_in/2` and `get_and_update_in/3` to access nested data structures.
#### [Updating values in a map](#updating-values-in-a-map)
**Elixir**
```elixir
tabletop_game = %{name: "Warhammer", players: 2}
%{tabletop_game | name: "Warhammer 40k"}  # %{name: "Warhammer 40k", players: 2}
```
**JavaScript**
```javascript
let tabletopGame = {name: "Warhammer", players: 2};
tabletopGame.name = "Warhammer 40k";  // {name: "Warhammer 40k", players: 2}
```
This elixir update syntax raises an error if no key was found

**Elixir**
```elixir
%{tabletop_game | name: "Warhammer 40k", edition: "9th"}  # ** (KeyError) key :edition not found in: %{name: "Warhammer", players: 2}
```
The Map module has other tools to update a map like `Map.put/3`.

**Elixir**
```elixir
Map.put(tabletop_game, :edition, "9th")  # %{name: "Warhammer 40k", players: 2, edition: "9th"}
Map.put(tabletop_game, :name, "Magic the Gathering")  # %{name: "Magic the Gathering", players: 2, edition: "9th"}
```
**JavaScript**
```javascript
let tabletopGame = {name: "Warhammer", players: 2};
tabletopGame.edition = "9th";  // {name: "Warhammer", players: 2, edition: "9th"}
```
#### [Deleting values from a map](#deleting-values-from-a-map)
**Elixir**
```elixir
paper_kite = %{paper_thickness: 0.1, paper_color: "white", paper_size: "A4", fold: "stealth"}

### [Lists](#lists)
**Elixir**
```elixir
[1, 2, 3]  # list
[1 | [2 | [3 | []]]]  # list
[1, 2, 3] ++ [4, 5, 6]  # [1, 2, 3, 4, 5, 6]
[1, 2, 3] -- [2]  # [1, 3]
[1, 2, 3] -- [2, 3]  # [1]
[1, 2, 3] -- [2, 3, 4]  # [1]
```
**JavaScript**
```javascript
[1, 2, 3]  // list
[1, ...[2, ...[3, ...[]]]]  // list
[1, 2, 3].concat([4, 5, 6])  // [1, 2, 3, 4, 5, 6]
[1, 2, 3].filter(x => x !== 2)  // [1, 3]
[1, 2, 3].filter(x => x !== 2 && x !== 3)  // [1]
[1, 2, 3].filter(x => x !== 2 && x !== 3 && x !== 4)  // [1]
```
*Nota bene*: Elixir stores lists and tuples very differently in memory. If you want performant code: [look it up honey](https://hexdocs.pm/elixir/1.16/lists-and-tuples.html).
#### [head and tail](#head-and-tail)
The `[head | tail]` syntax provides functionality similar to destructuring lists in JavaScript.
**Elixir**
```elixir
[head | tail] = [1, 2, 3]
head  # 1
tail  # [2, 3]
[first_element | _] = ['banana', 'apple', 'orange']
first_element  # 'banana'
[_first | rest] = ['brick', 'mortar', 'stone'] # _first is unused variable
rest  # ['mortar', 'stone']
```
**JavaScript**
```javascript
let [head, ...tail] = [1, 2, 3];
head  // 1
tail  // [2, 3]
```
*Nota bene*: [head | tail] requires a variable name for 'head' and 'tail'. In elixir '_' is a valid variable name and it is used to signify ignored/unused values. It is either the name of a variable `[first_element | _]` or a prefix for a variable name `[_first | rest]`.
### [Tuples](#tuples)
**Elixir**
```elixir
{1, 2, 3}  # tuple
{1, 2, 3} ++ {4, 5, 6}  # {1, 2, 3, 4, 5, 6}
{1, 2, 3} -- {2}  # {1, 3}
{1, 2, 3} -- {2, 3}  # {1}
{1, 2, 3} -- {2, 3, 4}  # {1}
```
*Nota bene*: Most common usage of tuples are return values. Similarly to lists there are several tool to manipulate them however, by far the most common usage is pattern matching tuples.
### [Functions](#functions)
In Elixir everything is a function. The most basic function is the anonymous function.
#### [Anonymous Functions](#anonymous-functions)
**Elixir**
```elixir
fn (name) -> "Hello, " <> name <> "!" end # arrow syntax
fn name -> 
  "Hello, " <> name <> "!"
end
my_greet = fn (name) -> "Hello, " <> name <> "!" end # assign anonymous function to variable
my_greet.("world")  # "Hello, world!"
```
**JavaScript**
```javascript
let myGreet = (name => "Hello, " + name + "!")
myGreet("world")  // "Hello, world!"
```
#### [Module functions](#module-functions)
**Elixir**
```elixir
defmodule Greeter do
  def greet(name) do # do/end syntax
    "Hello, " <> name <> "!"
  end

  def greet2(name), do: "Hello, " <> name <> "!"  # keyword syntax
end
Greeter.greet("world")  # "Hello, world!"
Greeter.greet2("world")  # "Hello, world!"
```
**JavaScript**
```javascript
function greet(name) {
  return "Hello, " + name + "!";
}
greet("world")  // "Hello, world!"
```
todo: private functions
#### [Function Signature](#function-signature)
#### [Function Overloading](#function-overloading)
todo: ! and ? functions
#### [Default Arguments](#default-arguments)
#### [Function guards](#function-guards)
#### [Pipe Operator](#pipe-operator)
#### [Function Capture](#function-capture)
### [Sigils](#sigils)
**Elixir**
```elixir
~r{regexp}  # regular expression
~r"hello"
iex> ~s(string with escape codes \x26 stuff)  # string with codes
"string with escape codes & stuff"
iex> ~S(string without escape codes \x26 stuff)  # string without codes
"string without escape codes \\x26 stuff"
~c{charlist}  # charlist
~w{words}  # word list
```
## [Control Flow](#control-flow)
### [cond](#cond)
**Elixir**
```elixir
cond do
  2 + 2 == 5 ->
    "This will never be true"
  2 * 2 == 3 ->
    "Nor this"
  1 + 1 == 2 ->
    "But this will"
end
```
**JavaScript**
```javascript
if (2 + 2 === 5) {
  "This will never be true";
} else if (2 * 2 === 3) {
  "Nor this";
} else if (1 + 1 === 2) {
  "But this will";
}
```
### [if/unless](#if)
**Elixir**
```elixir
if true do
  "true"
else
  "false"
end
unless true do
  "You may never see me"
end
```
**JavaScript**
```javascript
if (true) {
  "true";
} else {
  "false";
}
if (!true) {
  "You may never see me";
}
```
#### [Ternary Operator](#ternary-operator)
Since in Elixir everything is a function if/unless can be used as a JS ternary operator.
**Elixir**
```elixir
lunch = if hungry do
  "pizza"
else
  "salad"
end

dress = unless cold, do: "shorts", else: "pants"
```
**JavaScript**
```javascript
let lunch = hungry ? "pizza" : "salad";
let dress = !cold ? "shorts" : "pants";
```

### [case](#case)
**Elixir**
```elixir
case {1, 2, 3} do
  {4, 5, 6} -> "nope"
  {1, 2, 3} -> "yep"
end
```
**JavaScript**
```javascript
switch ([1, 2, 3]) {
  case [4, 5, 6]:
    "nope";
  case [1, 2, 3]:
    "yep";
}
```







## [Pattern Matching](#pattern-matching)
:scroll: Black magic alert :scroll:[^1]
[^1]: If you are an exorcist, we are not responsible for the possible flashing lights, hellfire, witchcraft, or any other supernatural phenomena that may occur while reading this section.

## [Modules](#modules)
**Elixir**
```elixir
require Redux   # compiles a module
import Redux    # compiles, and you can use without the `Redux.` prefix // adds functions to the current scope

use Redux       # compiles, and runs Redux.__using__/1
use Redux, async: true

import Redux, only: [duplicate: 2]
import Redux, only: :functions
import Redux, only: :macros

import Foo.{Bar, Baz}

defmodule Foo do    # defines a module
  def Bar, do: "Bar"
end
```
**JavaScript**
```javascript
const Redux = require('redux');  // compiles a module
import Redux from 'redux';  // compiles, and you can use without the `Redux.` prefix // adds functions to the current scope
import { Bar, Baz } from Foo;
```
### [Structs](#structs)
**Elixir**
```elixir
defmodule Person do
  defstruct name: "John Doe", age: 30  # defines a Person struct with default values
end
```
### [Module Attributes](#module-attributes)
Compile time not dynamic
**Elixir**
```elixir
```
**JavaScript**
```javascript
```