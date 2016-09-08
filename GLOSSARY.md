# Glossary

Jargon you might hear thrown around during meetings or grading sessions.

## HtDP2e

*[How to Design Programs, 2nd ed.](http://www.ccs.neu.edu/home/matthias/HtDP2e/)* is the current edition of the text written by Felleisen, Findler, Flatt, and Krishnamurthi. Sara Sood's version of the course uses this text.

Also referred to as "HtDP" for short.

## SICP

*Structure and Interpretation of Computer Programs*, the MIT text by Abelson and Sussman. Arguably pioneered the approach to teaching introductory computer science courses in terms of abstraction and design patterns, rather than the semantics of a particular language.

## Parens

Parentheses `()`. For whatever reason, programmers tend to use "parens" instead of "parentheses."

## TDD

Test-driven development. Refers to the practice of writing software by first defining expected behaviors through (initially failing) tests, then writing code to make the tests pass.

## Higher-order function

A function that does at least one of the following:

- Takes another function as input,
- Returns a function.

Higher-order functions are an important aspect of the functional programming paradigm, because they provide abstractions for function composition and reuse. Examples include `map`, `filter`, `foldl`, and `lambda`.

## Predicate

A function that returns a Boolean value. Per Racket convention, predicate names tend to end with a question mark (`?`). Examples include `empty?`, `<`, and `list?`.
