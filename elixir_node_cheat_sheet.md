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
# hello.js
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
def greet(name) do
  "Hello, " <> name <> "!"
end
greet("world")  # "Hello, world!"
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
fn (name) do: "Hello, " <> name <> "!"  # keyword syntax
fn (name) do
  "Hello, " <> name <> "!"
end
my_greet = fn (name) -> "Hello, " <> name <> "!" end # assign
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
### [Pattern Matching](#pattern-matching)
:scroll: Black magic alert :scroll:[^1]
[^1]: If you are an exorcist, we are not responsible for the possible flashing lights, hellfire, witchcraft, or any other supernatural phenomena that may occur while reading this section.
```elixir