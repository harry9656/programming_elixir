## Notes: Modules and Named Functions

In Elixir we create *modules* that group together *named functions*:

```elixir
defmodule Calculator do
    def sum(a,b) do
        a + b
    end

    def multiply(a, b) do
        a * b
    end
end
```

To compile a module we can load it inside the IEx we can append the file name after the `iex` command. Alternatively, from inside the iex terminal we can use the `c` command to compile the file: `c "calculator.ex"`.

Named functions are identified by their name and the *arity*, or number of, their arguments.

