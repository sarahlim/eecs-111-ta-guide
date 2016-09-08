# `filter`

```
(filter pred lst) -> lst
```

`filter` takes a predicate `pred` and a list `lst`, and returns a new list consisting of elements in `lst` for which `pred` returned true.

> Predicates can take any number of arguments of any type. In the case of `filter`, however, we are dealing specifically with predicates that take one argument.

## Usage

```scheme
; Get all the even numbers
> (filter even? (list 1 2 3 4 5))
'(2 4)

; Also works with lambdas
; Get all the posns with X-values greater than 10
> (filter (lambda (pos) (> (posn-x pos) 10))
          (list (make-posn 1 2)
                (make-posn 13 3)
                (make-posn 5 20)))
(list (make-posn 13 3))
```

## Student Language Implementation

```scheme
(define (my-filter pred lst)
  (cond
    [(empty? lst) empty]
    
    ; Check whether pred is truthy for the next list item
    ; cons if so
    [(pred (first lst)) (cons (first lst)
                              (my-filter pred (rest lst)))]

    ; If pred didn't return truthy, skip adding this item
    [else (my-filter pred (rest lst))]))
```

## Teaching Notes

A number of students have trouble remembering which "direction" `filter` goes in (that is, they assume that `filter` "filters out" list elements for which `pred` returns true).

Generally this becomes a non-issue as students gain more exposure over the quarter, but it's a very common point of confusion during early weeks.
