Excellent! This is one of the **Top 10 most frequently asked array questions** in interviews.

* Amazon ✅
* Google ✅
* Microsoft ✅
* Adobe ✅
* Walmart ✅
* Atlassian ✅

But here's something important:

> **Interviewers are NOT testing stocks.**
>
> They are testing whether you can **recognize patterns and optimize your solution.**

This problem teaches one of the most important patterns:

> **Maintaining the best value seen so far while traversing once.**

---

# 📌 Problem

**LeetCode 121 - Best Time to Buy and Sell Stock**

---

# Step 1: What is the Question Saying?

Suppose you have stock prices for consecutive days.

Example:

```
Day:      1  2  3  4  5  6

Price:    7  1  5  3  6  4
```

The array tells you:

```
prices[i]
```

means

> Price of stock on day **i**

For example

```
prices[0] = 7
```

means

Day 1 price is ₹7.

---

# What Are We Allowed To Do?

Very important.

You may

✅ Buy **only once**

AND

✅ Sell **only once**

Also

```
Buy must happen before Sell.
```

You cannot

```
Sell

↓

Buy
```

That would be illegal.

---

# What Does the Question Want?

It wants

```
Maximum Profit
```

Profit is

```
Selling Price - Buying Price
```

Suppose

Buy

```
₹1
```

Sell

```
₹6
```

Profit

```
6-1=5
```

---

# Example

```
7 1 5 3 6 4
```

Best decision

```
Buy

↓

1
```

Sell

```
↓

6
```

Profit

```
5
```

Answer

```
5
```

---

# Another Example

```
7 6 4 3 1
```

Prices always decrease.

Should we buy?

No.

Every day is worse.

Profit

```
0
```

Because

The problem says

> If no profit is possible,

return

```
0
```

---

# Step 2: What Is It Really Asking?

Forget stocks.

Think mathematically.

Given

```
7 1 5 3 6 4
```

Find

```
Maximum

prices[j]-prices[i]

where

i<j
```

Notice

```
i<j
```

means

Buy before sell.

This is the real problem.

---

# Step 3: Brute Force

Most beginners think

```
Buy on every day

↓

Try selling on every future day

↓

Take maximum
```

Example

```
7

Compare with

1
5
3
6
4
```

Then

```
1

Compare with

5
3
6
4
```

Then

```
5

Compare with

3
6
4
```

This checks every possible pair.

---

## Algorithm

```
For every buying day

    For every selling day after it

        Calculate profit

Keep maximum
```

---

## Java Code

```java
public int maxProfit(int[] prices) {

    int maxProfit = 0;

    for(int i=0;i<prices.length;i++){

        for(int j=i+1;j<prices.length;j++){

            maxProfit = Math.max(maxProfit,
                    prices[j]-prices[i]);
        }
    }

    return maxProfit;
}
```

---

## Complexity

Time

```
O(n²)
```

Space

```
O(1)
```

---

# Can We Do Better?

YES.

Now comes the interviewer thinking.

---

# Step 4: Think Like an Interviewer

Suppose today is

```
Day 5

Price = 6
```

To make maximum profit,

what do we need?

We need

> **the cheapest price before today.**

Not every previous price.

Only

```
Minimum Previous Price
```

That's the biggest observation.

---

# Let's Think

Current price

```
6
```

Possible buying prices

```
7

1

5

3
```

Do we need all of them?

No.

Only the smallest.

```
1
```

Profit

```
6-1=5
```

Amazing!

---

# 🌟 Biggest Observation

At every day,

only one thing matters.

```
Smallest price seen so far.
```

Not

```
All previous prices.
```

This reduces

```
O(n²)

↓

O(n)
```

---

# Step 5: New Algorithm

Keep

```
Minimum Price Seen So Far
```

Whenever you see a new price,

calculate

```
Current Profit

=

Current Price

-

Minimum Price
```

Update answer.

Then update minimum price.

---

# Example

```
7 1 5 3 6 4
```

Initially

```
minPrice = 7

maxProfit = 0
```

---

### Day 1

```
Price = 7
```

Minimum

```
7
```

Profit

```
0
```

---

### Day 2

Price

```
1
```

New minimum

```
1
```

---

### Day 3

Price

```
5
```

