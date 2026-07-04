# 🌳 Maximum Depth of Binary Tree (LeetCode 104)

---

# Problem Statement

Given the `root` of a binary tree, return its **maximum depth**.

**Maximum Depth** = Number of nodes along the longest path from the root node down to the farthest leaf node.

Example:

```
        1
      /   \
     2     3
    / \
   4   5
```

Output:

```
3
```

---

# Core Observation

Every node asks only one question:

> **"What is the height of my left subtree and right subtree?"**

Once it knows both heights, it can calculate its own height.

Formula:

```
height(node) = 1 + max(height(left), height(right))
```

The recursion stops when we reach a `null` node.

---

# Approach 1 : DFS (Recursion) ⭐ Recommended

## Intuition

A binary tree is made of **smaller binary trees**.

Instead of calculating the entire tree at once,

calculate:

- Height of left subtree
- Height of right subtree

Take the maximum and add one for the current node.

---

## Recursive Formula

```
height(node) =

0                        if node == null

1 + max(
        height(node.left),
        height(node.right)
      )
```

---

## Dry Run

Tree

```
        1
      /   \
     2     3
    / \
   4   5
```

```
height(4)

= 1 + max(0,0)

=1
```

```
height(5)

=1
```

```
height(2)

=1 + max(1,1)

=2
```

```
height(3)

=1
```

```
height(1)

=1 + max(2,1)

=3
```

Answer = **3**

---

## Java Code

```java
class Solution {

    public int maxDepth(TreeNode root) {

        if (root == null)
            return 0;

        int leftHeight = maxDepth(root.left);
        int rightHeight = maxDepth(root.right);

        return Math.max(leftHeight, rightHeight) + 1;
    }
}
```

---

## Code Explanation

### Base Case

```java
if(root == null)
    return 0;
```

An empty tree has height **0**.

---

### Calculate Left Height

```java
int leftHeight = maxDepth(root.left);
```

Recursively calculate the height of the left subtree.

---

### Calculate Right Height

```java
int rightHeight = maxDepth(root.right);
```

Recursively calculate the height of the right subtree.

---

### Return Height

```java
return Math.max(leftHeight, rightHeight) + 1;
```

Choose the taller subtree and include the current node.

---

## Time Complexity

```
O(n)
```

Every node is visited exactly once.

---

## Space Complexity

Balanced Tree

```
O(log n)
```

Worst Case (Skewed Tree)

```
O(n)
```

Space is due to the recursion call stack.

---

# Understanding the Recursion

Suppose the tree is

```
        1
      /   \
     2     3
    / \
   4   5
```

Execution order is **not**

```
1
├──2
└──3
```

Instead it is

```
1

↓

2

↓

4

↓

null

↑

4

↓

null

↑

2

↓

5

↓

null

↑

5

↑

2

↑

1

↓

3

↓

null

↑

3

↑

1
```

Important Observation:

- Java completely finishes the **left subtree**
- Then processes the **right subtree**
- Finally returns the answer for the current node

---

# Mental Model

Imagine every node asking its children:

```
"What is your height?"
```

Leaf node replies

```
1
```

Its parent calculates

```
1 + max(left,right)
```

and returns that value to its parent.

---

# Approach 2 : BFS (Level Order Traversal)

## Intuition

Instead of going deep into one branch,

visit the tree **level by level**.

```
Level 1

        1
```

```
Level 2

     2      3
```

```
Level 3

   4    5
```

The answer is simply the number of levels.

---

## Why Queue?

A Queue follows

```
FIFO

First In
First Out
```

Nodes are processed in the same order they are discovered.

---

## Queue Operations

```
offer()
```

Insert into queue.

```
poll()
```

Remove from queue.

```
peek()
```

View front element.

---

## Algorithm

1. Put root into queue.
2. While queue is not empty
    - Count nodes currently present in queue.
    - Process exactly those nodes.
    - Insert their children.
    - Increase depth by one.
3. Return depth.

---

## Dry Run

Tree

```
        1
      /   \
     2     3
    / \
   4   5
```

Initially

