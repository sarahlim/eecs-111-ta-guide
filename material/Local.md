# `local` expressions and scoping

```
(local [definition ...] expression)
```

`local` allows one or more `definitions` to be defined for usage within an `expression`.

Within the `expression` body of a `local`, there are two things to note:

1. `expression` has full access to variables or functions defined in outer scopes (think of scoping as a one-way trickle-down, where inner scopes can access identifiers bound in outer scopes, but not vice versa).
2. This is arguably the only exception to the above, but when there is a naming conflict between a variable or function defined at the upper level and one defined in `[definitions ...]`, the `local`ly defined name takes precedence (this is called *shadowing*).

## Usage

```scheme
(define my-variable 10)
(define (global-function n)
  (* n 2))

(define (another-function x)
  (local [; Note redefinition of `my-variable` from outer scope!
          (define my-variable 200)]
    ; This expression shadows the locally defined `my-variable`
    (+ my-variable
       ; Uses `global-function` defined at the global scope
       (global-function x))))

> (another-function 3)
206  ; 200 + (3 * 2)

; Outside the local scope, `my-variable` takes on the globally-defined
; value once again.
> (+ my-variable 5)
15  ; would be 205 using shadowed value of `my-variable`
```

## Teaching Notes

It may be helpful to articulate variable shadowing in terms of "overwriting" globally-bound names within the body of the `local` expression. However, if you do use this analogy, take care to emphasize that the "overwriting" *only* occurs within the body of the `local`; expressions outside the scope created by `local` will not be able to reference locally-bound values.

```scheme
(define (yet-another-function x)
  (local [(define y 10)]
    (+ x y)))

> (yet-another-function 5)
15

> y
y: this variable is not defined
```

In EECS 111, `local` is most frequently used to provide a wrapper around functions with accumulators.

Suppose we have the following implementation of a function to sum up a list:

```scheme
(define (sum-with-accumulator lst partial-sum)
  (cond
    [(empty? lst) partial-sum]
    [else (sum-with-accumulator
           (rest lst)
           (+ partial-sum (first lst)))]))
```

It wisely uses an accumulator pattern to remain space-efficient. However, the caveat of using accumulators is that we need to initalize them to some value (just like with `foldl` and `foldr`). So in practice, using `sum-with-accumulator` would look like this:

```scheme
(sum-with-accumulator '(1 2 3) 0)
```

This is pretty suboptimal for a couple reasons:

1. It's not user-friendly to have to keep adding the `0` every time. Since the `0` should never change (i.e. the function is designed to work with an initial accumulator of `0`), it can easily be hard-coded.
2. The entire concept of an accumulator is a question of implementation, which should be abstracted from the function API. The user doesn't need to know how the `sum` operation is constructed. Moreover, exposing the initial accumulator to the user only introduces the possibility of something going wrong (what if someone decides to use a different value as the initial accumulator?).

So we refactor, using a `local` expression to encapsulate the recursive function and initial accumulator from the user:

```scheme
(define (my-sum lst)
  (local [(define (sum-with-accumulator lst partial-sum)
            (cond
              [(empty? lst) partial-sum]
              [else (sum-with-accumulator
                     (rest lst)
                     (+ partial-sum (first lst)))]))]
    (sum-with-accumulator lst 0)))
```

Now we can invoke our function `my-sum` like so:

```scheme
(my-sum '(1 2 3))  ; avoids the need to specify 0
```