Profit

```
5-1=4
```

Maximum

```
4
```

---

### Day 4

Price

```
3
```

Profit

```
3-1=2
```

Maximum still

```
4
```

---

### Day 5

Price

```
6
```

Profit

```
6-1=5
```

Maximum

```
5
```

---

### Day 6

Price

```
4
```

Profit

```
4-1=3
```

Maximum remains

```
5
```

Answer

```
5
```

---

# Interview Trick

Notice the order.

We always calculate

```
Profit

↓

Update Answer

↓

Update Minimum
```

Actually, many people do:

```java
minPrice = Math.min(minPrice, price);
profit = price - minPrice;
```

and it's still correct because if today's price becomes the new minimum, the profit is simply `0` for today. The key invariant is:

* `minPrice` always stores the minimum price seen **up to the current day**.
* `maxProfit` always stores the best profit seen so far.

---

# Optimal Java Code

```java
class Solution {

    public int maxProfit(int[] prices) {

        int minPrice = Integer.MAX_VALUE;
        int maxProfit = 0;

        for (int price : prices) {

            minPrice = Math.min(minPrice, price);

            int currentProfit = price - minPrice;

            maxProfit = Math.max(maxProfit, currentProfit);
        }

        return maxProfit;
    }
}
```

---

# Line-by-Line Explanation

### Step 1

```java
int minPrice = Integer.MAX_VALUE;
```

### Why?

Initially, we don't know the minimum price.

Using `Integer.MAX_VALUE` guarantees that the first price will become the minimum.

---

### Step 2

```java
int maxProfit = 0;
```

### Why?

If prices always decrease, the answer should be `0`, not a negative value.

---

### Step 3

```java
for(int price : prices)
```

### Why?

Visit each day's price exactly once.

---

### Step 4

```java
minPrice = Math.min(minPrice, price);
```

### Why?

Keep track of the cheapest buying opportunity seen so far.

---

### Step 5

```java
int currentProfit = price - minPrice;
```

### Why?

"What if I sell today?"

Today's profit depends on the cheapest day before (or today).

---

### Step 6

```java
maxProfit = Math.max(maxProfit, currentProfit);
```

### Why?

We only care about the best profit seen so far.

---

# Time Complexity

```
O(n)
```

One traversal.

---

# Space Complexity

```
O(1)
```

Only two variables are used.

---

# How Should You Think in an Interview?

When you see:

> **Maximize `A[j] - A[i]` with `i < j`**

don't think about nested loops first.

Ask yourself:

> "While scanning left to right, what information from the past do I really need?"

Here, the answer is:

> **Only the minimum value seen so far.**

---

# Pattern Learned

This problem introduces a very common interview pattern:

## **Maintain the Best Value Seen So Far**

Examples:

* Best Time to Buy and Sell Stock
* Maximum Difference Between Elements
* Maximum Profit
* Maximum Subarray (similar idea, different implementation)
* Prefix minimum / Prefix maximum problems

---

# Common Mistakes

❌ Buying after selling (violates `i < j`).

❌ Returning a negative profit when prices always decrease (the answer should be `0`).

❌ Using nested loops even after noticing that only the minimum previous price matters.

---

# Similar Questions to Practice

### Easy

* 53. Maximum Subarray
* 643. Maximum Average Subarray I

### Medium

* 122. Best Time to Buy and Sell Stock II
* 123. Best Time to Buy and Sell Stock III
* 309. Best Time to Buy and Sell Stock with Cooldown
* 714. Best Time to Buy and Sell Stock with Transaction Fee

---

# 🎯 My Interview Advice

When you encounter an optimization problem, ask yourself these questions:

1. **What am I trying to maximize or minimize?**
2. **Do I need all previous values, or only the best one?**
3. **Can I maintain that best value while traversing once?**

Those three questions are exactly what lead from the `O(n²)` brute-force solution to the `O(n)` optimal solution in this problem.

---

## 🚀 Next Challenge for You

Before looking at the code again, answer this:

**Why do we initialize `minPrice` with `Integer.MAX_VALUE` instead of `prices[0]`?**

Both approaches work. I want you to think about **which one you would choose in an interview and why**. Your answer will tell me whether you've understood the algorithm deeply or are just following the code.
