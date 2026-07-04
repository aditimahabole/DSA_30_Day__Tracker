# File Name

```text
035_Search_Insert_Position.md
```

---

# 🔍 LeetCode 35 - Search Insert Position

**Difficulty:** Easy

**Pattern:** Binary Search (Boundary Search)

---

# Problem Statement

Given a **sorted** array of **distinct integers** and a target value, return:

- The index if the target is found.
- If the target is not found, return the index where it should be inserted so that the array remains sorted.

---

## Example 1

```text
Input:
nums = [1,3,5,6]
target = 5

Output:
2
```

---

## Example 2

```text
Input:
nums = [1,3,5,6]
target = 2

Output:
1
```

Explanation

```
1 3 5 6

Insert 2 here

1 2 3 5 6
  ↑

Index = 1
```

---

## Example 3

```text
Input:
nums = [1,3,5,6]
target = 7

Output:
4
```

---

## Example 4

```text
Input:
nums = [1,3,5,6]
target = 0

Output:
0
```

---

# Prerequisites

Before solving this question, you should know:

- Binary Search Fundamentals
- Search Space
- Why Binary Search requires a sorted array

---

# Observation

Notice the wording carefully.

It **does not** say

> Return -1 if the element is absent.

Instead it says

> Return the position where the element should be inserted.

This small change creates an entirely new Binary Search pattern called **Boundary Search**.

---

# Brute Force Approach

## Intuition

Traverse the array from left to right.

- If target is found, return its index.
- If target is smaller than the current element, return the current index.
- If the loop finishes, insert at the end.

---

## Java Code

```java
class Solution {

    public int searchInsert(int[] nums, int target) {

        for(int i=0;i<nums.length;i++){

            if(nums[i] >= target)
                return i;
        }

        return nums.length;
    }
}
```

---

## Dry Run

```
nums = [1,3,5,6]

target = 4
```

```
1 < 4

Continue
```

```
3 < 4

Continue
```

```
5 >= 4

Return index 2
```

Correct.

---

## Time Complexity

```
O(n)
```

---

## Space Complexity

```
O(1)
```

---

# Can We Do Better?

Yes.

The array is sorted.

Whenever you see

```
Sorted Array
```

your first thought should be

> "Can I eliminate half of the search space?"

If yes,

Binary Search can be applied.

---

# Optimal Approach (Binary Search)

---

# Core Idea

Unlike LeetCode 704,

we are not only searching for the target.

We are also interested in

> "Where would the target be inserted if it doesn't exist?"

The beautiful observation is

> **When Binary Search finishes, `left` automatically points to the correct insertion position.**

This is the entire trick behind this question.

---

# Search Space

Initially

```
left = 0

right = nums.length - 1
```

Meaning

```
The answer must lie somewhere between left and right.
```

Initially,

```
nums = [1,3,5,6]

left = 0

right = 3
```

Search Space

```
[1 3 5 6]
```

---

# Java Code

```java
class Solution {

    public int searchInsert(int[] nums, int target) {

        int left = 0;
        int right = nums.length - 1;

        while(left <= right){

            int mid = left + (right - left) / 2;

            if(nums[mid] == target)
                return mid;

            else if(nums[mid] < target)
                left = mid + 1;

            else
                right = mid - 1;
        }

        return left;
    }
}
```

---

# Complete Dry Run

## Example

```
nums = [1,3,5,6]

target = 2
```

---

### Initial State

```
left = 0

right = 3
```

Search Space

```
1 3 5 6
```

---

### Iteration 1

Calculate

```
mid = 1
```

Current Element

```
nums[1] = 3
```

Compare

```
2 < 3
```

Can the answer be after index 1?

No.

Everything after index 1 is even larger.

So eliminate right half.

```
right = mid - 1

right = 0
```

Search Space becomes

```
1
```

---

### Iteration 2

```
left = 0

right = 0
```

Calculate

```
mid = 0
```

Current Element

```
nums[0] = 1
```

Compare

```
2 > 1
```

Can the answer be before index 0?

No.

Everything before it is smaller.

Eliminate left half.

```
left = mid + 1

left = 1
```

Now

```
left = 1

right = 0
```

Search Space becomes empty.

Loop ends.

Return

```
left

=

1
```

Correct answer.

---

# Why Do We Return left?

This is the most important concept.

Suppose

```
nums = [1,3,5,6]

target = 2
```

