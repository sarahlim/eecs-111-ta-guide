# `foldl` and `foldr`

```scheme
(foldl proc init lst ...+) -Any
(foldr proc init lst ...+) -Any
```

`foldl` and `foldr` both act as reducers on lists, using `proc` to "fold" each item of the list in turn into the initial value `init`.

The signature of `proc` is important. More specifically, the type signature of `foldl`/`foldr` with one `lst` argument is

```
(X Y -Y) Y [List-of-X] -Y
```

meaning that `proc` should take two arguments (if there is one `lst`) like so:

```
item-from-lst accumulated-value
```

## Usage

The canonical example of `foldl` usage is taking the sum of a list of numbers:

```scheme
(define numbers '(1 2 3 4 5 6))

; `proc` is the addition function, `+`
; `init` is 0
; `lst` is `numbers`
(foldl + 0 numbers)
21

; Using foldl to concatenate a list of strings
(define words '("madam" "im" "adam"))

(foldl string-append "" words)
"adamimmadam"
```

## Difference Between `foldl` and `foldr`

The most important difference between `foldl` and `foldr` is that `foldl` traverses the list from left-to-right, and `foldr` from right-to-left. Continuing the above example,

```scheme
(foldl string-append "" words)
"adamimmadam"

; foldr does things the other way around
(foldr string-append "" words)
"madamimadam"
```

How does this affect implementation? The difference is that `foldl` is tail-recursive, whereas `foldr` is not.

With `foldl` (and accumulator patterns generally), `proc` is applied to the current list value and the **accumulator**. That is, the recursive expression looks like this:

```scheme
(my-foldl proc
          (proc (first lst) init)  ; proc applied here
          (rest lst))
```

With `foldr` and non-optimized patterns, `proc` is applied to the current value and the **result of recursing on the rest of the list**. That is, evaluation cannot complete until the entire list has been traversed.

```scheme
(proc (first lst)     ; proc is all the way up here
      (my-foldr proc
                init  ; init is unmodified
                (rest lst)))
```

## Student Language Implementations

Student lang implementations of each function, demonstrating how the difference in order of application affects the structure and space efficiency of each function:

```scheme
; foldl is constant space because it uses an accumulator
(define (my-foldl proc init lst)
  (cond
    ; In the base case, return init
    [(empty? lst) init]
    [else
     ; Fold current list item into the accumulator
     ; and pass that to the next recursive call,
     ; as the new init
     (my-foldl proc
               (proc (first lst) init)  ; new init
               (rest lst))]))

(define (my-foldr proc init lst)
  (cond
    ; In the base case, return init
    [(empty? lst) init]
    [else
     ; Since we are starting from the end of the list
     ; or innermost pair, we will use parenthetical order
     ; of operations to make sure the inner items get folded
     ; first.
     (proc (first lst)
           (my-foldr proc
                     init
                     (rest lst)))]))
```

## Teaching Notes

These are typically the most confusing higher-order functions in EECS 111. Whereas previous examples such as `map` and `filter` preserve the shape of the input `lst`, `foldl` and `foldr` flatten them completely using a vaguely arcane mechanism, which breaks with students' mental models for higher-order functions.

When first introducing `foldl`/`foldr`, it can help to follow this order:

1. Demonstrate usage of the "fold" mechanic, using just `foldl` and an associative procedure (e.g. `+` and `*`)
2. Introduce the difference between the two functions at a high level using a non-associative procedure (e.g. `-`)
3. Show how implementation varies between the two functions (with `foldl` being constant-space)

It is important to introduce both `+` and `*` in Step 1, in order to demonstrate two different `init` values.

```scheme
(foldl + 0 '(1 2 3 4))  ; the additive identity is 0
(foldl * 1 '(1 2 3 4))  ; the multiplicative identity is 1
```

**Make sure to guide students through Step 2 carefully.** It is easy to choose arguments without thinking, which result in identical output from `foldl` and `foldr`.

Namely, even if the student uses a non-associative function such as `-`, passing a `lst` with an odd number of elements or duplicate elements can sometimes cause `foldl` and `foldr` to produce the same value:

```scheme
> (foldl - 0 '(1 2 3 4 5))
3
> (foldr - 0 '(1 2 3 4 5))
3
> (foldr - 0 '(1 2 3 2))
0
> (foldl - 0 '(1 2 3 2))
0
```

Take care to show `(foldl - ...)` examples with an odd number of unique `lst` elements.

Students also get extremely confused when tracing the execution of both functions in their student language implementations. Prepare for tutorials by tracing through a couple of examples until you are rock-solid on how they work.
