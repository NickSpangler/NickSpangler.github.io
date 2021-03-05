---
layout: post
title:      "Creating JavaScript Arrays for those Leetcode Results"
date:       2021-03-05 19:49:33 +0000
permalink:  creating_javascript_arrays_for_those_leetcode_results
---


Post graduation, as I interview at various companies with varying levels of technical interviews and challenges, I also spend parts of my day practicing challenge after challenge on Leetcode. The expected data type of the output from each challenge varies, but a very common one is an array, often mirroring the original input in some way (most likely an array of the same length, but with some computed data at each index)

It can be very helpful to have multiple ways to generate this "results array" at your disposal, so let's take a look at a few options now!

# 1. Array Literal
This is the obvious one. The one you probably use every day. Declare a variable name and use the assignment operator (single equal sign) to set it equal to an actual array you write yourself.
```
let results = [1, 2, 3, 4]
```
However, this only gets you so far... and your results are likely to change over the course of your algorithm. But even if you initialize each element with a value of zero, your array length will likely need to change with different inputs! 

# 2. The new Keyword
The other basic method to declare a new array is using the `new` keyword. It's similar to a literal, but with clunkier syntax and some corner-case side effects you want to look out for. To get the same array from the first example you could write:
```
let results = new Array(1, 2, 3, 4)
=> [1, 2, 3, 4]
```

But here's what to look out for - if you're working strictly with numbers, using the new keyword with a *single* argument of a number, will *not* produce a single-element array of that number. It will produce an array with a length property set eqaul to that number, with each element undefined. So:
```
let restults = new Array(3)
=> [empty x 3]
```

This is not super helpful, so this construction method can generally be avoided. If you're truly desperate to create a single-element array in this manner, you can reach for Array.of(), which will accomplish exactly that. 
```
let results = Array.of(3)
=> [3]
```

# 3. Array.from()
the Array.from() method creates a new, *shallow-copied* instance of an array from an iterable object. As seen [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from):
```
console.log(Array.from('foo'));
// expected output: Array ["f", "o", "o"]

console.log(Array.from([1, 2, 3], x => x + x));
// expected output: Array [2, 4, 6]
```

You can see there is an option to pass a second argument, which is a map function that will operate on every element in the new array.

# 4. Array.fill()
The Array.fill() method will replace the values of an array with a static value, and takes optional arguments of an index to start at, as well as an index to end at. This can come in handy when you want to initialize an array with all zeros, perhaps as counters for your results. To declare this dynamically, based on the input, you could grab the length of your input and fill it with zeros:
```
const input = [12, 33, 15, 98, 100]
let results = Array.from(input).fill(0)
=> [0, 0, 0, 0, 0]
```

Your results are now initialized with the same length as your input, but with all values starting at zero!

These are clearly but one of many tools you'll need to handle and manipulate data in Leetcode challenges, but it's a good place to start!

