## Notes

Elixir doesn't use assignments. Instead, it uses bindings of values to names.
The value is bound to a variable name using the `=` operator.
```elixir
iex> x = 1
1
iex> x
1
```

Now, the variable x is bound to the value 1. Let's check that `x = 1` is not an assignment but a *pattern matching*
```elixir
iex> 1 = x
1
iex> 123 = x
** (MatchError) no match of right hand side value: 1
```

We can also bind multiple variables at once, for example with lists.

```elixir
iex> [a, b, c] = [1, 2, [7,6,4]]
[1, 2, 3]
iex> a
1
iex> b
2
iex> c
[7, 6, 4]
```

### Exercise: PatternMatching - 1
Which of the following will match?
```elixir
a = [1,2,3]
```
This is a simple match with a named variable and a list.

```elixir
a = 4
```

This will bound the value 4 to the variable a.

```elixir
4 = a
```

This still works, as we previously bound 4 to a.

```elixir
[a, b] = [1,2,3]
```
Here we don't have a match as the left side has 2 variables and the right side has 3 variables.

```elixir
a = [[1,2,3]]
```
This will match correctly and bound the value to of the list with a single element `[1,2,3]` to a.

```elixir
[a] = [[1,2,3]]
```

This will match and bound the value `[1,2,3]` to a. It is different from the previous example where a list of list is bound, but here only the internal list is bound. See:

```elixir
iex> a = [[1,2,3]]
[[1, 2, 3]]
iex> [z]= [[1,2,3]]
[[1, 2, 3]]
iex> z
[1, 2, 3]
iex> a
[[1, 2, 3]]
```

```elixir
[[a]] = [[1,2,3]]
```

This example fails because the left side tries to bind a single element of the internal list to a, there are three elements. but on the right side,
