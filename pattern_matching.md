## Notes: Pattern Matching

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
iex> [a, 2, c] = [1, 2, [7,6,4]]
[1, 2, 3]
iex> a
1
iex> c
[7, 6, 4]
iex> [a, 3, c] = [1, 2, [7,6,4]]
** (MatchError) no match of right hand side value: [1, 2, [7, 6, 4]]
```

If we don't want to bind or ignore it we can use underscores.

```elixir
iex> [a, _, _] = [1, 2, 3]
[1, 2, 3]
iex> a
1
```

Variables are bound only once.

```elixir
iex> [x,x] = [1,1]
[1, 1]
iex> x
1
iex> [x,x] = [1,2]
** (MatchError) no match of right hand side value: [1, 2]
```

We can force Elixir to pattern match with an existing variable using the caret `^` symbol:

```elixir
iex> x = 1
1
iex> ^x = 1
1
iex> ^x = 2
** (MatchError) no match of right hand side value: 2
```

NOTE: Erlang doesn't allow to reassign the same name variable. 
For example in Erlang we can't do this:

```erlang
2> X = 1.
1
3> X = 2.
** exception error: no match of right hand side value 2
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

This will bind the value 4 to the variable a.

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
This will match correctly and bind the value of the list with a single element `[1,2,3]` to a.

```elixir
[a] = [[1,2,3]]
```

This will match and bind the value `[1,2,3]` to a. It is different from the previous example where a list of lists is bound, but here only the internal list is bound. See:

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


### Exercise: PatternMatching - 2
Which of the following will match?
```elixir
[a,b,a] = [1,2,3]
[a,b,a] = [1,1,2]
[a,b,a] = [1,2,1]
```

Only the last one will match. The first and the second will throw an error as the binding happens only one and the variable `a` is assuming values multiple times in the same pattern match.

### Exercize: PatternMatching - 3
The variable `a` is bound to the value 2. Which of the following match?
```elixir
iex(1)>[a,b,a] = [1,2,3]
iex(2)>[a,b,a] = [1,1,2]
iex(3)>a = 1
iex(4)>^a = 2
iex(5)>^a = 1
iex(6)>^a = 2 - a
```
Assuming every example running with the same precondition of having `a` bound to 2.

1. fails for binding once rule
2. fails for binding once rule
3. doesn't fail: binds the value of a to 2
4. doesn't fail: pattern match
5. fails: `2 = 1` fails to pattern match
6. fails: `2 = 2 - 2` fails to pattern match 
