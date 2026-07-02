# LeetCode 1 - Two Sum

## Problem

Given an integer array `nums` and an integer `target`, return the indices of the two numbers such that they add up to `target`.

You may assume:

- Exactly one solution exists.
- The same element cannot be used twice.
- Return the answer in any order.

---

# Examples

### Example 1

```text
Input:
nums = [2,7,11,15]
target = 9

Output:
[0,1]
```

Explanation:

```
nums[0] + nums[1] = 2 + 7 = 9
```

---

### Example 2

```text
Input:
nums = [3,2,4]
target = 6

Output:
[1,2]
```

---

### Example 3

```text
Input:
nums = [3,3]
target = 6

Output:
[0,1]
```

---

# Approach 1: Brute Force

## Intuition

Check every possible pair.

If any pair sums to the target, return their indices.

## Algorithm

```
for each i
    for each j > i
        if nums[i] + nums[j] == target
            return [i, j]
```

## Java Solution

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {

        for(int i = 0; i < nums.length; i++) {

            for(int j = i + 1; j < nums.length; j++) {

                if(nums[i] + nums[j] == target) {
                    return new int[]{i, j};
                }

            }

        }

        return new int[]{};
    }
}
```

### Complexity

**Time:** O(n²)

**Space:** O(1)

---

# Approach 2: Sorting + Two Pointers

## Idea

Sort the array.

Use two pointers:

- Small sum → Move left pointer.
- Large sum → Move right pointer.

### Why this isn't the best solution?

Sorting changes the order of elements.

Since the problem asks for **original indices**, we would have to store index information separately.

### Complexity

Time: O(n log n)

Space: O(n)

---

# Approach 3: One-Pass HashMap (Optimal)

## Observation

Instead of searching for another number,

calculate the number that is required.

```
current + need = target

need = target - current
```

Now ask:

**Have I already seen this "need" value?**

If yes,

return the stored index and current index.

Otherwise,

store the current value in the HashMap.

---

# Dry Run

```
nums = [2,7,11,15]
target = 9
```

Initially

```
HashMap = {}
```

### Iteration 1

Current = 2

Need = 9 - 2 = 7

```
Map contains 7?

No

Store

2 → 0
```

Map

```
{
2 → 0
}
```

---

### Iteration 2

Current = 7

Need = 9 - 7 = 2

Map

```
{
2 → 0
}
```

Contains 2?

Yes

Return

```
[0,1]
```

Done.

---

# Java Solution

```java
import java.util.HashMap;

class Solution {

    public int[] twoSum(int[] nums, int target) {

        HashMap<Integer, Integer> map = new HashMap<>();

        for(int i = 0; i < nums.length; i++) {

            int need = target - nums[i];

            if(map.containsKey(need)) {
                return new int[]{map.get(need), i};
            }

            map.put(nums[i], i);

        }

        return new int[]{};
    }
}
```

---

# Why do we check before inserting?

Consider

```
nums = [3,3]
target = 6
```

First element

```
Need = 3

Not found

Store

3 → 0
```

Second element

```
Need = 3

Found

Return

[0,1]
```

Checking before inserting ensures we never use the same element twice.

---

# Complexity Analysis

## Brute Force

Time: O(n²)

Space: O(1)

---

## HashMap Solution

Time: O(n)

Space: O(n)

---

# Key Insight

Instead of searching for two numbers,

convert the problem into searching for **one number**.

```
need = target - current
```

HashMap lets us check whether that number has already appeared in **O(1)** time.

---

# Pattern Recognition

Whenever you see:

- Pair Sum
- Target Sum
- Complement
- Previous Occurrence
- Find Difference

Think:

```
HashMap
```

---

# Similar Problems

- Contains Duplicate
- Subarray Sum Equals K
- Two Sum II
- 3Sum
- 4Sum
- Longest Consecutive Sequence

---

# Interview Takeaways

✅ Brute Force is simple but inefficient.

✅ Sorting helps but loses original indices.

✅ HashMap trades space for speed.

✅ One-pass HashMap is the optimal solution.

**Golden Formula**

```
need = target - current
```

If `need` is already present in the HashMap,

you have found the answer.
