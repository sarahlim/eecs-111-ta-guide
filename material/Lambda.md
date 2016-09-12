# Anonymous Functions and `lambda`

Anonymous functions, also known as lambdas, are functions that have no name, that is they are bound to no identifier.  Functions are a type of data, just like numbers, booleans, etc. They can be passed as arguments just like anything else.  They can be assigned to an identifier just like anything else.  This can be *really confusing* to people.  It may take some time to get to understanding.  To start, this point should be stressed:

```scheme
#the syntax
(define (foo x) ...)

#is just shorthand for
(define foo (λ (x) ...))

```

The point being anonymous functions are not special.  They've been using functions the whole time. `+`, `-`, `sqrt` etc. can all go in the same place as lambdas.  Lambda is probably best explained by use cases:

```scheme
#argument reduction (currying)
(define (add-n-to-list n lst)
  (map (λ (lst-element) (+ n lst-element)
       lst)))

(add-n-to-list 3 (list 1 2 3))
> '(4 5 6)

(define (get-fraction-func divisor)
  (λ (x) (/ x divisor)))

(define halve (get-fraction-func 2))
(halve 6)
> 3

(define quarter (get-fraction func 4))
(quarter 6)
> 1.5

# closures (access to variables after scope is closed)
(define (get-comp-func threshold)
  (λ (x) (> x threshold)))

(define positive (get-comp-func 0))
(positive -1)
> #f

# creates a function that compares the argument to some number within the given range
(define (get-secret-number-func lower-bound length)
  (λ (x) (= x (+ (random length) lower-bound))))

```

People tend to get confused with the arguments to lambdas.  The syntax of them, or what will be going in them, or even sometimes they forget that the lambda should have a spot for arguments!  I emphasize the parts of lambda: λ, the arguments, then the body.  3 parts.  Try to hit on that lambda expressions produce actions, not numbers/symbols/etc. Remember that anonymous functions are difficult and are often used in conjunction with other difficult (and related) concepts: higher order functions and recursion.  Be patient, and steadfast in the core concepts: functions are a data type, and anonymous functions are doing the same thing as the other functions we've used and written!