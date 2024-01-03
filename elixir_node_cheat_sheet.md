# Elixir cheat sheet for Node developers

## [Hello World](#hello-world)
```elixir
# hello.exs
defmodule Greeter do
  def greet(name) do
    message = "Hello, " <> name <> "!"
    IO.puts message
  end
end

Greeter.greet("world")
```
```javascript
// hello.js
console.log("Hello, world!");
```
## [Variables](#variables)
```elixir
x = 1
```
```javascript
let x = 1;
```
*Nota bene*: Elixir variables are immutable. Once assigned, they cannot be changed only reassigned.

*N.B.2*: the elixir `=` operator is used for assignment, but it is the match operator. [Check pattern matching docs for a deep dive](https://hexdocs.pm/elixir/1.16/pattern-matching.html#the-match-operator) or [take a quick look you sexy beast](#pattern-matching).
## [Types](#types)
### [Primitive Types](#basic-types)
```elixir
1          # integer
0x1F       # integer -> no JS equivalent
1.0        # float
true       # boolean
:atom      # atom / symbol -> no JS equivalent
"elixir"   # string
[1, 2, 3]  # list
{1, 2, 3}  # tuple
%{a: 1}    # map -> Object
%Person{name: "John Doe", age: 30}  # struct -> Interface
def function() do ... end  # function
```
### [Strings](#strings)
```elixir
"elixir"  # "elixir"
"elixir" <> " " <> "rocks"  # "elixir rocks"
~s(elixir \x26 #{"inter" <> "polation"}) # "elixir & interpolation"
~S(elixir \x26 #{"inter" <> "polation"}) # "elixir \\x26 #{"inter" <> "polation"}"
```
```javascript
"JavaScript"  // string
"JavaScript" + " " + "rocks"  // "JavaScript rocks"
`JavaScript ${"inter" + "polation"}`  // "JavaScript interpolation"
```
### [Maps](#maps)
```elixir
address = %{city: "London", country: "UK"}
address[:city]  # "London"
address.country  # "UK"
```
```javascript
let address = {city: "London", country: "UK"};
address.city  // "London"
address["country"]  // "UK"
```
### [Lists](#lists)
```elixir
[1, 2, 3]  # list
[1 | [2 | [3 | []]]]  # list
[1, 2, 3] ++ [4, 5, 6]  # [1, 2, 3, 4, 5, 6]
[1, 2, 3] -- [2]  # [1, 3]
[1, 2, 3] -- [2, 3]  # [1]
[1, 2, 3] -- [2, 3, 4]  # [1]
```
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
```elixir
[head | tail] = [1, 2, 3]
head  # 1
tail  # [2, 3]
[first_element | _] = ['banana', 'apple', 'orange']
first_element  # 'banana'
[_first | rest] = ['brick', 'mortar', 'stone'] # _first is unused variable
rest  # ['mortar', 'stone']
```
```javascript
let [head, ...tail] = [1, 2, 3];
head  // 1
tail  // [2, 3]
```
*Nota bene*: [head | tail] requires a variable name for 'head' and 'tail'. In elixir '_' is a valid variable name and it is used to signify ignored/unused values. It is either the name of a variable `[first_element | _]` or a prefix for a variable name `[_first | rest]`.
### [Touples](#touples)
```elixir
{1, 2, 3}  # tuple
{1, 2, 3} ++ {4, 5, 6}  # {1, 2, 3, 4, 5, 6}
{1, 2, 3} -- {2}  # {1, 3}
{1, 2, 3} -- {2, 3}  # {1}
{1, 2, 3} -- {2, 3, 4}  # {1}
```
```javascript
[1, 2, 3]  // tuple
[1, ...[2, ...[3, ...[]]]]  // tuple
[1, 2, 3].concat([4, 5, 6])  // [1, 2, 3, 4, 5, 6]
[1, 2, 3].filter(x => x !== 2)  // [1, 3]
[1, 2, 3].filter(x => x !== 2 && x !== 3)  // [1]
[1, 2, 3].filter(x => x !== 2 && x !== 3 && x !== 4)  // [1]
```
### [Functions](#functions)
```elixir
def greet(name) do # do/end syntax
  "Hello, " <> name <> "!"
end
greet("world")  # "Hello, world!"

def greet2(name), do: "Hello, " <> name <> "!"  # keyword syntax
greet2("world")  # "Hello, world!"
```
```javascript
function greet(name) {
  return "Hello, " + name + "!";
}
greet("world")  // "Hello, world!"
```
#### [Anonymous Functions](#anonymous-functions)
```elixir
fn (name) -> "Hello, " <> name <> "!" end # arrow syntax
fn (name), do: "Hello, " <> name <> "!"  # keyword syntax
fn (name) do
  "Hello, " <> name <> "!"
end
my_greet = fn (name) -> "Hello, " <> name <> "!" end # assign anonymous function to variable
my_greet.("world")  # "Hello, world!"
```
```javascript
let myGreet = (name => "Hello, " + name + "!")
myGreet("world")  // "Hello, world!"
```
### [Sigils](#sigils)
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
### [if/unless](#if)
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
Since in Elixir everything is a function if/unless can be used as a JS ternary operator.
```elixir
lunch = if hungry do
  "pizza"
else
  "salad"
end

dress = unless cold, do: "shorts", else: "pants"
```
```javascript
let lunch = hungry ? "pizza" : "salad";
let dress = !cold ? "shorts" : "pants";
```

### [case](#case)
```elixir
case {1, 2, 3} do
  {4, 5, 6} -> "nope"
  {1, 2, 3} -> "yep"
end
```
```javascript
switch ([1, 2, 3]) {
  case [4, 5, 6]:
    "nope";
  case [1, 2, 3]:
    "yep";
}
```

### [cond](#cond)
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
```javascript
if (2 + 2 === 5) {
  "This will never be true";
} else if (2 * 2 === 3) {
  "Nor this";
} else if (1 + 1 === 2) {
  "But this will";
}
```







## [Pattern Matching](#pattern-matching)
:scroll: Black magic alert :scroll:[^1]
[^1]: If you are an exorcist, we are not responsible for the possible flashing lights, hellfire, witchcraft, or any other supernatural phenomena that may occur while reading this section.

## [Modules](#modules)
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
```javascript
const Redux = require('redux');  // compiles a module
import Redux from 'redux';  // compiles, and you can use without the `Redux.` prefix // adds functions to the current scope
import { Bar, Baz } from Foo;
```
### [Structs](#structs)
```elixir
defmodule Person do
  defstruct name: "John Doe", age: 30  # defines a Person struct with default values
end