## Notes: Immutability

Elixir enforces immutability. Variables are bound to the same data until we rebind them to new values.

Elixir can reusue the values from previously bound data so it doesn't have create more data than it is required. This is possible because values are bound to names. Even if we change the value for a certain name the new variable will continue to reference the correct values. For example:

```elixir
iex> a = 1
1
iex> b = [a, 2]
[1, 2]
iex> a = 2
2
iex> b
[1, 2]
```

The performance impact of the garbage collection is minimised as each process has its own little garbage collection. Therefore, the heap is very small and garbage collection runs very fast.

Functions always return a new copy of the data structure. Because ***we always transform data***.