When Binary Search finishes

```
left = 1

right = 0
```

Notice

```
left

↓

1 3 5 6
```

The target belongs exactly where

```
left
```

is pointing.

---

Another example

```
nums = [1,3,5,6]

target = 7
```

Final values

```
left = 4

right = 3
```

```
1 3 5 6
        ↑
      left
```

Insert

```
7
```

at index

```
4
```

Correct.

---

Another example

```
target = 0
```

Final values

```
left = 0

right = -1
```

```
↑
1 3 5 6
```

Insert at

```
0
```

Correct.

---

# Why Not Return right?

Example

```
target = 2
```

Final values

```
left = 1

right = 0
```

Returning

```
right
```

would return

```
0
```

Wrong.

---

Example

```
target = 7
```

Final values

```
left = 4

right = 3
```

Returning

```
3
```

Wrong again.

Therefore,

```
left
```

always represents the insertion position.

---

# Line by Line Explanation

---

## Step 1

```java
int left = 0;
int right = nums.length - 1;
```

### Why?

Initially, every index can potentially contain the answer.

So our search space is

```
[left.....right]
```

---

## Step 2

```java
while(left <= right)
```

### Why?

Continue searching while there is at least one candidate left.

If

```
left == right
```

there is still one element to check.

If

```
left > right
```

the search space becomes empty.

---

## Step 3

```java
int mid = left + (right-left)/2;
```

### Why?

Find the middle safely.

Using

```java
(left+right)/2
```

can overflow for very large arrays.

This formula avoids overflow.

---

## Step 4

```java
if(nums[mid] == target)
    return mid;
```

### Why?

Target found.

No further searching required.

---

## Step 5

```java
else if(nums[mid] < target)
    left = mid + 1;
```

### Why?

Everything up to

```
mid
```

is smaller than target.

We already checked

```
mid
```

So it cannot be the answer.

Therefore the new search space starts from

```
mid + 1
```

NOT

```
mid
```

Using

```java
left = mid;
```

can cause an infinite loop.

---

## Step 6

```java
else
    right = mid - 1;
```

### Why?

Everything from

```
mid
```

to the end is larger.

Since

```
mid
```

has already been checked,

remove it too.

New search space ends at

```
mid - 1
```

---

## Step 7

```java
return left;
```

### Why?

Binary Search stopped because

```
left > right
```

At this moment,

```
left
```

points to the first valid position where the target can be inserted while maintaining sorted order.

---

# Time Complexity

```
O(log n)
```

Each iteration removes half of the search space.

---

# Space Complexity

```
O(1)
```

No extra memory is used.

---

# Common Mistakes

### ❌ Returning -1

Wrong.

The question asks for the insertion position.

---

### ❌ Returning right

Wrong.

The insertion position is always

```
left
```

---

### ❌ Using

```java
left = mid;
```

May cause an infinite loop.

Always write

```java
left = mid + 1;
```

---

### ❌ Using

```java
right = mid;
```

May also cause an infinite loop.

Always write

```java
right = mid - 1;
```

---

### ❌ Using

```java
while(left < right)
```

The last candidate may never be checked.

---

# Interview Tips

Whenever the interviewer asks:

- Search Insert Position
- Lower Bound
- First Position
- First Element Greater Than X
- First Valid Position

Think

> **Boundary Binary Search**

Instead of finding the exact value,

find the **first valid position**.

---

# Pattern Learned

This question teaches one of the most important Binary Search patterns.

```
Exact Search

↓

Boundary Search
```

Instead of asking

```
Where is the target?
```

we ask

```
Where should the target be?
```

That idea appears in many advanced Binary Search questions.

---

# Similar Problems

### Easy

- 704. Binary Search
- 278. First Bad Version

---

### Medium

- 34. Find First and Last Position of Element in Sorted Array
- 744. Find Smallest Letter Greater Than Target
- 852. Peak Index in a Mountain Array

---

### Advanced

- 153. Find Minimum in Rotated Sorted Array
- 33. Search in Rotated Sorted Array
- 875. Koko Eating Bananas
- 1011. Capacity To Ship Packages Within D Days

---

# Key Takeaways

- Binary Search is not only used to find elements.
- It can also find the correct boundary or insertion position.
- At the end of Binary Search, `left` points to the first valid insertion index.
- Always think in terms of **search space**, not just the middle element.
- This problem is the foundation for many advanced Binary Search interview questions.
