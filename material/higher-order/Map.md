# `map`

```
(map proc, lst ...+) -> lst
```

In its most basic usage, `map` maps a `proc` over a `lst`.

## Usage 

Basic usage:

```scheme
(define doubler
    (lambda (n) (* 2 n)))

; Double all the numbers in a list
> (map doubler (list 1 2 3))
'(2 4 6)

; Also works with lambdas
> (map (lambda (n) (+ 1 n))
       (list 1 2 3))
'(2 3 4)
```

`map` also works with any number of `lsts`, not just one:

```scheme
; String, Number -> String
(define (birthday name age)
    (string-append "Happy "
                   (number->string age)
                   "th birthday, "
                   name))

(define names (list "Nikhil" "Shana" "Spencer"))
(define ages (list 20 30 40))

; Using map with multiple lists
> (map birthday names ages)
'("Happy 20th birthday, Nikhil"
  "Happy 30th birthday, Shana"
  "Happy 40th birthday, Spencer")
```

## Variadic `map`

When using `map` with multiple `lsts`, there are three requirements of the arguments passed:

1. **`proc` arity:** if there are $N$ `lsts` passed, then `proc` must take `N` arguments.
2. **`proc` argument type:** suppose there are 3 `lsts` passed in the following order: `List-of-Numbers, List-of-Strings, List-of-Lists-of-Strings`. Then the `proc` must take 3 arguments of type `Number, String, List-of-Strings` in that order.
3. **`lst` length:** all of the `lsts` passed must be of the same length.

These requirements essentially formalize the idea of mapping the function over the arguments of the `lsts`, one "row" at a time.

`map` will return a list of type `T`, where `T` is the return type of `proc`. That is, if `proc` is a function `Number -> String`, then `mapping` it over a `List-of-Numbers` will return a `List-of-Strings`.

## Student Language Implementation

```scheme
(define (my-map proc lst)
    (cond
        [(empty? lst) '()]
        [else (cons (proc (first lst))
                    (my-map proc (rest lst)))]))
```

## Teaching Notes

It can help to illustrate `map` on the board like so:

| `lst` | Output list |
|-------|-------------|
| `a`   | `(proc a)`  |
| `b`   | `(proc b)`  |
| ...   | ...         |

This visualization expands well to variadic use cases. That is, `(map proc lst1 lst2 lst3)` can be illustrated as:

| `lst1` | `lst2` | `lst3`  | Output list        |
|--------|--------|---------|--------------------|
| `a`    | `1`    | `true`  | `(proc a 1 true)`  |
| `b`    | `2`    | `true`  | `(proc b 2 true)`  |
| `c`    | `3`    | `false` | `(proc c 3 false)` |

