# 🔗 Reverse Linked List (LeetCode 206)

---

# Problem Statement

Given the `head` of a singly linked list, reverse the linked list and return the new head.

Example:

```
Input

1 → 2 → 3 → 4 → 5 → null
```

```
Output

5 → 4 → 3 → 2 → 1 → null
```

---

# Prerequisites

Before solving this problem, you should understand:

- Linked List Basics
- Nodes and References
- Pointer Manipulation
- Iteration
- Recursion (for recursive approach)

---

# Understanding a Linked List

Unlike arrays, linked list elements are **not stored continuously in memory**.

Each node contains:

- Data
- Pointer to the next node

Example

```
+------+      +------+      +------+      +------+
| 10 | o----->| 20 | o----->| 30 | o----->| 40 | X |
+------+      +------+      +------+      +------+
```

Java Definition

```java
class ListNode {

    int val;
    ListNode next;

    ListNode(int val) {
        this.val = val;
    }
}
```

---

# What Does "Reverse" Mean?

Original

```
1 → 2 → 3 → 4 → null
```

Reversed

```
4 → 3 → 2 → 1 → null
```

Notice:

- The **values do not change**.
- Only the **next pointers** are modified.

This is the key observation.

---

# Important Concept

Changing

```java
head = head.next;
```

does **not** delete the previous node.

It only moves your reference.

Example

Initially

```
head

↓

1 → 2 → 3
```

After

```java
head = head.next;
```

```
head

↓

2 → 3
```

Node `1` still exists.

You simply lost the reference to it.

---

# Golden Rule of Linked Lists

> **Never change a pointer until you have saved where it was pointing.**

Always save

```java
next = current.next;
```

before modifying

```java
current.next
```

Otherwise, the remaining part of the list becomes unreachable.

---

# Approach 1 : Using Extra Space (Brute Force)

## Idea

Copy all node values into an array or list.

Example

```
1 → 2 → 3 → 4
```

Store

```
[1,2,3,4]
```

Write values back in reverse order

```
4 → 3 → 2 → 1
```

---

## Algorithm

1. Traverse linked list.
2. Store values.
3. Traverse again.
4. Replace values from end of array.

---

## Time Complexity

```
O(n)
```

---

## Space Complexity

```
O(n)
```

---

## Drawback

- Extra memory required.
- Does **not** test pointer manipulation.
- Usually not preferred in interviews.

---

# Approach 2 : Iterative (Three Pointers) ⭐ Recommended

This is the standard interview solution.

---

## Intuition

Instead of reversing values,

reverse one pointer at a time.

Original

```
1 → 2 → 3 → 4 → null
```

Goal

```
1 ← 2 ← 3 ← 4
```

---

## Three Pointers

We maintain

```
prev
curr
next
```

Each pointer has a responsibility.

### prev

Already reversed part.

### curr

Current node being processed.

### next

Stores the remaining list before links are changed.

---

## Initial State

```
prev = null

curr = head
```

```
prev

null

curr

↓

1 → 2 → 3 → 4
```

---

## Step 1

Save next node

```java
next = curr.next;
```

```
next

↓

2
```

---

## Step 2

Reverse current pointer

```java
curr.next = prev;
```

```
1 → null
```

---

## Step 3

Move prev

```java
prev = curr;
```

```
prev

↓

1 → null
```

---

## Step 4

Move curr

```java
curr = next;
```

```
curr

↓

2 → 3 → 4
```

Repeat until

```
curr == null
```

Return

```
prev
```

because it becomes the new head.

---

## Dry Run

Initial

```
prev = null

curr = 1
```

---

Iteration 1

```
next = 2

1.next = null

prev = 1

curr = 2
```

```
1 → null

2 → 3 → 4
```

---

Iteration 2

```
next = 3

2.next = 1

prev = 2

curr = 3
```

```
2 → 1

3 → 4
```

---

Iteration 3

```
3.next = 2
```

```
3 → 2 → 1
```

---

Iteration 4

```
4.next = 3
```

```
4 → 3 → 2 → 1
```

Done.

---

## Java Code

```java
class Solution {

    public ListNode reverseList(ListNode head) {

        ListNode prev = null;
        ListNode curr = head;

        while (curr != null) {

            ListNode next = curr.next;

            curr.next = prev;

            prev = curr;

            curr = next;
        }

        return prev;
    }
}
```

---

## Code Explanation

### Initialize

