## Notes: Elixir Basics

### Built-in Types
- Integers
  - It can be represented in different bases, for example, 0b1010, 0o777, 0x1F.
  - There is no limit to the size of integers.
- Floating-point numbers
  - We can write the floating number with a decimal point or an exponential notion with at least one digit before the decimal point.
  - The maximum exponent is 308 with 16 digits of accuracy. 
  - Floating numbers follow the standard IEEE 754.
- Atmos
  - Constants representing a name. Their value is their name.
  - We write the in lowercase with underscores between words.
  - All atoms start with the colon symbol,`:`.
  - They follow the UTF-8 standard.
- Ranges
  - They represent a sequence of integers.
  - We can create a range with the `..` operator—for example, `1..10`.
- Regular expressions
  - We can create regular expressions with the `~r{regexp}` sigil.
  - For example, `~r{pineapple}` is a regular expression that matches the word "pineapple".
  - The format with slashes is admissible: `~r/pineapple/`. But that requires escaping the slashes.
  - Use the Regex module to work with regular expressions.
- Truth
  - Represented by three atoms: `true`, `false`, and `nil`.
  - `nil` is equivalent to `false` in a Boolean context.

### System Types
- PID
  - Process identifier that references a local or remote process.
  - We can get the PID of the current process with the `self()` function.
- Port
  - Port is the reference to a resource outside the Erlang VM.
- Reference
  - Used to create a global reference. Rarely used.

### Collection Types
- Tuples
  - Order collection of values.
  - We create a tuple with curly braces and values separated by commas. For example, `{ :banana, :apple, :orange }` or more commonly `{ :ok, 200, "Success" }`.
  - We keep tuples small, let's say having less than four values. Alternatively, we use maps.
- Lists
  - Lists are a linked data structure. We write a list as `[1,2,3]`.
  - They may be empty or contain a *head* and *tail*.
  - Expensive to access in random order.
  - Operation of lists:
    - Concatenation: `[1,2,3] ++ [4,5]` yields `[1,2,3,4,5]`.
    - Difference: `[1,2,3] -- [2]` yields `[1,3]`.
    - Membership: `1 in [1,2,3]` yields `true`.
  - Keyword lists
    - A list of tuples where the first element is an atom and represents the key, and the second element represents its value. For example, `[{:name, "John"}, {:age, 27}]`.
    - We can write more concisely: `[name: "John", age: 27]`.
    - If the keyword list is the last argument, we can also remove the square brackets.
- Maps
  - A collection of key/value pairs.
  - It looks like this: `%{ :key => value, :key2 => value2 }`.
  - It is convenient to use the same type for the keys, but elixir allows different types.
  - Maps don't allow the repetition of keys, as we could do with keyword lists.
  - Use them as associative arrays.
  - We can access the values using square brackets. For example, we can get the `value2`: `map[:key2]`. 
  - Similar to keyword lists, we can use the simplified notation when keys are atoms: `%{ key: value, key2: value2 }`.
  - If keys are atoms, we can use the dot notation: `map.key2`.
  - Throws `KeyError` when there is no match.
- Binaries
  - A sequence of bits and bytes.
  - Enclosed in `<<` and `>>`. For example. `<<1,2,3>>`.
  - We can add modifiers to control the size of each field. For example, `<<1::size(2), 2::size(4), 3::size(2)>>` creates a binary with 8 bits where the first field has 2 bits, the second 4 bits, and the last 2 bits.
  - Elixir uses them to represent UTF strings.
- Dates and Times
  - Use the `Calendar` module to manipulate dates. It follows the ISO-8601 Gregorian calendar convention.
  - Date type holds year, month and, and day: `Date.new(2024, 12, 25)`.
  - We can represent a range of Date: `Date.range(~D[2024-12-25], ~D[2025-01-01])`.
  - Time has the hours, minutes, seconds, and fractions of seconds: `Time.new(12, 30, 45)`.
  - There are two versions of date time type:
    - DateTime: includes the information on the time zone.
    - NaiveDateTime: represents only the data and the time.
  - We can use Elixir's sigils to create Date and time: `~D[2024-12-25]` and `~T[12:30:45]`.
 
### Names Conventions

Module, record, protocol, and behavior names start with an uppercase letter and use the BumpyCase convention. Everything else starts with a lowercase letter or an underscore and use_the_snake convention.

Eventually, identifiers can end with a question mark or exclamation mark to signal that they return a boolean or have a side effect, respectively.
 
### Variable Scope

Elixir is lexically scoped, which means that the scope of a variable depends on where it is defined in the code.

Elixir doesn't have the concept of blocks, but can group expressions in do blocks:

```elixir
if result = :ok do
  x = 1
  IO.puts x
end
```

The `with` expression allows to write variables with local scope. For example, open a file and read its content without leaking temporary variables outside:

```elixir
content = "outside with scope" 
file_content = with {:ok, file} <- File.open("example.txt") do 
  content = IO.read(file, :all) # Doesnt leak the contents
  content
end
IO.puts content   # Prints "outside with scope"
```

We can use pattern matching inside the `with` and return the value:

```elixir
file_content = with {:ok, file} <- File.open("example.txt"),
                    content <- IO.read(file, :all), do: content
```

`with` is a macro that calls a function. Therefore, we must put the first argument on the same level as the `with` expression. Alternatively, we can use parenthesis:

```elixir
file_content = with(
    {:ok, file} <- File.open("example.txt"),
        content <- IO.read(file, :all), do: content)
```