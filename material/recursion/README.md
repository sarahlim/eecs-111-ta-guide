# Recursion

Here's a link to the [Racket documentation on Lists, Iteration, and Recursion](https://docs.racket-lang.org/guide/Lists__Iteration__and_Recursion.html). 

Recursion is one of the first big leaps in any type of programming. Students new to programming will have a near-infinite number of WTF moments, and students exclusively new to functional programming (AP CS) will usually have a near-infinite number of WTF WHY IS THIS NOT JAVA AHHHH RACKET SUCKS moments. It's okay, and these moments will inevitably happen.

When moments like these happen, it is useful to step through some of the code with students. 

For example, this recursive code for a function "remove-dups" takes a list of things and removes all of the things that already exist in the list.

```scheme
(define (remove-dups l)
    (cond
      [(empty? l) empty]
      [(empty? (rest l)) l]
      [else
        (let ([i (first l)])
            (if (equal? i (first (rest l)))
                (remove-dups (rest l))
                (cons i (remove-dups (rest l)))))]))
```

Here's what it looks like when run through that code using a list (list "a" "b" "b" "b" "b" "b"). Of course, it is also extremely useful to walk through each line of the code in "remove=dups" as it runs in addition to what happens after each call as well. 

```scheme
  (remove-dups (list "a" "b" "b" "b" "b" "b"))
= (cons "a" (remove-dups (list "b" "b" "b" "b" "b")))
= (cons "a" (remove-dups (list "b" "b" "b" "b")))
= (cons "a" (remove-dups (list "b" "b" "b")))
= (cons "a" (remove-dups (list "b" "b")))
= (cons "a" (remove-dups (list "b")))
= (cons "a" (list "b"))
= (list "a" "b")
```