```java
ListNode prev = null;
ListNode curr = head;
```

Initially, no nodes are reversed.

---

### Save Next Node

```java
ListNode next = curr.next;
```

Save the remaining list before changing links.

---

### Reverse Link

```java
curr.next = prev;
```

Reverse the direction of the pointer.

---

### Move prev

```java
prev = curr;
```

Current node becomes part of the reversed list.

---

### Move curr

```java
curr = next;
```

Continue with the remaining list.

---

### Return

```java
return prev;
```

`prev` points to the new head.

---

## Time Complexity

```
O(n)
```

Each node is visited once.

---

## Space Complexity

```
O(1)
```

Only three pointers are used.

---

# Approach 3 : Recursive

## Intuition

Reverse the smaller linked list first.

Example

```
1 → 2 → 3 → 4
```

Ask recursion to reverse

```
2 → 3 → 4
```

Suppose recursion returns

```
4 → 3 → 2
```

Now simply attach

```
1
```

at the end.

---

## Java Code

```java
class Solution {

    public ListNode reverseList(ListNode head) {

        if (head == null || head.next == null)
            return head;

        ListNode newHead = reverseList(head.next);

        head.next.next = head;

        head.next = null;

        return newHead;
    }
}
```

---

## Time Complexity

```
O(n)
```

---

## Space Complexity

```
O(n)
```

Recursive call stack.

---

## Note

The line

```java
head.next.next = head;
```

is the most important part of the recursive solution.

Understanding **why** it works is essential before memorizing the code.

---

# Approach 4 : Using Stack

## Idea

Push all nodes into a stack.

Example

```
1 → 2 → 3 → 4
```

Stack

```
Top

4
3
2
1
```

Pop nodes one by one and reconnect them.

---

## Time Complexity

```
O(n)
```

---

## Space Complexity

```
O(n)
```

---

## Drawback

Uses extra memory.

Rarely preferred when an O(1) space solution is expected.

---

# Comparison of Approaches

| Approach | Time | Space | Preferred in Interviews |
|----------|------|--------|-------------------------|
| Extra Array/List | O(n) | O(n) | ⭐ |
| Stack | O(n) | O(n) | ⭐⭐ |
| Iterative (3 Pointers) | O(n) | O(1) | ⭐⭐⭐⭐⭐ |
| Recursive | O(n) | O(n) | ⭐⭐⭐⭐ |

---

# Common Mistakes

### Forgetting to save next

❌

```java
curr.next = prev;
curr = curr.next;
```

The remaining list is lost.

---

### Returning head

At the end,

`head` is no longer the first node.

Always return

```java
prev
```

---

### Forgetting

```java
head.next = null;
```

in the recursive approach.

This creates a cycle.

---

# Pattern Learned

This problem teaches one of the most important linked list patterns.

```
Save Next

↓

Reverse Pointer

↓

Move Previous

↓

Move Current
```

Many linked list interview problems are based on this exact sequence.

---

# Similar Problems to Practice

## Easy

1. Middle of the Linked List (LeetCode 876)
2. Linked List Cycle (LeetCode 141)
3. Remove Linked List Elements (LeetCode 203)
4. Palindrome Linked List (LeetCode 234)
5. Delete Node in a Linked List (LeetCode 237)

---

## Medium

6. Reverse Linked List II (LeetCode 92)
7. Swap Nodes in Pairs (LeetCode 24)
8. Remove Nth Node From End of List (LeetCode 19)
9. Reorder List (LeetCode 143)
10. Rotate List (LeetCode 61)

---

## Advanced

11. Reverse Nodes in k-Group (LeetCode 25)
12. Copy List with Random Pointer (LeetCode 138)
13. Merge k Sorted Lists (LeetCode 23)
14. Sort List (LeetCode 148)

---

# Interview Tips

Whenever you see a linked list question, ask yourself:

- Am I changing **values** or **pointers**?
- Will I lose the remaining list after changing a pointer?
- Should I save `next` first?
- Can this be solved iteratively and recursively?

Thinking this way helps you identify the correct pattern quickly.

---

# Key Takeaways

- Reversing a linked list means reversing **links**, not values.
- Always save `next` before modifying `current.next`.
- The iterative three-pointer solution is the most common interview solution.
- The recursive solution is elegant but uses additional stack space.
- Mastering this problem builds the foundation for many advanced linked list problems like reversing a sublist, reversing in groups, and reordering lists.
