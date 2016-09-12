# Types

## Primitive Types

### Numbers : [https://docs.racket-lang.org/guide/numbers.html](documentation), [https://docs.racket-lang.org/reference/numbers.html](guide)

Racket numbers are either exact or inexact.  As explained by the guide I linked above, exact numbers are integers, ratios, or a complex number with exact components.  Inexact numbers are decimal numbers, that is, they are represented in floating point.  This also includes positive and negative infinity and NaN (not a number).

Exact numbers can be arbitrarily large and/or arbitrarily precise.  Inexact numbers are defaulted to 64-bit floating point (double precision), and only go to 32-bit if created that way (i.e. `1.0f`).

As far as teaching goes, though, you don't need to cover these things explicitly.  Number representation is taught in later classes.  In general, numbers behave as people expect, they add, multiply, divide, etc.  However, you will probably run into a floating-point precision issue at some point, and when you do, you should explain that the internal representation of decimals can't contain every possibility, since there are infinitely many, and so the system picked the closest number to the "true" value it *can* represent.  You should be familiar with some solutions to the problem, like `round`, `floor` and `ceiling`.  `round` is usually what you want if you're looking for an integer result.  Otherwise it's often best just to leave the number as is.  I suggest [http://floating-point-gui.de/formats/fp/](reading up) on floating point representation if you don't feel comfortable with it already.

As an aside, always use the `=` function when working with numbers, and advise your students to do the same.  Never use `eq?` or `equal?` or `eqv?` for numbers, because you will get some strange, unexpected results.  [https://docs.racket-lang.org/reference/booleans.html](Read up) on the differences if you want to understand why the other functions don't quite do what you'd think. 

### Booleans

Booleans are true or false.  Simple as that.  They are used for any kind of condition, like `if`, `cond`, `when`, `unless`, etc.  If you want a literal boolean, you can use several representations: `#t` and `#f`, `#T` and `#F`, `true` or `false`.  `#t` and `#f` are the preferred literal boolean representations.

To demonstrate booleans, I suggest using examples with numbers, such as
```scheme
(< 0 -1)
> #t

(< 5 8)
> #f

(= 2 5)
> #f

(= 6 6.0)
> #t

(if #t 1 0)
> 1

(if (<= 5 4) 1 0)
> 0
```

For the most part, students understand booleans readily, but one small thing I notice often is
```scheme
(if a-bool #t #f)
```
which, as I hope you notice, can simply be replaced with `a-bool`.  That is, students don't recognize that functions with a return value of boolean and arguments that are booleans are, in fact, booleans in all possible use cases.  This is partly not understanding arguments and return values in general, but I see this with booleans more frequently than others.  I suggest just pointing out the unnecessary code when you see it and explaining then.

Note: (from the Racket documentation) In the result of a test expression for if, cond, and, or, etc., however, any value other than #f counts as true.

### Characters

Characters in Racket are represented in Unicode, which is probably the most commonly used character encoding.  If you're unfamiliar with character encoding, it is simply a correspondance between numbers and display characters.  So, characters are stored in memory as binary numbers, just like anything else, but the system uses the encoding to convert to a display character when it's asked to show the character.  Unicode is originally based on ASCII, a 128-character encoding often taught, but it's been greatly expanded to include many other languages' characters and other helpful characters.

The most confusing thing about characters is converting between a number and a character, or when you need characters' number properties to achieve something.  If this happens, definitely explain character encoding and demonstrate with `char->integer` and `integer->char`.

### Symbols

Symbols are easy to explain by themselves to first time programmers.  They are just ordered sets of characters.  However, as soon as they learn about strings, it all gets confusing.  So, first, symbols are not strings.  Do not try to do string things on them.  It will not work.

Now, symbols are mostly used as identifiers and are a constant value; they are not mutable.  You can still display symbols just like you can strings.  The difference between symbols and strings is that symbols are normally "interned", which means that if two symbols have the same content they will be in the same memory location.  But, explaining interning and memory to a first-time programmer is overwhelming and unnecessary.  For our purposes, unless someone asks specifically, symbols are just an unchangeable ordered sets of characters.

### Strings

Strings, like symbols, are ordered sets of characters.  They can be mutable or immutable.  Trying to change an immutable string will result in an error (hopefully, that is obvious).  Strings, unlike symbols, are each separate entities from each other (they are not interned), and they also have a [https://docs.racket-lang.org/reference/strings.html](host of functions) that makes working with them relatively easy.  Highlights include `string-length`, `substring` and `string-ref` (character at function). Strings are meant to be used for text inputs and outputs (whereas symbols were for identifiers).  Note that you can convert symbols into strings using the `symbol->string` function.

If you're asked to explain strings in a little more depth, the most important thing is that strings are arrays of characters.  Each character has a position in the string, and that character can be changed (for mutable strings).  The array is of fixed length, so if you try to add a character, a whole new array is created to contain the now longer string.  The opposite goes for removing a character.  There's not too much other nuance going on besides character encoding, but I've covered that under characters.

### Functions
Functions are a primitive type, or a class of primitive types, but we've covered them under **Anonymous Functions**.

## Collections

### Structs

Structs contain a set of named values that go together.  These values can be of any type, primitive or not, which allows for recursive structs; structs which contain one or more of themselves inside.  Every struct is created with accessor and, when we get to mutation, mutator methods, a constructor method, and a predicate (`number?`, `boolean?`, `char?` are other predicates).  This is *very important* to stress to students.  When people have trouble with structs, it's usually the syntax of how to get values out of the struct--the accessor methods.  The students will be confused at where the information is, so it's important to talk about the struct having values inside it that need to be accessed.  Now, talking all in theory will make it all go over people's heads, so use some concrete examples:

`posn`s are structs, and by the time we cover structs the students will have seen them.  `posn` was defined like so:

```scheme
(define-struct posn (x y))
```

Within that `define-struct` call, it's as if all of the following happened:

```scheme
#constructor
(define (posn x y) ...)

#accessors
(define (posn-x a-posn) ...)

(define (posn-y a-posn) ...)

#predicate
(define (posn? an-obj) ...)

# mutators.. only if the field is mutable (not covered initially with structs, but covered later)
(define (set-posn-x! a-posn x) ...)

(define (set-posn-y! a-posn y) ...)
```

So, now we can use those methods to work with `posn`s:

```scheme
#creates a posn and binds it to the identifier my-posn
(define my-posn (posn 1 5))

(posn-x my-posn)
> 1

(posn-y my-posn)
> 5

(posn? my-posn)
> #t

(posn? 4)
> #f

(posn? (posn 3 7))
> #t

(set-posn-x! my-posn 4)
(posn-x my-posn)
> 4

(define (dist-between posn1 posn2)
  (sqrt (sqr (- (posn-x posn2) (posn-x posn1)))
        (sqr (- (posn-y posn2) (posn-y posn1)))))

(dist-between (posn 7 -3) (posn 4 1))
> 5

(dist-between my-posn my-posn)
> 0
```

Another example just to get your mind going--feel free to generate your own and to invite your students to do the same.

```scheme
# a chess pawn
# color- which color is the piece
# loc- posn on the board where this pawn is
# moved?- has this pawn moved yet? Used to check whether it go forward two in one turn.
# (pawn symbol posn boolean)
(define-struct pawn (color loc moved?))

# which makes, internally:
(define (pawn color loc moved?) ...)

(define (pawn-color a-pawn) ...)

(define (pawn-loc a-pawn) ...)

(define (pawn-moved? a-pawn) ...)

(define (set-pawn-color! a-pawn color) ...)

(define (set-pawn-loc! a-pawn loc) ...)

(define (set-pawn-moved?! a-pawn moved?) ...)

(define (pawn? an-obj) ...)

#end internal method generation

#white pawn in its starting position in the first rank
(define my-pawn (pawn 'white (posn 1 2) #f))

(pawn-loc my-pawn)
> #<posn>

(posn-y (pawn-loc my-pawn))
> 2

(pawn-color my-pawn)
> 'white

(define (move a-pawn squares)
  (set-pawn-loc! 
    a-pawn
    (posn 
      (posn-x (pawn-loc a-pawn)) 
      (+ squares (posn-y (pawn-loc a-pawn))))))

(if (pawn-moved? my-pawn) (move my-pawn 1) (move my-pawn 2))

(posn-y (pawn-loc my-pawn))
> 4

```

Note that the way we define structures is not type-safe, that is, the system will allow the values to be of any type.  We trust the programmer to know what type the struct's values should be and to use it accordingly, so be wary.  Assigning the wrong type to a structure value can lead to some difficult to trace errors.

### Lists

According to [https://docs.racket-lang.org/reference/pairs.html](the Racket documentation), a list is: either the empty list, or a pair whose second element is a list.  So, a list is mostly a special case of the pair AKA `cons` struct.  `first` and `rest` are aliases for the accessors of a pair.  The empty list can be represented by `'()`, `null`, or `empty`.

There are several ways to construct lists:
```scheme
(cons 1 (cons 2 (cons 3 empty)))

(list 1 2 3)

'(1 2 3)

empty

null

'()

```

However, note if you want to put values bound to identifiers in a list, you can't use the quoted constructor:
```scheme
(define a 1)
(define b 2)
(define c 3)
(list a b c)
> '(1 2 3)

'(a b c)
> '(a b c)
#(obvious in retrospect.)

# you can avoid this...
`(,a ,b ,c)
> '(1 2 3)
```

The last example I show so that you know it exists, but really, don't use it.  It's called quasiquote, and you can look it up.

In general, I find the `list` syntax to be the least confusing to people.  The problem with the quoted syntax is:
```scheme
(define x '('a 'b 'c))
(first x)
> ''a
```

As you can see, if someone accidentally puts quotes before each symbol they want, (as you would need to for normal literal symbols), you get a symbol with a quote at the beginning.  Yet when you use this syntax with numbers you get numbers, not symbols.  Confusing.

People tend to have trouble understanding how to get at latter elements in a list (especially recursively), so I recommed really stressing how the `rest` function works.  You might have students define the following:

```scheme
(define (nth-element lst n)
  ...)

# try that yourself.

(define (nth-element lst n)
  (if (= n 0)
      (first lst)
      (nth-element (rest lst) (- n 1))))

# try basic things in the command window:
(rest (list 'a 'b 'c))
> '(b c)

(rest (rest (list 'a 'b 'c)))
> '(c)

(first (rest (rest (list 'a 'b 'c))))
> 'c
```

Lists of lists also tend to be confusing.  It's important to always check that students know what the type of a given list is.  Anytime there are lists involved, ask often "and this variable is a ..?", expecting "a list of lists of numbers" or whatever it may be.  As with anything, just show an example of a list of lists:

```scheme
(define lst1 (list 1 2 3))
(define lst2 (list 4 -3))
(define lst3 (list 28 930 -23 55 73))

(define lst-of-lsts (list lst1 lst2 lst3))

(first lst-of-lsts)
> '(1 2 3)

(first (rest lst-of-lsts))
> '(4 -3)

(first (first lst-of-lsts))
> 1

#etc.
```