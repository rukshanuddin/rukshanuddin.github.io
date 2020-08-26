---
layout: post
title:      "Equals vs Threequals in JS"
date:       2020-08-26 03:39:45 +0000
permalink:  equals_vs_threequals_in_js
---

## Knowing the lingo matters

When it comes down to certain things in JS, they become second nature and as a developer I often will start writing certain code without giving a second thought to why I did it. "That's just the way it is" or "You use x when you need y..." But as I learned from a recent telephone screen, sometimes the simplest concepts can trip you up when it comes down to explaining them. Most developers with any experience in Javascript will instinctively use ```=```, ```==```, and ```===``` correctly. I recently got asked the question "What is the difference between ```==``` and ```===``` in Javascript and I could not explain it in a clear and concise manner. I understand the concept. I was able to convey that I did. But I took about 5 minutes to long to try and explain it. I still got to the next interview round, but for future reference, I wanted to write out the proper definitions behind all three usages of the 'equals' sign.

### = - The assignment operator

![Equals](https://png.pngtree.com/png-vector/20190721/ourmid/pngtree-equal-to--icon-in-trendy-style-isolated-background-png-image_1566189.jpg)

```=``` in JavaScript is used for assigning values to a variable. It is called as assignment operator and	can evaluate to the assigned value.

1. It does not return true or false
2. = simply assign one value of variable to another one.
3. = will not compare the value of variables at all.

```
let x = 1
```

In the example above we are using ```=``` to assign 1 to the variable x.

### == - A comparison operator

![Equal equals](https://pbs.twimg.com/profile_images/884840329211518976/_Y8omoNx.jpg)

```==``` in JavaScript is used for comparing two variables, but it ignores the datatype of variable. It is called a comparison operator and checks the equality of two operands without considering their type.

1. Return true if the two operands are equal. It will return false if the two operands are not equal.
2. == make type correction based upon values of variables.
3. The == checks for equality only after doing necessary conversations.

```
1 == "1"
```

The above would return true as 1 is equal to 1, we do not check the datatype, so it doesn't matter that the second 1 is a string.

### === - Another comparison operator

![Threequals](https://image.slidesharecdn.com/threequals-141015100309-conversion-gate01/95/threequals-case-equality-in-ruby-1-638.jpg?cb=1413367480)

```===``` is used for comparing two variables, but this operator also checks datatype and compares two values. It is also called a comparison operator and compares equality of two operands with their types.

1. It returns true only if both values and data types are the same for the two variables.
2. === takes type of variable in consideration.
3. If two variable values are not similar, then === will not perform any conversion.

```
1 === 1
```

The above would return false as the first 1 is an integer, while the second one is a string. Since the datatypes are different, it is inconsequential that 1 and "1" are the same, since strictly speaking strings and integers are not the same.

### The moral of the story

Looking back, it is a simple question with a simple answer. To answer the question concisely:

> == is a comparison operator that will compare two values while === will also account for the datatypes.
