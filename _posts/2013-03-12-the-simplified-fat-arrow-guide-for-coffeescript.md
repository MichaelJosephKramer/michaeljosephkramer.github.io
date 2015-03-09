---
layout: post
title: The Simplified Fat Arrow Guide for CoffeeScript
---

CoffeeScript introduces some fancy new operators and syntax enhancements, but one of the most misunderstood additions is the "fat arrow" (`=>`) operator. The JavaScript `function` keyword was replaced with the "thin arrow" (`->`), but the fat arrow serves as the `function` keyword and binds the function to the current context.

## TL;DR

*if callback and @, use arrow the fat*

It's a stupid little phrase, but try to forget it now.

To elaborate just a little, if the function is an event handler or a callback, and you have a reference to the current context (i.e., "@") in the function, use the the fat arrow. Otherwise, the thin arrow will be fine.

## A Little Bit of Context (har!)
Context in CoffeeScript, and JavaScript, generally follows a few simple rules. Take a function named `makeManhattan`:

1. The context is the thing to the left of the dot. Here, `bartender` is the context.
`bartender.makeManhattan()`

2. If there's nothing to the left of the dot, the context is the global context.
`makeManhattan()`

3. If the function is an event handler or callback, the context is the event owner or caller. Here, it's `button`:
`button.onClick(makeManhattan)`

Rule #3 starts to get a little tricky when callbacks are passed around:

~~~coffeescript
class Bartender
    drinkName: -> 'Manhattan'

    # callback and @, use arrow the fat
    bourbonCallback: => "Maker's Mark #{@drinkName()}"

    # yup, use the fat arrow again
    ryeCallback: => "Bulleit Rye #{@drinkName()}"

    # callback and no @, so no fat!
    oldCrowCallback: -> "How about a Bud Light instead?"

# simple callback -- will alert the result of the function you pass in
callbacker = (callback) -> alert(callback())

bartender = new Bartender()

callbacker(bartender.bourbonCallback)
callbacker(bartender.ryeCallback)
callbacker(bartender.oldCrowCallback)
~~~

In this example, you'll get an nasty message when you change the fat arrows to thin arrows in the `Bartender` class. Feel free to play with it here: [http://jsfiddle.net/uxCCD/7/](http://jsfiddle.net/uxCCD/7/ "http://jsfiddle.net/uxCCD/7/")

## Can I Just Use the Fat Arrow All The Time?

The world won't end, but it may cause minor performance degredation because the CoffeeScript compiler will have to generate a little more JavaScript -- in other words, probably not a huge deal in most scenarios. Just follow the rules, and you'll know when to use it.

Having said that, the fat arrow has been known to quickly fix some failing test. You'll impress your friends.
