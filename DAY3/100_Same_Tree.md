
# 100. Same Tree

**Difficulty:** Easy

**Pattern:** Tree | DFS | Recursion | Tree Comparison

---

# Problem Statement

Given the roots of two binary trees `p` and `q`, return **true** if they are identical.

Two binary trees are considered the same if:

- They have the **same structure**
- They contain the **same node values**

Otherwise, return **false**.

---

# Understanding the Problem

Many beginners misunderstand this question.

The question is **NOT** asking:

> Do both trees contain the same values?

Instead it asks:

> Are both trees structurally identical **and** do corresponding nodes have the same values?

Both conditions are mandatory.

---

## Example 1

```text
Tree 1

      1
     / \
    2   3
```

```text
Tree 2

      1
     / \
    2   3
```

Structure ✔

Values ✔

Answer

```text
true
```

---

## Example 2

```text
Tree 1

      1
     /
    2
```

```text
Tree 2

      1
       \
        2
```

Values

```text
1
2
```

are same.

Structure is different.

Answer

```text
false
```

---

## Example 3

```text
Tree 1

      1
     / \
    2   1
```

```text
Tree 2

      1
     / \
    1   2
```

Structure ✔

Values ✖

Answer

```text
false
```

---

# How to Think?

Don't think about the entire tree.

Think only about the **current pair of nodes**.

Ask yourself:

> Are these two nodes identical?

If yes,

then ask the same question for

- Left children
- Right children

Notice something?

The problem becomes **smaller versions of itself**.

This is the biggest clue that recursion is the right approach.

---

# The Recursive Thinking

For two trees to be identical,

three conditions must be true.

```text
Current nodes are equal

AND

Left subtrees are equal

AND

Right subtrees are equal
```

Mathematically,

```text
SameTree(p, q) =

(p.val == q.val)

AND

SameTree(p.left, q.left)

AND

SameTree(p.right, q.right)
```

This is the entire recursive idea.

---

# Base Cases

Every recursive problem starts by asking:

> When should recursion stop?

---

## Base Case 1

Both nodes are null.

```text
p = null

q = null
```

Both trees ended together.

Return

```text
true
```

---

## Base Case 2

One node is null.

```text
p = null

q = 5
```

Structures differ.

Return

```text
false
```

---

## Base Case 3

Values are different.

```text
5

6
```

Return

```text
false
```

---

# Algorithm

```
Compare both nodes

↓

Both null ?

Return true

↓

One null ?

Return false

↓

Values different ?

Return false

↓

Compare left subtree recursively

AND

Compare right subtree recursively
```

---

# Java Code

```java
class Solution {

    public boolean isSameTree(TreeNode p, TreeNode q) {

        // Base Case 1
        if (p == null && q == null)
            return true;

        // Base Case 2
        if (p == null || q == null)
            return false;

        // Base Case 3
        if (p.val != q.val)
            return false;

        // Recursive Case
        return isSameTree(p.left, q.left)
                &&
               isSameTree(p.right, q.right);
    }
}
```

---

# Code Explanation

## Step 1

```java
if(p == null && q == null)
    return true;
```

### Why?

Both trees ended at the same position.

They are identical till here.

---

## Step 2

```java
if(p == null || q == null)
    return false;
```

### Why?

One tree has a node.

The other doesn't.

Structures are different.

---

## Step 3

```java
if(p.val != q.val)
    return false;
```

### Why?

Current node values differ.

Trees cannot be identical.

---

## Step 4

```java
return isSameTree(p.left,q.left)
       &&
       isSameTree(p.right,q.right);
```

### Why?

If current nodes match,

both left and right subtrees must also match.

Notice the use of

```text
AND (&&)
```

Both recursive calls must return true.

---

# Dry Run

Trees

```text
      1                    1
     / \                  / \
    2   3                2   3
```

Call

```text
isSameTree(1,1)
```

Values same ✔

↓

Compare left

```text
isSameTree(2,2)
```

↓

Compare

```text
null

null
```

Returns

```text
true
```

↓

