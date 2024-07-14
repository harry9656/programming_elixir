## Notes: Modules and Named Functions

In Elixir we create *modules* that group together *named functions*:

```elixir
defmodule Times do
    def double(n) do
        n * 2
    end
end
```

To compile a module we can load it inside the IEx we can append the file name after the `iex` command. Alternatively, from inside the *iex terminal*, we can use the `c` command to compile the file: `c "times.ex"`.

Named functions are identified by their name and the *arity*, or number of, their arguments.
**Don't use the same name for two functions that do unrelated things**.

*do..end* blocks group expressions that are used in module and named functions, control structures and places where code needs to be handled as an entity. The actual syntax is something like this:

```elixir
def double(n), do: n * 2
```

While the *do..end* is just syntactic sugar for:

```elixir
defmodule Times, do: (def double(n), do: n*2)
```

Which is not very readable.

### Exercise 1: ModulesAndFunctions-1

Extend the `Times` module with a `triple` function that multiplies its parameter by three.

```elixir
defmodule Times do
    def double(n), do: n*2
    def tripple(n), do: n*3
end
```

### Exercise 2: ModulesAndFunctions-2

Run the result in IEx. Use both techniques to compile the file.
> The first one is to use `iex <times.exs>`
> The second is to use the `c times.exs` from inside the IEx terminal.

### Exercise 3: ModulesAndFunctions-3

Add a `quadruple` function. (Maybe it could call the `double` function...)

```elixir
defmodule Times do
    def double(n), do: n*2
    def tripple(n), do: n*2
    def quadruple(n), do: double(double(n))
end
```

### Exercise 4: ModulesAndFunctions-4

Implement and run a function `sum(n)` that uses recursion to calculate the sum of the integers from 1 to n. You'll need to write this function inside a module in a separate file. Then load up IEx, compile that file, and try your function.

```elixir
defmodule Calculator do
    def sum(0), do: 0
    def sum(n), do: n+sum(n-1)
end
```

### Exercise 5: ModulesAndFunctions-5

Write a function `gcd(x,y)` that finds the greatest common divisor between two nonnegative integers. Algebraically, `gcd(x,y)` is x if y is zero; it's gcd(y, rem(x,y)) otherwise.

```elixir
defmodule Calculator do
    def gcd(x, 0), do: x
    def gcd(x, y), do: gcd(y, rem(x, y))
end
```

## Guard Clauses

In short, guards are predicates attached to a function definition using one or more `when` keywords. After pattern matching, Elixir evaluates these predicates. If they return `true` then run the function:

```elixir
defmodule Guard do
    def what_is(x) when is_number(x) do
        IO.puts "#{x} is a number"
    end
    def what_is(x) when is_list(x) do
        IO.puts "#{x} is a list"
    end
    def what_is(x) when is_atom(x) do
        IO.puts "#{x} is an atom"
    end
end
```

Guards can only be used with:

- Comparison operators
- Boolean and negation operators
- Arithmetic operators
- Join operators
- The `in` operator
- Type-check functions
- Other functions

## Default parameters

We can give a default value to any parameter of a function using the syntax `param \\ default_value`.

To avoid any confusion we can add a function head with no body that contains the default parameters:

```elixir
defmodule Params do
    def func(p1, p2 \\ 123)

    def func(p1, p2) when is_list(p1) do
        "You said #{p2} with a list"
    end
    def func(p1, p2) do
        "You passed in #{p1} and #{p2}"
    end
end
```

### Exercise: ModulesAndFunctions-6
I'm thinking of a number between 1 and 1000...

...

Your API will be `guess(actual, range)`, where `range` is an Elixir range. 

```elixir
defmodule Chop do
    def guess(actual, range) do
        (l..h) = (range)
        mid = l + div(h-l, 2)
        IO.puts "Is it #{mid}"
        match(actual, range, mid)
    end

    def match(actual, _range, guess) when actual == guess do
        IO.puts "#{guess}"
    end

    def match(actual, range, guess) when actual > guess do
        (l..h) = range
        mid = l + div(h-l, 2)
        guess(actual, mid..h)
    end

    def match(actual, range, guess) when actual < guess do
        (l..h) = range
        mid = l + div(h-l, 2)
        guess(actual, l..mid)
    end
end
```

## Private functions

The `defp` macro defines a private function that can only be called within the module that declares it.
You cannot have some heads private and others public:

```elixir
def fun(a) when is_list(a), do: true
defp fun(a), do: false
```

Is not valid.

## The Pipe Operator :>

The `|>` operator takes the result of the expression to its left and inserts it as the first parameter of the function invocation to its right.

So, `val |> f(a,b)` is the same as calling `f(val, a, b)`.

## Modules

Modules provide namespaces for things you define. We can define one module inside the other one, but all modules are available at the top level.

For nested modules, Elixir prepends the outer module name to the inner module name separated by a dot between the two names. For example, `Mix.Tasks.Doctest`.

### Directives in Elixir

The `import` directive: allows to bring a module's functions and/or macros into the current scope. It can help cut down the clutter.
The full syntax is `import Module [, only:|expect: ]`.

The `alias` directive creates an alias for a module. Usually, it reduces the typing. For example, `alias My.Other.Module.Parser, as: Parser`.
We could remove the `as:` because it defaults to the last part of the module name. Also, we can group multiple aliases into one command:
`alias My.Other.Module.{Parser, Runner}`.

The `require` module is used when we want to use any macros it defines.

## Module Attributes

Each Elixir module has metadata. Each item of the metadata is called an *attribute* of the module and we can access it using the `@` sign followed by the name of the attribute:

```elixir
defmodule Example do
    @author "Dave Thomas"
    def get_author do
        @author
    end
end
```

We can set the attribute multiple times and the value is the same as we expect at the time of the function definition:

```elixir
defmodule Example do
    @attr "one"
    def first, do: @attr    # => returns two
    @attr "two"
    def two, do: @attr      # => returns two
end
```

**Use attributes for configuration and metadata only.**

## Module names

A call to a function is a module is really an atom followed by a dot followed by the function name:

```elixir
iex(1)> IO.puts 123
123
:ok
iex(2)> :"Elixir.IO".puts 123
123
:ok
```

## Calling a function in an Erlan Library

Erlang uses different conventions for names. For example, variables start with an uppercase letter and atoms are simple lowercase names.
Erlang module `timer` is the atom `timer` and to use the function `tc` in `timer` in Elixir we write `:timer.tc`. **Note the semicolon at the start**.

## Finding libraries

The built-in libraries are documented on the official site. Others can be found on (hex.pm)[www.hex.pm].