# File Name

```text
217_Contains_Duplicate.md
```

---

# 217. Contains Duplicate

**Difficulty:** Easy

**Pattern:** HashSet | Hashing

---

# Problem Statement

Given an integer array `nums`, return **true** if any value appears at least twice in the array, otherwise return **false**.

---

## Example

```text
Input:
[1,2,3,1]

Output:
true
```

```text
Input:
[1,2,3,4]

Output:
false
```

---

# Key Observation

For every element, we only need to answer one question:

> **"Have I seen this element before?"**

We **do not** need:

- Index
- Frequency
- Position

We only need **membership (Yes/No)**.

This makes **HashSet** the best choice.

---

# Approach 1: Brute Force

## Idea

Compare every element with every other element.

### Java

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {

        for (int i = 0; i < nums.length; i++) {

            for (int j = i + 1; j < nums.length; j++) {

                if (nums[i] == nums[j])
                    return true;
            }
        }

        return false;
    }
}
```

### Complexity

- **Time:** `O(n²)`
- **Space:** `O(1)`

---

# Approach 2: Sorting

## Idea

Sort the array.

After sorting, duplicate elements become adjacent.

Example

```text
Before:
4 1 3 2 1

After Sorting:
1 1 2 3 4
```

Compare adjacent elements.

### Java

```java
import java.util.Arrays;

class Solution {

    public boolean containsDuplicate(int[] nums) {

        Arrays.sort(nums);

        for (int i = 1; i < nums.length; i++) {

            if (nums[i] == nums[i - 1])
                return true;
        }

        return false;
    }
}
```

### Complexity

- **Time:** `O(n log n)`
- **Space:** `O(log n)` *(Java `Arrays.sort()` for primitive arrays)*

---

# Approach 3: HashSet (Best)

## Idea

Maintain a HashSet of all elements seen so far.

For every element:

- If already present → return `true`
- Otherwise add it to the set.

### Java

```java
import java.util.HashSet;

class Solution {

    public boolean containsDuplicate(int[] nums) {

        HashSet<Integer> set = new HashSet<>();

        for (int num : nums) {

            if (set.contains(num))
                return true;

            set.add(num);
        }

        return false;
    }
}
```

### Complexity

- **Time:** `O(n)` *(Average Case)*
- **Space:** `O(n)`

---

# Which Approach Should I Use?

| Approach | Time | Space | Recommended |
|----------|------|-------|-------------|
| Brute Force | `O(n²)` | `O(1)` | ❌ No |
| Sorting | `O(n log n)` | `O(log n)` | ⭐ Good |
| HashSet | `O(n)` | `O(n)` | ⭐⭐⭐⭐⭐ Best |

---

# Why Not XOR?

XOR works only when duplicate elements cancel each other (e.g., **Single Number**).

In this problem, we need to know:

> **"Have I seen this element before?"**

XOR cannot remember previously seen elements, so it cannot solve this problem.

---

# Interview Tips

- If the question asks **"Have I seen this before?"** → Think **HashSet**.
- If the question asks **"How many times?"** → Think **HashMap**.
- Sorting is a good alternative when extra space should be minimized.
- Mention the trade-off:
  - **HashSet:** Faster (`O(n)`) but uses extra space.
  - **Sorting:** Less extra space but slower (`O(n log n)`).

---

# Pattern Learned

- HashSet
- Membership Checking
- Duplicate Detection

---

# Similar Questions

- 1. Two Sum
- 202. Happy Number
- 128. Longest Consecutive Sequence
- 349. Intersection of Two Arrays
- 36. Valid Sudoku

---

# Key Takeaways

- Ask yourself: **"Do I only need to know whether an element has been seen before?"**
- If **Yes**, use a **HashSet**.
- HashSet gives **average O(1)** lookup and insertion.
- This is one of the most common interview patterns.
