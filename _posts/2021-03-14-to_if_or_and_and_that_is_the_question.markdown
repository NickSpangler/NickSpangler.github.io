---
layout: post
title:      "To if or &&, that is the question..."
date:       2021-03-14 17:31:55 +0000
permalink:  to_if_or_and_and_that_is_the_question
---

Navigating which syntactical style to use in conditional programming can be a bit overwhelming to new developers. If you're like me, you enjoy asking the question, "Well, which version is *right*, or the best practice?" And here, there really isn't a concise answer. 

In JavaScript, you have many choices at your disposal. When learning the basics of if/else logic, the most straightforward syntax is something like this:

```
if (x > 9) {
    console.log('This number has at least two digits!')
} else {
    console.log('This number is a single digit!')
}
```

There you go. Surrounded by curly braces and pleasingly indented, the human eye (and brain!) can very easily and quickly understand that in those five lines of code, you look at the conditional on the first line, and if it's true, execute the first block, but if it's false, execute the second block. Easy peasy. 

But five-line chunks of code can start to take up a lot of real estate as your program grows. So there are other options available to us. We can maintain the exact same logic thusly:

```
if (x > 9) console.log('This number has at least two digits!');
else console.log('This number is a single digit!');
```

Ah! We've broken out of the warm embrace of our curly braces! We even (optionally) end each statement with a semicolon, indicating they are individual statements! How are we to know those two lines of code are inately linked to one another!? This is all about readability and personal preference. Both options execute identically, but which do you prefer?

As you would imagine, as our logic increases in complexity, our syntactical options increase as well. We can utilize the && operator to increase our conditional checks. For example:

```
if (x > 9 && y > 9) console.log('Adding X to Y will result in at least a two-digit number.')
```

Here we are only logging our statement if *two* conditions are met. And one of the best parts about the && operator is what's called short-circuiting. Short circuiting is when the left side of the && operator evaluates to false, and the program *never even evaluates the right side*. It has already found a falsey statement, so it doesn't even bother looking up Y and finding its value. This speeds up the execution of the code, and can be very handy.

However, this handiness can result in some dangerous territory... 

Taking one step back in our discussion, we can actually rewrite one of those first conditionals as:
```
x > 9 && console.log('This number has at least two digits!')
```

Wow. No curly braces... not even the word *if*! But it works. When that line of code is executed, the short-circuiting takes place, and if x is indeed greater than 9, then the console.log will execute. But if x is *not* greater than 9, well, we're out of luck. On to the next line of code... There's some debate as to whether this is good practice or not. You may personally enjoy that syntax, but how will another developer reading your code react? Will they recognize it right away? Will they see it for what it is, a simple if statement?

Here's another wrinkle: this kind of syntax may not always behave the way you'd expect it to. As noted in [this](https://stackoverflow.com/questions/10308480/using-as-a-shorthand-if-statement/10308492) Stack Overflow question, the following code is fine:

```
var var1 = 5;
var var2 = 1;

var1 == 5 && var2++;
```

But the code below results in the error `Uncaught ReferenceError: Invalid left-hand side in assignment`:

```
var var1 = 5;
var var2 = 1;

var1 == 5 && var2 = 2;
```

As one user notes, the `&&` operator has higher precedence than the `=` operator. So the code is being interpreted like this:

`(var1 == 5 && var2) = 2; `

So to make it execute properly, you need the correct parentheses around the right-hand side of the &&:

`var1 == 5 && (var2 = 2); `

So we're in murky waters... However, there *are* some good use cases for this special utilization of the && operator. For example, guarding against errors resulting from something being undefined. 

I recently completed a React/Redux coding challenge which involved swapping two items in an array maintained in the Redux store. It involved finding the index of my desired item, and switching it with the item before it (at index - 1). Before I refactored the code, it took me three lines to swap the two items, like so:

```
let temp = state[index]
state[index] = state[index -1]
state[index -1] = temp
```

Though it's clear, I wanted to simplify it on the screen, so I attempted to use array destructuring to make the swap a one-liner: 

```
[state[index], state[index - 1]] = [state[index - 1], state[index]]
```

It saves me some real estate, and there's no need to create a temp variable. *However*, I ran into an issue. When the reducer is compiled, I hit an error at that line because I am trying to access specific indicies of my `state` variable, which in the compiling phase has been declared, but not assigned. What a wrench in my whole operation. But after some fiddling, I found that if I used our handy `&&`, I could still simplify the statement to one line, but also avoid throwing an error. So the final code looked like this:

```
state.length && [state[index], state[index - 1]] = [state[index - 1], state[index]]
```

Thus, when compiling, if `state` has no length (it won't) then we can ignore the right-hand side of the `&&`, but when an action is dispatch to the reducer, and this specific case statement is hit, `state` will indeed have length (it was passed as an argument) and so we *will* execute my swapping one-liner. Success!

Thanks `&&`!
