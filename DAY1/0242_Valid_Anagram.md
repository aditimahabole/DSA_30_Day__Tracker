# 242. Valid Anagram

**Difficulty:** Easy  
**Topic:** Arrays, String, HashMap, Frequency Count

---

# Problem

Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, otherwise return `false`.

An **Anagram** is a word or phrase formed by rearranging the letters of another word, using all the original letters exactly once.

---

# Examples

### Example 1

```text
Input:
s = "anagram"
t = "nagaram"

Output:
true
```

---

### Example 2

```text
Input:
s = "rat"
t = "car"

Output:
false
```

---

# Thought Process

The first thing to notice is:

> Two anagrams must have the **same length**.

So always start with:

```java
if(s.length() != t.length())
    return false;
```

Now the question becomes:

> Do both strings have the **same frequency of every character?**

---

# Approach 1: Sorting

## Intuition

If two strings are anagrams, sorting both strings will make them identical.

```
eat

↓

aet
```

```
tea

↓

aet
```

Compare both sorted strings.

---

## Java

```java
class Solution {

    public boolean isAnagram(String s, String t) {

        if(s.length()!=t.length())
            return false;

        char[] a=s.toCharArray();
        char[] b=t.toCharArray();

        Arrays.sort(a);
        Arrays.sort(b);

        return Arrays.equals(a,b);
    }
}
```

### Complexity

| Time | Space |
|------|-------|
| O(n log n) | O(n) |

---

# Approach 2: Two HashMaps

## Intuition

Store frequency of every character in both strings.

Example

```
anagram

a -> 3
n -> 1
g -> 1
r -> 1
m -> 1
```

```
nagaram

a -> 3
n -> 1
g -> 1
r -> 1
m -> 1
```

Now compare both maps.

---

## Java

```java
class Solution {

    public boolean isAnagram(String s, String t) {

        if(s.length()!=t.length())
            return false;

        HashMap<Character,Integer> map1=new HashMap<>();
        HashMap<Character,Integer> map2=new HashMap<>();

        for(char c:s.toCharArray())
            map1.put(c,map1.getOrDefault(c,0)+1);

        for(char c:t.toCharArray())
            map2.put(c,map2.getOrDefault(c,0)+1);

        return map1.equals(map2);
    }
}
```

### Complexity

| Time | Space |
|------|-------|
| O(n) | O(n) |

---

# Approach 3: One HashMap

## Intuition

Instead of maintaining two maps,

- Increase count while traversing first string.
- Decrease count while traversing second string.

At the end every frequency should become **0**.

---

## Java

```java
class Solution {

    public boolean isAnagram(String s, String t) {

        if(s.length()!=t.length())
            return false;

        HashMap<Character,Integer> map=new HashMap<>();

        for(char c:s.toCharArray())
            map.put(c,map.getOrDefault(c,0)+1);

        for(char c:t.toCharArray()){

            if(!map.containsKey(c))
                return false;

            map.put(c,map.get(c)-1);

            if(map.get(c)==0)
                map.remove(c);
        }

        return map.isEmpty();
    }
}
```

### Complexity

| Time | Space |
|------|-------|
| O(n) | O(n) |

---

# Approach 4: Frequency Array ⭐ (Optimal)

## Observation

The problem states:

```
s and t consist of lowercase English letters.
```

There are only **26** possible characters.

Instead of using a HashMap,

use an integer array of size **26**.

---

## Algorithm

```
Check lengths

↓

Create frequency array

↓

Increase count using first string

↓

Decrease count using second string

↓

Every value should become zero
```

---

## Dry Run

```
s = "anagram"

t = "nagaram"
```

After first string

```
a = 3
n = 1
g = 1
r = 1
m = 1
```

After second string

```
a = 0
n = 0
g = 0
r = 0
m = 0
```

Everything becomes **0**

Return **true**

---

# Java (Optimal)

```java
class Solution {

    public boolean isAnagram(String s, String t) {

        if(s.length()!=t.length())
            return false;

        int[] freq=new int[26];

        for(int i=0;i<s.length();i++){

            freq[s.charAt(i)-'a']++;

            freq[t.charAt(i)-'a']--;
        }

        for(int count:freq){

            if(count!=0)
                return false;
        }

        return true;
    }
}
```

---

# C++ (Optimal)

```cpp
class Solution {
public:

    bool isAnagram(string s, string t) {

        if(s.length()!=t.length())
            return false;

        vector<int> freq(26,0);

        for(int i=0;i<s.length();i++){

            freq[s[i]-'a']++;

            freq[t[i]-'a']--;
        }

        for(int count:freq){

            if(count!=0)
                return false;
        }

        return true;
    }
};
```

---

# Complexity Comparison

| Approach | Time | Space |
|----------|------|-------|
| Sorting | O(n log n) | O(n) |
| Two HashMaps | O(n) | O(n) |
| One HashMap | O(n) | O(n) |
| Frequency Array ⭐ | O(n) | O(1) |

---

# Interview Thinking

When you see this problem, think in this order.

### Step 1

Can I sort?

Yes.

Complexity:

```
O(n log n)
```

Can we do better?

↓

### Step 2

Need character frequencies.

Use HashMap.

Complexity:

```
O(n)
```

Can we optimize space?

↓

### Step 3

Character set is fixed (26 letters).

Use

```java
int freq[] = new int[26];
```

This gives

```
Time : O(n)

Space : O(1)
```

---

# Common Mistakes

❌ Forgetting to compare lengths.

❌ Using sorting even though O(n) solution exists.

❌ Using two HashMaps when one is enough.

❌ Forgetting that array solution works only for lowercase English letters.

---

# Pattern Recognition

Whenever you see

- Character Frequency
- Same Characters
- Rearrangement
- Permutation
- Count Characters

Think

```
Frequency Array

or

HashMap<Character,Integer>
```

---

# Similar Questions

## Easy

- 1. Two Sum
- 217. Contains Duplicate
- 383. Ransom Note
- 387. First Unique Character in a String
- 771. Jewels and Stones

## Medium

- ⭐ 49. Group Anagrams
- ⭐ 438. Find All Anagrams in a String
- 560. Subarray Sum Equals K
- 128. Longest Consecutive Sequence

---

# Revision Cheat Sheet

```
Different lengths?

↓

Return false

↓

Need same frequency?

↓

HashMap

↓

Character set fixed (26)?

↓

Use int[26]

↓

O(n) Time
O(1) Space
```

---

# Key Takeaways

- Always check lengths first.
- Frequency counting is better than sorting.
- If the character set is fixed (`a-z`), prefer an array over a HashMap.
- When the character set is unknown (Unicode, emojis, etc.), use a `HashMap`.
