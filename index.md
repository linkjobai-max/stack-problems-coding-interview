# Stack Problems in Coding Interviews: Valid Parentheses, Min Stack and Friends

Stacks show up in coding interviews far more often than their simple definition suggests. Whenever a problem asks you to match, undo, or "remember the most recent thing that hasn't been resolved yet," a stack is usually the cleanest tool. This guide walks through the handful of stack problems that come up again and again, and the reasoning that ties them together.

## Why stacks keep appearing

A stack gives you last-in, first-out access in O(1). That property maps directly to three kinds of interview problems:

1. **Matching pairs**: brackets, tags, or anything that opens and must close in reverse order.
2. **Deferred decisions**: you can't decide what to do with an element until you see something later (expression evaluation, "decode string").
3. **Undo / history**: browser back buttons, text editor undo, path simplification.

If you can say out loud which of these three a problem belongs to, you've usually found the solution shape.

## 1. Valid parentheses

Push every opening bracket. When you see a closing bracket, the top of the stack must be its partner; otherwise the string is invalid. At the end, the stack must be empty.

```python
def is_valid(s):
    pairs = {')': '(', ']': '[', '}': '{'}
    stack = []
    for ch in s:
        if ch in pairs:
            if not stack or stack.pop() != pairs[ch]:
                return False
        else:
            stack.append(ch)
    return not stack
```

Edge cases interviewers like to probe: an empty string (valid), a string that starts with a closing bracket, and a string with only opening brackets.

## 2. Min stack

Design a stack that returns its minimum in O(1). The trick is to store, alongside each value, the minimum at the time it was pushed. Popping then automatically restores the previous minimum.

```python
class MinStack:
    def __init__(self):
        self.items = []  # (value, current_min)

    def push(self, x):
        m = x if not self.items else min(x, self.items[-1][1])
        self.items.append((x, m))

    def pop(self):
        self.items.pop()

    def top(self):
        return self.items[-1][0]

    def get_min(self):
        return self.items[-1][1]
```

Mention the space trade-off: a second stack that only stores new minimums uses less memory when values are mostly increasing.

## 3. Evaluate reverse Polish notation

Numbers go on the stack; an operator pops two numbers, applies itself, and pushes the result. The only subtle part is operand order (the second pop is the left operand) and integer division that truncates toward zero.

## 4. Decode string

For input like `3[a2[c]]`, push the current string and repeat count when you hit `[`, and when you hit `]`, pop them and build `previous + count * current`. This is the "deferred decision" pattern: you don't know what to do with `a` until the inner bracket closes.

## 5. Simplify a Unix path

Split on `/`, skip empty parts and `.`, pop on `..`, push everything else. Join the stack at the end. It's a small problem, but it shows you understand the undo pattern.

## How to talk through a stack problem

- Say why a stack fits before you write code: "I need the most recent unmatched opener, so a stack gives me that in O(1)."
- Trace a short example by hand, writing the stack contents after each step.
- State complexity: almost all of these are O(n) time and O(n) space in the worst case.
- Test the empty input and a deeply nested input.

## Practice suggestion

Do the five problems above in order, then try "daily temperatures" and "largest rectangle in histogram," which move into monotonic stacks. Practicing them out loud, with someone (or something) asking follow-up questions, is the closest thing to the real interview. If you want feedback on how you explain your approach in a live setting, [Linkjob AI](https://www.linkjob.ai/coding-interview-copilot/) is one option.

*Disclosure: I work on Linkjob AI.*
