---
layout: post
title:      "JavaScript Map v. Object"
date:       2021-03-22 02:06:59 +0000
permalink:  javascript_map_v_object
---


As I continue to work my way through Leetcode problems, more and more often I see other people's solutions that include a concept that was honestly never addressed in my bootcamp. Generally it looks something like:

`let somethingSomething = new Map()`

I've put it off long enough, vowing to focus more intently on concepts like frequency counters, multiple pointers, stacks and queues, etc. So let's dive in and see what a JS map is all about.

I started my research [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map), just to start to familiarize myself. As MDN notes, in simplistic terms, a Map is very similar to an Object, just with some different rules. A Map is an object that holds key-value pairs. But there are some key differences.

**Key difference:** a Map remembers the original insertion order of the keys. So when iterating over the keys, or the entries, it will iterate similar to an array (in order) rather than the possibly random order of iterating over an Object.

**Key difference:** a Map will accept *any type of data* as a key, and keep it that way. Objects will only accept strings or symbols as keys. Furthermore, if you *try* to set a different datatype as a key in an Object, it will implicitly convert it for you, leading to sometimes unexpected results. 

The easiest example here is if you create an object with *numbers* as keys, look what happens:

```
let obj = {}
obj[1] = 'one'
obj[2] = 'two'

typeof Object.keys(obj)[0]
//=> 'string'
```

We set the *number* 1 as a key, but once it's in the object, it's now the *string* '1'. 

Conversely, a Map will retain whatever datatype you set as a key. Even the booleans true and false. 

**Key difference:** iteration is much easier with a Map as well. As seen above, an Object requires the use of static methods from the Object prototype in order to iterate. So you're left with the ugly syntax of calling `Object.keys()` or `Object.entries()` and then passing your desired object as an argument. A Map makes it so much easier, with dot notation available directly on the variable holding the Map, as in `myMap.keys()` with no argument passed, or even a `for...of` loop, which will return an array of `[key, value]` for each entry.

```
let myMap = new Map([[1, 'one'], [2, 'two']])
for (const [key, value] of myMap) {
     console.log(key, value)
}
//=> 1 "one"
//=> 2 "two"
```

**Key difference:** determining the number of key-value pairs in a Map is also much easier than in an Object. With an object, you must determine the number of items manually, with something like `Object.keys(myObject).length`. In other words, call a static method on the Object to get an iterable array of keys, and then call the static `length()` method on that array to determine how many there are. With a Map, this functionality is built right in. So go right for the shortcut of `myMap.size` - simple as that.

Combine the more elegeant syntax for iteration and for determining the size of a Map with the fact that it *also* iterates through its key-value pairs in order of insertion, and you can start to see why a Map is a popular go-to for algorithm challenges. It's not a replacement for a POJO, but rather a handsome sibling.

Happy coding.