Compare right

```text
isSameTree(3,3)
```

↓

Returns

```text
true
```

↓

Final

```text
true && true

↓

true
```

---

# Important Dry Run

```text
Tree 1

      1
     /
    2
```

```text
Tree 2

      1
       \
        2
```

Call

```text
isSameTree(1,1)
```

↓

Left comparison

```text
2

null
```

Base Case 2

↓

Return

```text
false
```

Whole answer becomes

```text
false
```

---

# Why do we use && ?

Suppose

```java
return left && right;
```

If

```text
left = false
```

Java immediately returns

```text
false
```

It doesn't even check

```text
right
```

This is called

> **Short Circuit Evaluation**

It avoids unnecessary recursive calls.

---

# Time Complexity

Let

```
N = Number of Nodes
```

Every node is visited once.

```
O(N)
```

---

# Space Complexity

Recursion stack stores function calls.

```
O(H)
```

where

```
H = Height of Tree
```

Balanced Tree

```
O(log N)
```

Worst Case (Skew Tree)

```
O(N)
```

---

# Common Mistakes

## Mistake 1

Checking value before null.

```java
if(p.val != q.val)
```

This causes

```text
NullPointerException
```

Always check null first.

---

## Mistake 2

Using ||

```java
return left || right;
```

Wrong.

Both subtrees must be identical.

Correct operator is

```java
&&
```

---

## Mistake 3

Comparing only values.

Example

```text
      1             1
     /               \
    2                 2
```

Values same.

Structure different.

Answer is

```text
false
```

---

# Interview Thinking

Instead of memorizing,

ask these three questions.

## Question 1

What should my function return?

Answer

> Whether the two trees rooted at `p` and `q` are identical.

---

## Question 2

When should recursion stop?

- Both null
- One null
- Values different

---

## Question 3

How can I reduce the problem?

If current nodes match,

compare

- Left subtree
- Right subtree

The same problem repeats.

Hence recursion.

---

# Traversal Used

This solution behaves like

## Preorder Traversal

Why?

Because we process the current node first.

```java
if(p.val != q.val)
```

Only after checking the root,

we move to

- Left subtree
- Right subtree

Order

```text
Root

↓

Left

↓

Right
```

Although we are not printing nodes,

the recursion follows **Preorder-style traversal**.

---

# Compare with Maximum Depth

Maximum Depth

```java
return 1 + Math.max(left,right);
```

Needs answers from children first.

Traversal Style

```text
Left

↓

Right

↓

Root
```

Which behaves like

```text
Postorder
```

---

# Interview Tips

## Tip 1

Don't think about the entire tree.

Always think

> "Can I solve this for the current node?"

---

## Tip 2

Whenever you see

```text
Current Answer

depends on

Left Answer

and

Right Answer
```

Think

> **Recursion**

---

## Tip 3

Tree recursion has three important questions.

1. What should my function return?
2. What are the base cases?
3. How do I reduce the problem?

If you answer these,

you can derive the solution yourself.

---

## Tip 4

For Tree problems,

identify when you are using the current node.

- Before recursion → Preorder-style
- Between left & right → Inorder-style
- After recursion → Postorder-style

This helps identify the traversal naturally.

---

# Pattern Learned

This problem teaches

- Tree Comparison
- DFS
- Recursion
- Base Cases
- Recursive Thinking
- Preorder-style Recursion

---

# Similar Questions

- 101. Symmetric Tree ⭐⭐⭐⭐⭐
- 572. Subtree of Another Tree
- 226. Invert Binary Tree
- 617. Merge Two Binary Trees
- 110. Balanced Binary Tree
- 104. Maximum Depth of Binary Tree

---

# Key Takeaways

- Same Tree means **same structure + same values**.
- Always handle **null** cases before accessing node values.
- The recursive relation is:

```text
Current nodes equal

AND

Left subtrees equal

AND

Right subtrees equal
```

- The solution naturally uses recursion because the problem repeats on smaller subtrees.
- Think about **one pair of nodes**, not the entire tree.
