## Notes: Anonymous Functions

Functions are a basic type in Elixir. So, we can bind them to variables and later call them.

To create a function, we use the following format:

```elixir
foo = fn 
    (argument1, argument2) -> body
end
```

Then, we can call this function `foo.(arg1, arg2)`. The dot after the name tells us that it is an anonymous function. Parenthesis' are optional in Elixir when defining functions, but we need them when calling the function.

Functions return the last expression in the body. For example, the following function returns the sum of two numbers:

```elixir
sum = fn (a, b) -> a + b end
IO.puts sum.(1, 2) # 3
```

When calling functions, Elixir does pattern matching and not an assignment as we would have in most other languages. If the matching succeeds, then the variables are bound to their argument. 

A single function can have multiple bodies based on the type of the arguments. The number of arguments must be the same, though.
This is possible because Elixir uses pattern matching to select the clause to run. For example, when reading a file, we can act differently:

```elixir
handle_open = fn
    {:ok, file} -> IO.puts "File contents: #{IO.read(file, :all)}" 
    {_, error} -> IO.puts "Error: #{:file.format(error)}"
end
```
In case of success, we print the contents of the file. Alternatively, we are printing the error. When using the underscore character `_`, the pattern matching ignores that variable. 
We can use `#{...}` to format strings nicely. 

Functions can return other functions because they can be treated as data types. Therefore, we can create *closures*:

```elixir
greeter = fn (greeting) -> 
    fn (name) -> IO.puts "#{greeting}, #{name}!"
end

welcome = greeter.("Welcome")
welcome.("Harry") # Welcome, Harry!

ciao = greeter.("Ciao")
ciao.("Harry") # Ciao, Harry!
```

Elixir allows to pass pinned values as function parameters as well. 

For conciseness, we can use the `&` notation to to write functions:

```elixir
sum = &(&1 + &2)
```

The `&()` refers to a function whereas `&1` and `&2` refer to the arguments. So, this is equal to:

```elixir
sum = fn (a, b) -> a + b end
```

Moreover, Elixir can simplify the anonymous function when it simply wraps a named function:

```elixir
iex> print = &(IO.puts(&1))
&IO.puts/1
```
It just saves the reference of the `IO.puts/1` function. So we can also shorten the call:

```elixir
print = &IO.puts/1
```


## Exercises

### Functions 1
Create and run a function that do the following:
```elixir
list_concat.([:a, :b], [:c, :d]) # Outputs: [:a, :b, :c, :d]
sum.(1,2,3) #=> Outputs: 6
pair_tuple_to_list.({ 1234, 5678 }) #=> Outputs: [1234, 5678]
```
Solution
```elixir
list_concat = fn (a,b) -> a ++ b end
sum = fn (a,b,c) -> a+b+c end
pair_tuple_to_list = fn ({a,b}) -> [a,b] end
```

### Functions 2
Write a function that takes three arguments. If the first two are zero, return "FizzBuzz". If the first is zero, return "Fizz". If the second is zero, return "Buzz". Otherwise, return the third argument.
Do not use any language features that we haven't covered in this book.

Solution:
```elixir
fizz_buzz = fn 
    0 , 0, _ -> "FizzBuzz"
    0, _, _ ->  "Fizz"
    _, 0, _ -> "Buzz"
    _, _, a -> a
end
```

### Functions 3
The operator *rem(a, b)* returns the remainder after dividing *a* by *b*. Write a function that takes a single integer (*n*) and calls the function in the previous exercise, passing it *rem(n, 3)*, *rem(n, 5)*, and *n*. Call it seven times with the arguments 10, 11, 12, and so on. 

Expected output: "Buzz, 11, Fizz, 13, 14, FizzBuzz, 16"

Solution:

```elixir
fb = fn (n) -> fizz_buzz.(rem(n,3),rem(n,5), n) end

IO.puts fb.(10)
IO.puts fb.(11)
IO.puts fb.(12)
IO.puts fb.(13)
IO.puts fb.(14)
IO.puts fb.(15)
IO.puts fb.(16)
```

### Functions 4
Write a function prefix that takes a string. It should return a new function that takes a second string. When that is called, it will return a string containing the first string, a space, and the second string.

Solution:

```elixir
prefix = fn (prefix) ->
            fn (name) -> "#{prefix} #{name}"
            end
        end
```

### Functions 5

Use the `&` notation to rewrite the following:
- `Enum.map [1,2,3,4], fn x -> x + 2 end`
- `Enum.map [1,2,3,4], fn x -> IO.inspect x end`

Solution:

```elixir
Enum.map [1,2,3,4], &(&1+2)
Enum.map [1,2,3,4], &IO.inspect/1
```