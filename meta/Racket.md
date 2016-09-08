# On Racket

Sooner or later, you are going to face backlash from a student who would rather drop the course than walk a linked list recursively.

Racket is an excellent teaching language, period. There remains a (endless) debate as to whether Racket is the *best* language for an introductory computer science course, but its offerings for both first-time programmers and veterans cannot be denied.

Functional programming is in vogue right now, and ideally, everyone would leave EECS 111 with the mental models necessary to explore FP themselves. Unfortunately, this foundation can be difficult to achieve when students are more interested in griping about parens than actually learning principles of abstraction.

Matt Might provides a candid response to the haters in his blog post, ["What every computer science major should know"](http://matt.might.net/articles/what-cs-majors-should-know/):

> Racket, as a full-featured dialect of Lisp, has an aggressively simple syntax.
>
> For a small fraction of students, this syntax is an impediment.
>
> To be blunt, if these students have a fundamental mental barrier to accepting an alien syntactic regime even temporarily, they lack the mental dexterity to survive a career in computer science.[^1]

As a TA, you are uniquely situated to help students grasp the importance of Racket and FP from the start. Here are some reasons.

## Tooling

> "What is this 'DrRacket' thing I have to install? I hate it"

DrRacket is an outstanding IDE for learning. Features including code alignment, bracket-matching, the stepper, interactive program REPL, and test coverage lower the barrier to entry and facilitate the development of mental models for reasoning about code.

If students *really* hate their development environment, check back in after EECS 211 starts to see whether Xcode has changed their opinions.

## Syntax

> "But there are so many parentheses idk"

In comparison to the curly-brace, arrow-operator, angle-bracket hellscape of C-style languages, Racket's parenthetical syntax is easy to remember and reason about.

As an aside, it's likely that students complaining about readability aren't fully aware of DrRacket's indentation management. Show them how to split lines and align arguments in a coherent manner.

And, for the love of Knuth, stop

```scheme
(this egregious practice
)
```

in its tracks.

## Practical applications

> "Nobody uses Racket irl lol"

Early introduction of the functional paradigm is one of the best reasons to learn with Racket. FP is easy to reason about (everything boils down to order of operations and substitution), and the Medium article ["Functional Programming should be your #1 priority for 2015"](https://medium.com/@jugoncalves/functional-programming-should-be-your-1-priority-for-2015-47dd4641d6b9#.k12sexuzp) provides a succinct summary of its role in modern software engineering.

In particular, the Racket syntax and way of thinking are highly applicable to:

- **Non-majors, and those pursuing careers in mathematics, statistics, and economics**, due to the close coupling between functional programming and mathematical representation (quant firm Jane Street [uses OCaml](https://blogs.janestreet.com/category/ocaml/), a functional language);
- **Web developers on both ends of the stack**, as modern web frameworks and technologies (React, Flux/Redux, Scala, Ember) are becoming increasingly functional in nature, and this learning curve is a barrier for many developers who have only written imperative-style programs;
- **Anyone interested in artificial intelligence, knowledge representation, game development, or programming languages**. Lisp/Scheme and Racket, respectively, appear frequently in these domains.


[^1]: Generally speaking, the notion of "predisposed fitness" for the computer science field has been applied in harmful ways. Our guiding principle is to make the computer science program as inclusive as possible to everyone interested. But for the small subset of students whose complaints become self-aggrandizing, in a sense, this should help them understand the big picture.
