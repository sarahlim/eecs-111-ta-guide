# `big-bang`

A simple `big-bang` tutorial. Part one is to be *explained to students in tutorial*. Part 2 is the students exercises.

Students should understand that they are not expected to finish the tutorial. If they have problems
with `big-bang` later they should go back and try to finish this one for practice.


# Part 1

`big-bang` is a reactive library for making games in the student languages.

All `big-bang` programs consist a world state, a drawing function of type `world-state -> image` and
several handler functions roughly of `event world-state -> world-state`.

The world state is just a piece of data that represents the world.

The drawing function will take the current world state and return an image to be drawn to the screen.

The handler functions take in an event (mouse click, clock tick, etc) and the current state of the
"world" and returns a new stat for the world.

Every time an event occurs `big-bang` gets the new world state using the appropriate handler and
then redraws the world.

A simple example:

This program will draw a circle that grows on each clock tick, resetting when it hits the edge of the screen.

First we require the libraries for drawing and `big-bang`:

```
(require 2htdp/universe)
(require 2htdp/image)
```

We will also want a constant for our screen size:

```
(define SIZE 100)
```

Our big bang function will only react to clock ticks so we only need one handler, plus the starting
world state and drawing function. Thus our main function looks like:

```
(define (go)
  (big-bang 0 ;; world-state, integer, represents radius of the ball
            (to-draw draw-the-world) ;; drawing function
            (on-tick grow-ball))) ;; ticking function
```


Now to the ticking function. It will grow the ball, unless the ball has reached the size of the
screen, in which case it will reset the ball size.

```
;; grow-ball: Integer -> Integer
;; grow the ball, or reset its size
(define (grow-ball radius)
  (if (> radius (/ SIZE 2))
      0
      (add1 radius)))
(check-expect (grow-ball 0) 1)
(check-expect (grow-ball (/ SIZE 2)) 0)
```

Finally the drawing function, which draws a circle with the radius of the current ball to a screen of size `SIZE`.




In generall `big-bang` programs take this form:
```
(big-bang <initial-state>
         (to-draw <drawing-func>)
         (on-tick <tick-handler-function> <optional-tick-rate>)
         (on-key <key-down-handler-function>)
         (stop-when <should-i-stop-function> <optional-last-scene-function>))
```

0. The initial state can be any type you wish

1. `to-draw` says "use this function to draw the current state".

2. `on-tick` says "use this function to advance the state each clock tick". You can provide a
   optional number for the tick rate.

3. on-key says "use this function to handle key presses". Its signature is `(world-state key-event?
   -> world-state)`. `key-event?`s are string that represent the key pressed. So "q" for q, " " for
   space, "\t" for tab, "up" for ↑ etc.

4. `stop-when` says "stop big bang whenever this function returns true". (So its a function of
   `(world-state -> boolean)`). It gets run any time a handler is run. You can provide an optional
   procedure to render the final state for you. Otherwise the `on-draw` function is used.

In addition if any handler evaluates to `(stop-with <some-world-state>)` the game will stop as if
the `stop-when` handler had returned true.

Other handlers exists. We will have you look them up in the documentation yourself later.

# Part 2

## Exersize 1

Design a bouncing ball animation. The ball should start at the top of the screen, fall until it hits the bottom, then move back up.

You will need the following things:

0. A representation of state. The only thing you need to keep track of is the height of the ball.
1. A draw function that takes this representation and draws it to a screen
2. A tick function that moves the ball based on the specification above.

## Exersize 2

Design an animation where a ball travels around the screen in a circle. The animation should stop
when the user clicks on the screen. (It does not need to resume if the player clicks again)

You will need:

0. A representation of state that keeps track of where the ball is in its circlar motion. Think back to your geometry class (I know, *shudder*). 
1. A draw function, as usual.
2. A tick function that moves the ball farther along its arc.
3. An `on-mouse` handler that stops the game. You will need to look up this handler in the
   documentation. Don't forget `stop-with` for stoping the game!

## Exersize 3

Design a simple guessing number game. The player will pick a whole number between 1 and 100, and the
game will attempt to guess it. If the number is too low the player will let the game know by
pressing *↑*. If the number is too low, the player will press *↑*. If the number is correct the
player will press *=*, which will end the game.

You will need:

0. A state that somehow represents what the game is guessing
1. Draw function that displays the current guess
2. A key handler that deals with the keys from above. If you have difficulty with this function
   don't forget to look in the documentation for examples!