```
Queue

1
```

Depth = 0

---

After processing Level 1

```
Queue

2 3
```

Depth = 1

---

After processing Level 2

```
Queue

4 5
```

Depth = 2

---

After processing Level 3

```
Queue

Empty
```

Depth = 3

Answer = **3**

---

## Why Store Queue Size?

Suppose queue contains

```
2 3
```

Queue size = 2

Process exactly two nodes.

Even if new nodes are added

```
4
5
```

they belong to the **next level**.

Without storing queue size, levels would mix together.

This is one of the most important concepts in BFS.

---

## Java Code

```java
class Solution {

    public int maxDepth(TreeNode root) {

        if (root == null)
            return 0;

        Queue<TreeNode> queue = new LinkedList<>();

        queue.offer(root);

        int depth = 0;

        while (!queue.isEmpty()) {

            int levelSize = queue.size();

            for (int i = 0; i < levelSize; i++) {

                TreeNode current = queue.poll();

                if (current.left != null)
                    queue.offer(current.left);

                if (current.right != null)
                    queue.offer(current.right);
            }

            depth++;
        }

        return depth;
    }
}
```

---

## Time Complexity

```
O(n)
```

Every node enters and leaves the queue once.

---

## Space Complexity

Worst Case

```
O(n)
```

The queue may contain an entire level of the tree.

---

# DFS vs BFS Comparison

| Feature | DFS (Recursion) | BFS (Queue) |
|----------|-----------------|-------------|
| Data Structure | Call Stack | Queue |
| Traversal | Depth First | Level Order |
| Code Length | Short | Slightly Longer |
| Space (Balanced Tree) | O(log n) | O(n) |
| Space (Worst Case) | O(n) | O(n) |
| Best Choice | Tree property depends on children | Level-based questions |

---

# Interview Tips

Think **DFS** when:

- Height
- Diameter
- Balanced Tree
- Path Sum
- Lowest Common Ancestor
- Tree comparison

Think **BFS** when:

- Level Order Traversal
- Zigzag Traversal
- Right Side View
- Average of Levels
- Minimum Depth
- Nearest Node
- Shortest Path (Unweighted Graph)

---

# Common Mistakes

### DFS

❌ Forgetting the base case.

❌ Returning `Math.min()` instead of `Math.max()`.

❌ Confusing height with number of edges.

---

### BFS

❌ Forgetting to store `queue.size()` before the loop.

❌ Incrementing depth for every node instead of every level.

❌ Adding `null` nodes into the queue.

---

# Pattern Learned

This problem teaches one of the most important tree recursion patterns.

```
Answer(node)

=

Combine(

Answer(left),

Answer(right)

)
```

Many binary tree interview problems follow exactly this pattern.

---

# Similar Problems to Practice

## Easy

1. Same Tree (LeetCode 100)
2. Invert Binary Tree (LeetCode 226)
3. Minimum Depth of Binary Tree (LeetCode 111)
4. Binary Tree Level Order Traversal (LeetCode 102)
5. Symmetric Tree (LeetCode 101)

---

## Medium

6. Balanced Binary Tree (LeetCode 110)
7. Diameter of Binary Tree (LeetCode 543)
8. Path Sum (LeetCode 112)
9. Binary Tree Right Side View (LeetCode 199)
10. Average of Levels in Binary Tree (LeetCode 637)

---

## Advanced

11. Binary Tree Maximum Path Sum (LeetCode 124)
12. Lowest Common Ancestor of a Binary Tree (LeetCode 236)
13. Serialize and Deserialize Binary Tree (LeetCode 297)
14. Binary Tree Zigzag Level Order Traversal (LeetCode 103)

---

# Key Takeaways

- Binary trees are naturally recursive because every subtree is itself a binary tree.
- Always identify the **base case** before writing recursion.
- Ask yourself: **"What should my recursive function return?"**
- For BFS, the **queue size represents one complete level**.
- DFS is generally preferred for recursive tree properties, while BFS is preferred for level-order processing.
- Mastering this problem builds the foundation for most binary tree interview questions.
