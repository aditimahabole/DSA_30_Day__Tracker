# 20. Valid Parentheses

## Problem Statement

Given a string `s` containing only:

- `(`
- `)`
- `{`
- `}`
- `[`
- `]`

Return `true` if the input string is valid.

A string is valid if:

1. Every opening bracket has a corresponding closing bracket.
2. Brackets are closed in the correct order.
3. Every closing bracket matches the most recent unmatched opening bracket.

### Examples

```
Input: "()"
Output: true
```

```
Input: "()[]{}"
Output: true
```

```
Input: "(]"
Output: false
```

```
Input: "([)]"
Output: false
```

```
Input: "{[]}"
Output: true
```

---

# Thought Process

The important observation is:

> **The last opened bracket must be closed first.**

Example:

```
({[]})

Open (
Open {
Open [

Now close ]

Then }

Then )
```

This follows the **Last In First Out (LIFO)** principle.

The data structure that follows LIFO is a **Stack**.

---

# Approach 1 : Remove Matching Pairs (Brute Force)

## Idea

Keep removing

```
()
{}
[]
```

from the string until nothing changes.

Example

```
({[]})

↓

({})

↓

()

↓

""
```

If the string becomes empty, it is valid.

Otherwise, it is invalid.

### Time Complexity

```
O(n²)
```

### Space Complexity

```
O(n)
```

### Why not recommended?

Removing characters from a string repeatedly is expensive because remaining characters shift every time.

---

# Approach 2 : Stack (Most Common Interview Solution)

## Idea

Traverse the string from left to right.

### If current character is

```
(
{
[
```

Push it into the stack.

### If current character is

```
)
}
]
```

Then

- Stack should NOT be empty.
- Top element should be matching opening bracket.
- Otherwise return false.

Finally,

If stack becomes empty

Return true.

Else

Return false.

---

# Dry Run

Input

```
([{}])
```

Initially

```
Stack

empty
```

Read

```
(
```

Push

```
(
```

Read

```
[
```

Push

```
[
(
```

Read

```
{
```

Push

```
{
[
(
```

Read

```
}
```

Top is

```
{
```

Correct

Pop

```
[
(
```

Read

```
]
```

Top

```
[
```

Correct

Pop

```
(
```

Read

```
)
```

Top

```
(
```

Correct

Pop

Stack becomes empty.

Return

```
true
```

---

# Invalid Example

Input

```
([)]
```

Read

```
(
```

Push

Read

```
[
```

Push

Stack

```
[
(
```

Read

```
)
```

Top is

```
[
```

Expected

```
(
```

Mismatch

Return

```
false
```

---

# Java Solution (Best)

```java
class Solution {

    public boolean isValid(String s) {

        Stack<Character> stack = new Stack<>();

        for(char ch : s.toCharArray()){

            if(ch=='(' || ch=='{' || ch=='['){

                stack.push(ch);

            }else{

                if(stack.isEmpty())
                    return false;

                char top = stack.pop();

                if((ch==')' && top!='(') ||
                   (ch=='}' && top!='{') ||
                   (ch==']' && top!='['))
                    return false;
            }
        }

        return stack.isEmpty();
    }
}
```

---

# Time Complexity

```
O(n)
```

Every character is pushed and popped at most once.

---

# Space Complexity

```
O(n)
```

Worst case

```
(((((((
```

All opening brackets remain in stack.

---

# Approach 3 : Stack with Expected Closing Bracket (Cleaner Solution)

Instead of pushing opening brackets,

Push the **expected closing bracket**.

Example

Read

```
(
```

Push

```
)
```

Read

```
[
```

Push

```
]
)
```

Now whenever a closing bracket comes,

Simply compare

```
stack.pop() == currentCharacter
```

No long if-condition required.

---

# Java Solution

```java
class Solution {

    public boolean isValid(String s) {

        Stack<Character> stack = new Stack<>();

        for(char ch : s.toCharArray()){

            if(ch=='(')
                stack.push(')');

            else if(ch=='{')
                stack.push('}');

            else if(ch=='[')
                stack.push(']');

            else{

                if(stack.isEmpty() || stack.pop()!=ch)
                    return false;
            }
        }

        return stack.isEmpty();
    }
}
```

---

# Time Complexity

```
O(n)
```

# Space Complexity

```
O(n)
```

---

# Approach 4 : HashMap + Stack

## Idea

Store matching brackets inside a HashMap.

```java
Map<Character, Character> map = new HashMap<>();

map.put(')', '(');
map.put('}', '{');
map.put(']', '[');
```

Algorithm

- Opening bracket → Push
- Closing bracket → Compare with `map.get(ch)`

### Java Code

```java
class Solution {

    public boolean isValid(String s) {

        Stack<Character> stack = new Stack<>();

        Map<Character, Character> map = new HashMap<>();

        map.put(')', '(');
        map.put('}', '{');
        map.put(']', '[');

        for(char ch : s.toCharArray()){

            if(ch=='(' || ch=='{' || ch=='['){

                stack.push(ch);

            }else{

                if(stack.isEmpty())
                    return false;

                if(stack.pop()!=map.get(ch))
                    return false;
            }
        }

        return stack.isEmpty();
    }
}
```

---

# Edge Cases

### Empty String

```
""
```

Output

```
true
```

---

### Starts with Closing Bracket

```
")("
```

Output

```
false
```

Reason

Stack is empty.

---

### More Opening Brackets

```
(((
```

Output

```
false
```

Reason

Stack is not empty after traversal.

---

### More Closing Brackets

```
())
```

Output

```
false
```

Reason

Trying to pop from empty stack.

---

# Why Stack?

Because

```
(
{
[
```

must be closed as

```
]
}
)
```

The **last opened bracket is always closed first.**

This is exactly

```
LIFO

Last In First Out
```

which is implemented using a **Stack**.

---

# Interview Questions

## Q1. Why Stack?

Because parentheses follow **LIFO** order.

---

## Q2. Why check stack.isEmpty() before pop()?

To avoid popping from an empty stack.

Example

```
")("
```

---

## Q3. Why check stack.isEmpty() at the end?

If stack still contains elements,

Some opening brackets were never closed.

Example

```
(((
```

---

## Q4. Can this be solved without extra space?

No.

We must remember previously opened brackets.

Worst case

```
(((((((
```

requires storing all opening brackets.

---

# Complexity Comparison

| Approach | Time | Space | Recommended |
|----------|------|-------|-------------|
| Remove Matching Pairs | O(n²) | O(n) | ❌ |
| Stack | O(n) | O(n) | ✅ Best |
| Expected Closing Stack | O(n) | O(n) | ⭐ Cleanest |
| HashMap + Stack | O(n) | O(n) | ⭐ Flexible |

---

# Pattern Learned

Whenever you see

- Nested structures
- Matching symbols
- Undo operations
- Expression evaluation
- Last opened should close first

👉 Think **STACK**

---

# Similar Interview Questions

1. Min Stack (155)
2. Evaluate Reverse Polish Notation (150)
3. Daily Temperatures (739)
4. Next Greater Element I (496)
5. Largest Rectangle in Histogram (84)
6. Asteroid Collision (735)
7. Remove All Adjacent Duplicates in String (1047)
