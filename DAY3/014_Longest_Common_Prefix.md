# File Name

```text
014_Longest_Common_Prefix.md
```

---

# 14. Longest Common Prefix

**Difficulty:** Easy

**Pattern:** String | Vertical Scanning

---

# Problem Statement

Given an array of strings, return the **longest common prefix** among all the strings.

If there is no common prefix, return an empty string `""`.

---

## Example 1

```text
Input:
["flower","flow","flight"]

Output:
"fl"
```

Explanation

```text
flower
flow
flight
```

Compare from left to right.

```text
f  ✔
l  ✔
o  ✖ (flight has 'i')
```

Longest Common Prefix =

```text
fl
```

---

## Example 2

```text
Input:
["dog","racecar","car"]

Output:
""
```

Explanation

```text
dog
racecar
car
```

First character itself is different.

No common prefix exists.

---

# What is a Prefix?

A prefix is a sequence of characters that starts from **index 0**.

Example

```text
String = "flower"

Prefixes:

""
"f"
"fl"
"flo"
"flow"
"flowe"
"flower"
```

Notice

```text
"low"
```

is **NOT** a prefix because it doesn't start from index 0.

---

# Understanding the Problem

The question is asking:

> What is the longest sequence of starting characters that is common in **every** string?

Example

```text
flower
flow
flight
```

Common characters

```text
f ✔
l ✔
o ✖
```

Answer

```text
fl
```

---

# How to Think?

Don't think about comparing complete strings.

Think about comparing **one column at a time**.

Example

```text
flower
flow
flight
```

Index 0

```text
f
f
f
```

All same ✔

---

Index 1

```text
l
l
l
```

All same ✔

---

Index 2

```text
o
o
i
```

Mismatch ❌

Stop immediately.

Answer =

```text
fl
```

---

# Biggest Observation

A prefix grows **character by character**.

The moment a mismatch occurs,

the prefix **cannot become longer**.

So we stop immediately.

---

# Why Do We Compare Only Till the Shortest String?

Example

```text
flower
flow
flowing
```

Lengths

```text
flower   -> 6
flow     -> 4
flowing  -> 7
```

The common prefix can never be longer than

```text
flow
```

because one string already ends there.

Therefore,

find the shortest string length first.

---

# Best Approach - Vertical Scanning

## Intuition

Instead of comparing strings,

compare **characters at the same index**.

Algorithm

```
Find shortest string length

↓

For every character index

↓

Take character from first string

↓

Compare it with every other string

↓

Mismatch?

Return prefix till previous index

↓

No mismatch?

Continue
```

---

# Java Code

```java
class Solution {

    public String longestCommonPrefix(String[] strs) {

        if (strs == null || strs.length == 0)
            return "";

        int minLength = Integer.MAX_VALUE;

        for (String word : strs)
            minLength = Math.min(minLength, word.length());

        for (int i = 0; i < minLength; i++) {

            char current = strs[0].charAt(i);

            for (int j = 1; j < strs.length; j++) {

                if (strs[j].charAt(i) != current)
                    return strs[0].substring(0, i);
            }
        }

        return strs[0].substring(0, minLength);
    }
}
```

---

# Code Explanation

## Step 1

```java
if(strs == null || strs.length == 0)
    return "";
```

### Why?

If no strings are given,

there cannot be any common prefix.

---

## Step 2

```java
int minLength = Integer.MAX_VALUE;
```

### Why?

Find the length of the shortest string.

The answer can never be longer than the shortest string.

---

## Step 3

```java
for(int i = 0; i < minLength; i++)
```

### Why?

Check every character position.

Example

```
Index

0
1
2
3
```

---

## Step 4

```java
char current = strs[0].charAt(i);
```

### Why?

Use the first string as the reference.

Every other string must match this character.

---

## Step 5

```java
for(int j = 1; j < strs.length; j++)
```

### Why?

Compare the current character with every other string.

No need to compare the first string with itself.

---

## Step 6

```java
if(strs[j].charAt(i) != current)
```

### Why?

If one string differs,

the common prefix ends here.

---

## Step 7

```java
return strs[0].substring(0, i);
```

### Why?

Suppose mismatch occurs at

```text
Index = 2
```

Then

```text
0
1
```

were matching.

Return

```text
substring(0,2)
```

which gives

```text
fl
```

---

## Step 8

```java
return strs[0].substring(0, minLength);
```

### Why?

If no mismatch occurs,

the entire shortest string is the common prefix.

---

# Dry Run

```text
Input

flower
flow
flight
```

Minimum Length

```text
4
```

---

### i = 0

```text
f
f
f
```

Match ✔

---

### i = 1

```text
l
l
l
```

Match ✔

---

### i = 2

```text
o
o
i
```

Mismatch ❌

Return

```text
fl
```

---

# Time Complexity

Let

```
N = Number of Strings

M = Length of Shortest String
```

Outer loop

```
M
```

Inner loop

```
N
```

Overall

```
O(N × M)
```

---

# Space Complexity

```
O(1)
```

No extra data structure is used.

---

# Common Mistakes

### ❌ Comparing till first string length

Wrong.

The first string may be longer than the others.

Always compare till the **shortest string length**.

---

### ❌ Returning

```java
substring(0, i + 1)
```

Wrong.

Mismatch happened at `i`.

That character is **not** part of the common prefix.

---

### ❌ Forgetting empty array check

```text
[]
```

Should return

```text
""
```

---

### ❌ Starting inner loop from 0

```java
for(int j = 0; j < strs.length; j++)
```

Unnecessary.

The first string always matches itself.

Start from

```java
j = 1;
```

---

# Interview Tips

### Tip 1

Whenever you hear

> **Longest Common Prefix**

Think

> **Compare characters vertically (column by column).**

---

### Tip 2

Always mention

> "The common prefix cannot be longer than the shortest string."

Interviewers like this observation.

---

### Tip 3

As soon as one mismatch occurs,

stop immediately.

Checking further characters is useless because a prefix must be continuous from the beginning.

---

### Tip 4

Don't jump into code.

Explain your thought process first.

Example:

> "I'll compare one character position at a time across all strings. The moment I find a mismatch, I know no longer prefix can exist, so I return the prefix built so far."

---

# Alternative Approaches

| Approach | Time | Interview Recommendation |
|----------|------|--------------------------|
| Horizontal Scanning | O(N × M) | ⭐⭐⭐⭐ |
| **Vertical Scanning** | **O(N × M)** | ⭐⭐⭐⭐⭐ (Best) |
| Divide & Conquer | O(S) | ⭐⭐⭐ |
| Binary Search on Prefix Length | O(N × M × log M) | ⭐⭐⭐⭐ |
| Trie | O(S) | ⭐⭐ |

---

# Pattern Learned

This problem teaches:

- String Traversal
- Vertical Scanning
- Early Stopping
- Character Comparison

---

# Similar Questions

- 28. Find the Index of the First Occurrence in a String
- 58. Length of Last Word
- 125. Valid Palindrome
- 392. Is Subsequence
- 1768. Merge Strings Alternately

---

# Key Takeaways

- Prefix always starts from index `0`.
- Compare characters **column by column**, not string by string.
- Stop immediately on the first mismatch.
- The answer can never be longer than the shortest string.
- Vertical Scanning is the cleanest and most interview-friendly solution.
