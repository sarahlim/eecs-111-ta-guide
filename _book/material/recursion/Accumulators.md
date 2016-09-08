# Accumulators

In general when students ask about this first you should talk about two different ways of designing a function:

1. Accumulating the result into an extra argument at each recursive call and the base case returns that accumulator
2. When the base case returns the, well, base case and each recursive call’s result gets put together after the call is made.

Simple examples of this are the two ways of writing `map` and `fold`.

NOW you may talk about the performance concerns and stacks.

Specifically, stepping through the two examples of `map` and `fold` will help show how much extra the computer has to keep track of.
