---
layout: post
title: "One problem, many solutions"
date: 2022-10-05 22:00:00 +0200
categories: algorithms problem-solving challenge code
---

# One problem, many solutions

Hi everyone. It's been a while since my last blog post. But today I'm talking about the different ways to solve a problem. Also, I want to point out that, as you will see, there is no perfect solution. Every solution has its downsides. It really depends on the context. Maybe your main concern is memory. Maybe it's CPU cycles. Another thing I want to point out is that these are my solutions, and thus not the only ones. Maybe you have a completely different (and valid) solution. With that said, let's explain the problem we're solving.

## The problem
The problem is bracket matching. Let's say we have a string that consists of the characters `{}[]()<>`. You want to check if said string is syntactically correct, following obvious agrupation rules. You'll unerstand with the following examples:
```
()      correct
(]      incorrect
(<>)    correct
(<)>    incorrect
{       incorrect
)       incorrect
[[]()]  correct
{{}}    correct
```
Easy, right? Now, how would you write a program to check this? I really encourage you to try it out for yourself.

## Solution 1: Stack-based
One thing you may have noticed is that, if you start reading from left to right, and trying to remember the context you're in (e.g. inside of `<>`), you'll remember it in a stack-based way. Let's say you read `(<>)`. Stack would look like this:
```
| STACK                 | STRING |
| (empty)               | ----   |
| parentheses           | (---   |
| parentheses, angles   | (<--   |
| parentheses           | (<>-   |
| (empty)               | (<>)   |
```
Notice that the stack ends up being empty. If there was something left there, that would mean we're missing some closing brackets, and thus the string is wrong.

Also, when we encounter a closing bracket, we only remove it if it matches the last value on the stack. If not, it's a bracket type mismatch (e.g. `(>`).

Last, if we encounter a closing bracket but the stack is empty, that's also wrong.

So that's basically it. Now let's talk a bit about the performance.

### Upsides
 - This is O(n), not bad!
 - Really easy to implement

### Downsides
 - Not scalable
 - You should have a sufficiently big stack (half of the string length) to ensure no stack overflows happen

## Solution 2: Find and delete
This solution is based on the fact that, for any correct string, we can do some things repeatedly until the string is empty. We basically do:
1. Find pairs that are together (e.g. `{}` in `[{}]`)
2. If found, remove

Notice that, if we don't find any pair and string is not empty, it's not a valid string. Easy peasy!

But you may have noticed that this is not very efficient...

### Upsides
 - Really easy to implement

### Downsides
 - Not scalable
 - O(n^2) (well, it's O(n/2\*n) really, but it's still so bad)

## Solution 3: Recursion
This solution is similar to the first one, but with function calls (and thus the call stack).

We have a string and an index. The index starts at 0. Then we do:
1. Remember the character in string[index]
2. Increase index by 1
3. If the character at string[index] is the closing of the one remembered at step 1., return quietly. If not, call itself (yes, this step list)
4. Increase index by 1
5. Check if the character at string[index] now matches the one remembered at step 1. If it does, return quietly. If not, PANIC!!! The string is wrong!

It may seem a little bit complicated. But you'll understand it better if I write it as pseudocode:

Here I use the operator `=|=` to check if the second operand is the closing bracket to the first operand, so that `"<" =|= ">"` would be true.

```
int idx = 0;
string str = "{()}";

// Check if str is a valid string or not
bool isValid(){
    char opening = str[idx];
    idx++;
    if(str[idx] =|= opening) return true;
    else{
        if(!isValid()) return false;
    }
    idx++;
    if(str[idx] =|= opening) return true;
    else return false;
}

print(isValid());
```

I hope you understand that better.

### Upsides
 - It's really scalable (you could build an entire programming language parser from this)
 - It's O(n)

### Downsides
 - It's a bit overkill if you only want to do this
 - It uses more memory than the first solution, so you should consider using that one
 - Recursion can cause a lot of stack-realated problems

This 3 solutions should cover a wide variety of possible use cases, and demonstrate that there are many ways to solve one problem.

Anyway, thank you for reading this. Consider following me on [GitHub](https://github.com/Arnau478), [Twitter](https://twitter.com/Arnau478) or subscribing via RSS. But that's all I got for you today. Hope you enjoyed, and see you next time! (hopefully that's not in 2 months)
