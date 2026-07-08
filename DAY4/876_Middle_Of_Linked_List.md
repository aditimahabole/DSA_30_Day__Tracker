
# Java Code (Fast & Slow Pointer)

```java
class Solution {

    public ListNode middleNode(ListNode head) {

        ListNode slow = head;
        ListNode fast = head;

        while (fast != null && fast.next != null) {

            slow = slow.next;
            fast = fast.next.next;
        }

        return slow;
    }
}
```

---

# Line-by-Line Explanation

## Step 1

```java
ListNode slow = head;
ListNode fast = head;
```

### Why?

Both pointers start from the beginning of the list.

* `slow` moves **1 node** at a time.
* `fast` moves **2 nodes** at a time.

---

## Step 2

```java
while (fast != null && fast.next != null)
```

### Why this condition?

Before moving:

```java
fast = fast.next.next;
```

we must ensure:

* `fast` is not `null`.
* `fast.next` is not `null`.

Otherwise, trying to access `fast.next.next` would cause a **NullPointerException**.

---

## Step 3

```java
slow = slow.next;
```

### Why?

Move the slow pointer **one step**.

---

## Step 4

```java
fast = fast.next.next;
```

### Why?

Move the fast pointer **two steps**.

This creates the speed difference that allows the slow pointer to end up in the middle.

---

## Step 5

```java
return slow;
```

### Why?

When the loop ends:

* `fast` has reached the end of the list.
* `slow` has reached the middle.

For an even-length list, this naturally returns the **second middle node**, exactly as the problem requires.

---

# Dry Run

Input:

```text
1 → 2 → 3 → 4 → 5
```

| Iteration | Slow | Fast |
| --------: | :--: | :--: |
|     Start |   1  |   1  |
|         1 |   2  |   3  |
|         2 |   3  |   5  |
|      Stop |   3  | null |

Return:

```text
3
```

---

Input:

```text
1 → 2 → 3 → 4 → 5 → 6
```

| Iteration | Slow | Fast |
| --------: | :--: | :--: |
|     Start |   1  |   1  |
|         1 |   2  |   3  |
|         2 |   3  |   5  |
|         3 |   4  | null |

Return:

```text
4
```

Notice that we automatically get the **second middle node**.

---

# Time Complexity

```
O(n)
```

Although `fast` moves two steps at a time, every node is processed at most once overall, so the time complexity remains **O(n)**.

---

# Space Complexity

```
O(1)
```

Only two pointers are used.

---

# Common Mistakes

### ❌ Mistake 1

```java
while (fast != null)
```

Wrong.

If `fast.next` is `null`, then:

```java
fast.next.next
```

throws a **NullPointerException**.

Always write:

```java
while (fast != null && fast.next != null)
```

---

### ❌ Mistake 2

Moving `fast` only one step.

```java
fast = fast.next;
```

Now both pointers move at the same speed, so `slow` will simply reach the end instead of the middle.

---

### ❌ Mistake 3

Returning `fast`.

The question asks for the **middle node**.

The middle is tracked by `slow`, not `fast`.

---

# Interview Tip

If an interviewer asks:

> **"Why does this algorithm work?"**

A strong answer is:

> "The slow pointer moves one node at a time while the fast pointer moves two nodes. When the fast pointer reaches the end of the list, it has traveled twice the distance of the slow pointer. Therefore, the slow pointer has covered exactly half the list and is positioned at the middle."

---

## 🎯 One More Thing (Very Important)

This exact template:

```java
ListNode slow = head;
ListNode fast = head;

while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;
}
```

is one of the **most reusable Linked List templates**.

You'll use almost the same code in:

* ✅ 141. Linked List Cycle
* ✅ 142. Linked List Cycle II
* ✅ 234. Palindrome Linked List
* ✅ 143. Reorder List

The only thing that changes is **what you do with `slow` and `fast` after or during the loop**. This is why mastering this pattern is much more valuable than memorizing the solution to a single problem.
