1. [Introduction to Dynamic Programming](#introduction-to-dynamic-programming)
2. [Properties of a Dynamic Programming Problem](#properties-of-a-dynamic-programming-problem)
3. [Overlapping Subproblems Property](#overlapping-subproblems-property)
4. [Optimal Substructure Property](#optimal-substructure-property)
5. [Solving a Dynamic Programming Problem](#solving-a-dynamic-programming-problem)
6. [Tabulation vs Memoization](#Tabulation-vs-Memoization)
7. [Sample Problems on Dynamic Programming](#Sample-Problems-on-Dynamic-Programming)
   
# 1. Introduction to Dynamic Programming

**Dynamic Programming** (DP) is an algorithmic technique that helps to solve complex problems efficiently by breaking them into smaller overlapping subproblems and storing the results of these subproblems to avoid redundant computations. The main idea is to **reuse past computations** to save time and reduce the number of operations, which makes it especially useful for optimization problems.

DP is often applied to problems involving optimization, such as minimizing costs, maximizing profit, or finding the shortest path.

- **Memoization** is a top-down approach, where we solve the main problem by recursively solving the subproblems and storing their results.
- **Tabulation** is a bottom-up approach, where we solve the problem iteratively by building up from base cases to solve the main problem.

### Key Idea of Dynamic Programming

Dynamic Programming is all about **breaking down a problem into smaller subproblems**, solving them, and **storing their results** to avoid redundant calculations. It's similar to **Divide and Conquer**, but DP is more about **overlapping subproblems** where the same subproblem is solved multiple times.

Let’s start with a classic example:

### Example: Finding the N-th Fibonacci Number

The **Fibonacci sequence** is a great starting point to understand dynamic programming. In the Fibonacci sequence:

- Each number is the sum of the two preceding ones.
- Mathematically, it is defined as:
  - `fib(n) = fib(n-1) + fib(n-2)`, for `n >= 2`.
  - `fib(0) = 0`, `fib(1) = 1`.

The first few Fibonacci numbers are:
```
0, 1, 1, 2, 3, 5, 8, 13, 21, 34, ...
```

If we calculate `fib(5)` using **recursion**, the function can be defined as:

```java
int fib(int n) {
    if (n <= 1) {
        return n;
    }
    return fib(n - 1) + fib(n - 2);
}
```

### Recursion Tree Diagram

Below is a **recursion tree** for calculating `fib(5)`:

```
                          fib(5)
                     /             \
               fib(4)                fib(3)
             /      \                /     \
         fib(3)      fib(2)         fib(2)    fib(1)
        /     \        /    \       /    \
   fib(2)   fib(1)  fib(1) fib(0) fib(1) fib(0)
   /    \
fib(1) fib(0)
```

In this diagram:
- Notice that the function `fib(3)` is called **multiple times**. This repetition is inefficient because we are recalculating the same result over and over again.

This redundancy leads to an **exponential time complexity** of **O(2^n)**, which is very inefficient for large values of `n`.

### Worst-Case Scenario:

In the worst case, we're considering the situation where we are calculating the Fibonacci number for an arbitrary index \(n\) using the **naive recursive algorithm**.

The algorithm works by breaking down the problem into two recursive subproblems:
- To find \(F(n)\), the algorithm calls:
  - \(F(n-1)\)
  - \(F(n-2)\)

For each of these subproblems, the algorithm will recursively compute two more Fibonacci numbers. This means that the computation tree expands exponentially, as each Fibonacci call leads to two more calls.

#### Visualizing the Recursive Tree:
When you want to compute \(F(n)\), the tree of function calls looks like this:

- **Level 0 (Root)**: You make a call to \(F(n)\).
  - This splits into:
  
- **Level 1**: You now need to calculate \(F(n-1)\) and \(F(n-2)\).
  - \(F(n-1)\) itself calls \(F(n-2)\) and \(F(n-3)\).
  - \(F(n-2)\) itself calls \(F(n-3)\) and \(F(n-4)\).
  
And the process repeats. At each level, the number of calls doubles, forming a **binary tree** structure.

#### Why \(2^n\)?

Now, let’s address **why the number of calls is \(2^n\)**.

1. **At level 0 (the root)**: You have **1 call** for \(F(n)\).
2. **At level 1**: You have **2 calls**: \(F(n-1)\) and \(F(n-2)\).
3. **At level 2**: You have **4 calls**: \(F(n-2)\), \(F(n-3)\), \(F(n-3)\), and \(F(n-4)\).
4. **At level 3**: You have **8 calls**.
5. And so on...

At each subsequent level, the number of calls doubles. This is a **binary tree structure** because every call branches into two new calls. The **height of this tree** is roughly \(n\), because you are reducing the argument of the Fibonacci function by 1 at each level (e.g., \(n \to n-1, n-2\), etc.).

In general, the number of function calls at level \(k\) is \(2^k\), and this continues until you reach the base cases \(F(0)\) and \(F(1)\), which do not generate further recursive calls.

#### Total Number of Calls:
The total number of calls is the sum of calls at each level of recursion. This is approximately:

\[
1 + 2 + 4 + 8 + 16 + \dots + 2^k
\]

Where \(k\) is the number of levels, which is proportional to \(n\) (because the recursive calls keep decreasing the argument by 1). The sum of this series is approximately \(2^n\), since the largest term \(2^n\) dominates the sum.

Thus, the time complexity is \(O(2^n)\), because the total number of recursive calls grows exponentially as \(n\) increases.

### Why It’s Exponential:
The reason it's \(2^n\) and not something else is that for each call to calculate \(F(n)\), you generate two more calls. Each call to a Fibonacci function essentially "splits" into two subproblems, causing the recursive call tree to grow exponentially with a branching factor of 2. The total number of nodes (calls) in this tree is proportional to \(2^n\).

---

### Worst-Case Breakdown:
1. **Root level (level 0)**: 1 call (\(F(n)\))
2. **Level 1**: 2 calls (\(F(n-1)\) and \(F(n-2)\))
3. **Level 2**: 4 calls (\(F(n-2), F(n-3), F(n-3), F(n-4)\))
4. **Level 3**: 8 calls
5. ... and so on, doubling at each level.

The total number of calls is roughly \(2^n\), leading to the **exponential time complexity of \(O(2^n)\)**.

This exponential time complexity is a result of the repeated subproblems. Many of these calls are redundant (e.g., \(F(n-2)\) is calculated multiple times), which is why this algorithm is inefficient for large \(n\).

### Improving with Dynamic Programming

To avoid recomputation, we can use **Dynamic Programming** by storing the values of already computed Fibonacci numbers. This can be done either by:

1. **Memoization (Top-Down Approach)**:
   - Store the results of each Fibonacci number in a data structure (e.g., array or dictionary).
   - Check if the result is already computed before making a recursive call.

2. **Tabulation (Bottom-Up Approach)**:
   - Start from the base cases (`fib(0)` and `fib(1)`) and iteratively build up to the desired value (`fib(n)`).

Here is the **DP approach using Tabulation**:

```java
int fib(int n) {
    // Array to store Fibonacci numbers
    int[] f = new int[n + 1];  // 1 extra to handle case, n = 0
    f[0] = 0;
    f[1] = 1;

    for (int i = 2; i <= n; i++) {
        f[i] = f[i - 1] + f[i - 2];
    }

    return f[n];
}
```

### Explanation of Code:

- **Initialization**: We initialize an array `f` of size `n + 2` to store all Fibonacci numbers up to `fib(n)`.
- **Base Cases**: Set `f[0] = 0` and `f[1] = 1`.
- **Loop Calculation**:
  - Start from `i = 2` to `n`, and compute `f[i]` by adding the previous two numbers: `f[i-1] + f[i-2]`.
- This approach has a **linear time complexity** of **O(n)**, which is a big improvement over the recursive solution.

### Text Diagram of Dynamic Programming Table

Let’s visualize how the DP table gets filled for `fib(5)`:

| i  | 0  | 1  | 2  | 3  | 4  | 5  |
|----|----|----|----|----|----|----|
| f  | 0  | 1  | 1  | 2  | 3  | 5  |

- We start with `f[0]` and `f[1]`.
- Then, `f[2] = f[1] + f[0] = 1`.
- `f[3] = f[2] + f[1] = 2`.
- Continue until `f[5] = 5`.

### Why Dynamic Programming Works Well for This Problem

Dynamic Programming works well here because:
1. The **problem can be broken down** into **overlapping subproblems**.
2. **Storing solutions** to subproblems saves us from recalculating the same values multiple times.
3. The recursive solution's time complexity is **O(2^n)**, whereas the DP approach brings it down to **O(n)**, which is **much more efficient**.

### Similar Examples in Dynamic Programming

## 1. **Climbing Stairs Problem**:
   - You can either take 1 or 2 steps to reach the top of a staircase with `n` steps.
   - You need to find the number of distinct ways to reach the top.
   - The solution is very similar to Fibonacci, as `ways(n) = ways(n-1) + ways(n-2)`.

#### **Understanding the Climbing Stairs Problem Using Dynamic Programming**
The **Climbing Stairs Problem** is a classic **Dynamic Programming** (DP) problem that helps beginners understand **recursion**, **memoization**, and **bottom-up approach**.

##### **Problem Statement**
You are given `n` steps. You can climb either **1 step or 2 steps at a time**. Find out **how many distinct ways** you can reach the top.

---

##### **Solution 1: Recursion (Brute Force)**
```java
public class Solution {
    public int climbStairs(int n) {
        return climb_Stairs(0, n);
    }

    public int climb_Stairs(int i, int n) {
        if (i > n) {  // If we exceed n, no valid way
            return 0;
        }
        if (i == n) { // If exactly at step n, count as one way
            return 1;
        }
        // Try both possibilities: taking 1 step and taking 2 steps
        return climb_Stairs(i + 1, n) + climb_Stairs(i + 2, n);
    }
}
```

##### **Explanation**
- **Recursive Tree:** The function recursively calls itself twice:
  - **One path where we take 1 step** (`i + 1`).
  - **One path where we take 2 steps** (`i + 2`).
- The recursion stops when:
  - `i == n` (Valid path found → return `1`).
  - `i > n` (Invalid path → return `0`).

##### **Example: `n = 3`**
Let's visualize the recursive calls.

```
climb_Stairs(0, 3)
   ├── climb_Stairs(1, 3)
   │   ├── climb_Stairs(2, 3)
   │   │   ├── climb_Stairs(3, 3) → 1 ✅
   │   │   ├── climb_Stairs(4, 3) → 0 ❌
   │   ├── climb_Stairs(3, 3) → 1 ✅
   ├── climb_Stairs(2, 3)
       ├── climb_Stairs(3, 3) → 1 ✅
       ├── climb_Stairs(4, 3) → 0 ❌
```
##### **Total Ways = 3**

##### **Complexity Analysis**
- Since we **recompute the same values multiple times**, this solution has an **exponential time complexity** of **O(2ⁿ)**.
- **Optimization needed** ➝ **Memoization (Top-Down DP).**

---

#### **Solution 2: Recursion + Memoization (Top-Down DP)**
```java
public class Solution {
    public int climbStairs(int n) {
        int memo[] = new int[n + 1];  // Memoization array
        return climb_Stairs(0, n, memo);
    }

    public int climb_Stairs(int i, int n, int memo[]) {
        if (i > n) { 
            return 0;
        }
        if (i == n) { 
            return 1;
        }
        if (memo[i] > 0) {  // If already calculated, use memoized value
            return memo[i];
        }
        // Store computed results in memo array
        memo[i] = climb_Stairs(i + 1, n, memo) + climb_Stairs(i + 2, n, memo);
        return memo[i];
    }
}
```

##### **Explanation**
- We use an **array `memo[]`** to store previously computed results.
- Before making recursive calls, we check if the value **already exists** in `memo[]`, preventing redundant calculations.
- If a value is **computed for `climb_Stairs(i, n)` once, it will be reused** next time.

##### **Example: `n = 3`**
```
climb_Stairs(0, 3)
   ├── climb_Stairs(1, 3)
   │   ├── climb_Stairs(2, 3)
   │   │   ├── climb_Stairs(3, 3) → 1 ✅
   │   │   ├── climb_Stairs(4, 3) → 0 ❌
   │   ├── climb_Stairs(3, 3) → 1 ✅ (MEMOIZED)
   ├── climb_Stairs(2, 3) → **MEMOIZED result used!**
```
##### **Total Ways = 3**

##### **Complexity Analysis**
- **Time Complexity:** **O(n)** (Each subproblem is solved only once).
- **Space Complexity:** **O(n)** (for memoization array).

---

## **Solution 3: Bottom-Up DP (Tabulation)**
```java
public class Solution {
    public int climbStairs(int n) {
        if (n == 1) {
            return 1;
        }
        int[] dp = new int[n + 1];  // DP array to store results
        dp[1] = 1;  // 1 way to reach step 1
        dp[2] = 2;  // 2 ways to reach step 2

        // Build the DP table iteratively
        for (int i = 3; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2]; // Sum of previous two steps
        }
        return dp[n]; // Final answer
    }
}
```

##### **Explanation**
- We **build the solution iteratively from step 1 to step `n`**.
- The recurrence relation is:
  ```
  dp[i] = dp[i-1] + dp[i-2]
  ```
- We use an **array `dp[]`** where:
  - `dp[i]` stores the number of ways to reach step `i`.
  - `dp[1] = 1`
  - `dp[2] = 2`
  - For `i ≥ 3`, `dp[i] = dp[i-1] + dp[i-2]`.

##### **Example: `n = 5`**
```
dp[1] = 1
dp[2] = 2
dp[3] = dp[2] + dp[1] = 2 + 1 = 3
dp[4] = dp[3] + dp[2] = 3 + 2 = 5
dp[5] = dp[4] + dp[3] = 5 + 3 = 8
```
##### **Total Ways = 8**

##### **Complexity Analysis**
- **Time Complexity:** **O(n)** (Loop runs `n-2` times).
- **Space Complexity:** **O(n)** (For `dp[]` array).

---

##### **Final Comparison**
| Solution | Approach | Time Complexity | Space Complexity | Notes |
|----------|------------|----------------|----------------|--------|
| **Solution 1** | Recursion (Brute Force) | O(2ⁿ) | O(n) (Recursion Stack) | Exponential time, slow |
| **Solution 2** | Recursion + Memoization | O(n) | O(n) | Efficient, avoids redundant calls |
| **Solution 3** | Bottom-Up DP (Tabulation) | O(n) | O(n) | Iterative, avoids recursion overhead |

### **Optimization: O(1) Space**
- We can further **optimize space** to **O(1)** by using **only two variables** instead of an array:
```java
public int climbStairs(int n) {
    if (n == 1) return 1;
    int prev2 = 1, prev1 = 2;
    for (int i = 3; i <= n; i++) {
        int curr = prev1 + prev2;
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```
##### **Space Complexity: O(1)**
Instead of storing all `dp[i]`, we **only keep track of the last two steps**, which **reduces space usage drastically**.

---

##### **Conclusion**
- **Brute Force Recursion** is inefficient.
- **Memoization (Top-Down DP)** improves performance.
- **Tabulation (Bottom-Up DP)** avoids recursion overhead.
- **Optimized O(1) Space Solution** is best for large `n`.

This problem is a great **introduction to Dynamic Programming** and is **similar to Fibonacci**. 🚀


## 2. **Coin Change Problem**:
   - Given an amount and a set of coins, find out how many ways you can make up that amount.
   - This involves breaking down the problem into smaller amounts and building up a solution.

## **Coin Change Problem - Dynamic Programming (Complete Beginner Guide)**  

#### **🔹 Problem Statement**
Tumhare paas **ek array of coins** given hai, jisme **different denominations** hai, aur tumhe ek **target amount** banana hai using **minimum number of coins**.  

Agar **target amount nahi ban sakta** given coins se, to return `-1`.  

---

#### **🔹 Example**
##### **Example 1**
```plaintext
Input:
coins = [1, 2, 5]
amount = 11

Output:
3   // (5 + 5 + 1)
```
##### **Example 2**
```plaintext
Input:
coins = [2]
amount = 3

Output:
-1   // 3 nahi ban sakta sirf 2 ke coins se
```

---

#### **🔹 Solution Explanation (Dynamic Programming - Bottom-Up Approach)**
Hum **dynamic programming (DP)** ka use karenge kyunki ye **overlapping subproblems** solve karne me help karta hai aur **optimal solution** deta hai.

💡 **Approach:**  
1. Ek **DP array `amt[]` banao**, jo **amount tak minimum coins store karega**.  
2. **Initialization:**  
   - **amt[0] = 0** (Amount `0` banane ke liye `0` coins chahiye).  
   - **Baaki saare values ko `amount + 1` se initialize kar do** (Ye signify karega ki abhi koi valid solution nahi mila).  
3. **Iterate har amount `i` ke liye aur check karo har coin `coins[j]` se:**  
   - Agar **coin `coins[j]` use kar sakte ho** (i.e. `i >= coins[j]`), to  
     ```plaintext
     amt[i] = min(amt[i], 1 + amt[i - coins[j]])
     ```
     Yani **ya to existing value rakh lo ya ek aur coin use karke naya chhota value le lo**.  
4. **Agar `amt[amount]` abhi bhi `amount + 1` hai, to iska matlab ye amount ban nahi sakta, return `-1`**.  
5. **Warna, `amt[amount]` return karo**, jo **minimum coins needed** hai.

---

##### **🔹 Code with Hinglish Comments**
```java
import java.util.Arrays;

class Solution {
    public int coinChange(int[] coins, int amount) {
        
        // DP array banate hai jo minimum coins store karega har amount ke liye
        int amt[] = new int[amount + 1]; 
        
        // Sare values ko `amount + 1` se initialize kar do (impossible value denote karne ke liye)
        Arrays.fill(amt, amount + 1);
        
        // Base Case: Amount 0 banane ke liye 0 coins chahiye
        amt[0] = 0;

        // **Har amount ke liye minimum coins find karna hai**
        for (int i = 1; i <= amount; i++) {  
            for (int j = 0; j < coins.length; j++) {  
                // Agar current coin ka value `i` se chhota ya equal hai tabhi use kar sakte hai
                if (i >= coins[j]) {  
                    // Min coins ka update karenge: ya toh previous value rakh lo ya ek aur coin le lo
                    amt[i] = Math.min(amt[i], 1 + amt[i - coins[j]]);
                }
            }
        }
        
        // Agar `amt[amount]` abhi bhi `amount + 1` hai, iska matlab amount nahi ban sakta
        if (amt[amount] < amount + 1) {
            return amt[amount];  // Valid answer return karo
        }
        return -1;  // Valid solution nahi mila
    }
}
```

---

##### **🔹 Dry Run (Step-by-Step Execution for Input `coins = [1, 2, 5]` & `amount = 11`)**
---
##### **Step 1: Initialize DP Table**
```plaintext
amt[] = [0, INF, INF, INF, INF, INF, INF, INF, INF, INF, INF, INF]
```
(Where `INF = amount + 1 = 12` for initialization)

---

#### **Step 2: Compute DP Values**
##### **i = 1**
- `coins[0] = 1` use kar sakte hai:
  ```plaintext
  amt[1] = min(amt[1], 1 + amt[1 - 1]) = min(12, 1 + amt[0]) = 1
  ```
  ✅ **amt[1] = 1**  

##### **i = 2**
- `coins[0] = 1` use kar sakte hai:
  ```plaintext
  amt[2] = min(amt[2], 1 + amt[2 - 1]) = min(12, 1 + amt[1]) = 2
  ```
- `coins[1] = 2` bhi use kar sakte hai:
  ```plaintext
  amt[2] = min(amt[2], 1 + amt[2 - 2]) = min(2, 1 + amt[0]) = 1
  ```
  ✅ **amt[2] = 1** (kyunki `2` ek hi coin se ban sakta hai)

##### **i = 3**
- `coins[0] = 1` use kar sakte hai:
  ```plaintext
  amt[3] = min(amt[3], 1 + amt[3 - 1]) = min(12, 1 + amt[2]) = 2
  ```
- `coins[1] = 2` use kar sakte hai:
  ```plaintext
  amt[3] = min(amt[3], 1 + amt[3 - 2]) = min(2, 1 + amt[1]) = 2
  ```
  ✅ **amt[3] = 2**

##### **i = 5**
- `coins[2] = 5` directly use kar sakte hai:
  ```plaintext
  amt[5] = min(amt[5], 1 + amt[5 - 5]) = min(12, 1 + amt[0]) = 1
  ```
  ✅ **amt[5] = 1** (Kyunki `5` ek hi coin se ban sakta hai)

---
##### **Final DP Table after filling all values**
```plaintext
amt[] = [0, 1, 1, 2, 2, 1, 2, 2, 3, 3, 2, 3]
```

---
##### **🔹 Final Answer**
```plaintext
amt[11] = 3
```
✅ Minimum **3 coins** chahiye (`5 + 5 + 1`).

---
##### **🔹 Output**
```plaintext
3
```

---
##### **🔹 Why Use Dynamic Programming?**
1. **Overlapping Subproblems** → Har amount ke liye hum pehle se stored values use kar rahe hai.
2. **Optimal Substructure** → Har step me hum **best minimum solution** choose kar rahe hai.
3. **Time Complexity:** `O(amount × n)` → (n = number of coins)
4. **Space Complexity:** `O(amount)`

---
##### **🔹 Summary**
✔ **DP array `amt[]` maintain karna hai jo minimum coins store karega**  
✔ **Loop se har amount ke liye minimum coins calculate karna hai**  
✔ **If `amt[amount] == INF` then return `-1`, otherwise return `amt[amount]`**  
✔ **Time Complexity = `O(n * amount)`, Space Complexity = `O(amount)`**  

---
### More Dynamic Programming Concepts

#### 1. Overlapping Subproblems
Dynamic Programming is useful when the problem can be **broken into smaller subproblems** that **reoccur** throughout the computation. These overlapping subproblems can be solved **once** and reused, which reduces redundant work.

- **Example**: Fibonacci has overlapping subproblems such as `fib(3)` being recalculated multiple times. Instead, we store it to use whenever needed.

#### 2. Optimal Substructure
A problem has an **optimal substructure** if an optimal solution to the problem can be constructed from optimal solutions of its subproblems.

- **Example**: To find the optimal solution for `fib(n)`, we need the optimal solutions for `fib(n-1)` and `fib(n-2)`. The final answer relies directly on the optimal solutions of these smaller subproblems.

### Example of Memoization

Instead of calculating all the values up to `fib(n)` (as in tabulation), we can store values only when they are computed during the recursion:

```java
int fibMemo(int n, int[] memo) {
    if (n <= 1) {
        return n;
    }

    if (memo[n] == 0) {
        memo[n] = fibMemo(n - 1, memo) + fibMemo(n - 2, memo);
    }

    return memo[n];
}

int fib(int n) {
    int[] memo = new int[n + 1];
    return fibMemo(n, memo);
}
```

### Dry Run Example: Memoization for `fib(5)`

- Initially, `memo = [0, 0, 0, 0, 0, 0]`.
- For `fib(5)`, it will recursively call `fib(4)` and `fib(3)`:
  - For `fib(4)`, it will call `fib(3)` and `fib(2)`:
    - This continues until `fib(1)` and `fib(0)`, which return **1** and **0**.
  - The value of `fib(2)` is stored as **1** in `memo[2]`, so it doesn’t need to be recomputed.
- This approach avoids recomputation, leading to significant time savings.

### Summary

- **Dynamic Programming** helps solve complex problems efficiently by avoiding redundant computations through **memoization** or **tabulation**.
- It is particularly useful for problems with **overlapping subproblems** and an **optimal substructure**.
- Examples include **Fibonacci numbers**, **Climbing Stairs**, and **Coin Change**.
- By **storing intermediate results**, DP transforms problems with exponential time complexity to **linear** or **polynomial** time, making them much more practical to solve.

### Additional Terms for Beginners

1. **Memoization vs Tabulation**:
   - **Memoization**: Recursively solve problems, storing the results to avoid recomputation.
   - **Tabulation**: Iteratively solve all subproblems, storing results in a table.

2. **State Representation**:
   - In DP, a **state** is a representation of a subproblem.
   - For example, `f[i]` in Fibonacci represents the i-th Fibonacci number.

3. **State Transition**:
   - This describes how to move from one state to another.
   - For Fibonacci, the **state transition** is: `f[i] = f[i-1] + f[i-2]`.

------------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------------------------
# 2. Properties of a Dynamic Programming Problem

Dynamic Programming (DP) involves two main properties that make a problem solvable using this approach: **Overlapping Subproblems** and **Optimal Substructure**.

### 1. Overlapping Subproblems
This property means that a problem can be broken down into subproblems that are reused multiple times. Dynamic Programming is often used when the solution involves the repeated solving of the same subproblems.

For example, let's consider the **Fibonacci sequence** problem. The Fibonacci sequence is defined as:

- **fib(0) = 0**
- **fib(1) = 1**
- **fib(n) = fib(n - 1) + fib(n - 2)** for **n ≥ 2**

If we use a simple recursive solution, the same function is called repeatedly for some inputs, creating overlapping subproblems. The following code and recursion tree show how this works:

```java
int fib(int n) { 
   if (n <= 1) 
      return n; 
   return fib(n - 1) + fib(n - 2); 
}
```

### Recursion Tree for fib(5)
Below is a visual representation of the recursion tree for finding `fib(5)`:

```
                         fib(5)
                     /             \
               fib(4)                fib(3)
             /      \                /     \
         fib(3)      fib(2)         fib(2)    fib(1)
        /     \        /    \       /    \
  fib(2)   fib(1)  fib(1) fib(0) fib(1) fib(0)
  /    \
fib(1) fib(0)
```

In this recursion tree:
- **fib(3)** is called multiple times.
- **fib(2)** is called multiple times.

The repetition of subproblems means that there is redundancy in computation. To optimize this, DP saves the results of subproblems in a **table** (usually an array or hashmap), so that they don't need to be recomputed. This property makes DP different from simple recursion or divide-and-conquer techniques.

For example, by using **Memoization** (Top-Down approach), we can store results in an array to avoid recalculations:

```java
int[] memo = new int[n + 1];
Arrays.fill(memo, -1);
int fib(int n) {
    if (n <= 1) return n;
    if (memo[n] != -1) return memo[n];
    memo[n] = fib(n - 1) + fib(n - 2);
    return memo[n];
}
```

Alternatively, using **Tabulation** (Bottom-Up approach):

```java
int fib(int n) {
    int[] dp = new int[n + 1];
    dp[0] = 0;
    dp[1] = 1;
    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    return dp[n];
}
```

Both approaches save overlapping subproblem results in an array.

### 2. Optimal Substructure
A problem has the **Optimal Substructure** property if an optimal solution to the problem contains optimal solutions to its subproblems. In other words, you can build the solution to a larger problem from solutions to smaller subproblems.

#### Example: Shortest Path Problem
Consider finding the shortest path between two nodes in a graph. This problem has the **Optimal Substructure** property, meaning that:

- If a node `x` lies in the shortest path from `u` to `v`, then the shortest path from `u` to `v` is a combination of the shortest path from `u` to `x` and the shortest path from `x` to `v`.

The **Floyd-Warshall** and **Bellman-Ford** algorithms are examples of solving problems using Dynamic Programming with Optimal Substructure properties. These algorithms solve the shortest path problem by breaking it down into smaller paths.

Below is a text representation of a graph for finding the shortest path from node **A** to node **D**:

```
      A --3--> B
       \       |
        1      4
         \     |
          > C -5-> D
```
- The shortest path from **A** to **D** can be obtained by taking the path **A -> C -> D**, where the distance is **1 + 5 = 6**.
- This optimal solution uses the smaller paths from **A to C** and **C to D**.

#### Example: Longest Path Problem
Unlike the shortest path, the **Longest Path** problem does not always have an optimal substructure. For example, in an unweighted directed graph, there could be multiple longest paths between two nodes. Below is an example graph:

```
    q -> r
    ^    |
    |    v
    s <- t
```

- There are two possible paths from **q** to **t**:
  - `q -> r -> t`
  - `q -> s -> t`

These paths might have different intermediate nodes, making it impossible to build the longest path simply by combining smaller optimal subpaths.

### Improved Diagram for Optimal Substructure and Non-Optimal Substructure
Below is an updated diagram for understanding the optimal and non-optimal substructure properties:

#### Optimal Substructure Example - Shortest Path
```
   A ------> B ------> D
   \                  /
    \                /
     ---> C --------
```
- Shortest path from **A** to **D** is **A -> B -> D**.
- The shortest path to **D** is a combination of smaller optimal paths from **A to B** and **B to D**.

#### Non-Optimal Substructure Example - Longest Path
```
   q ----> r
   ^       |
   |       v
   s <---- t
```
- Longest path from **q** to **t** is not necessarily a combination of smaller longest paths.

### Recap and Summary
1. **Overlapping Subproblems** - Repeatedly solve the same smaller subproblems, and therefore reuse previously computed solutions.
2. **Optimal Substructure** - The optimal solution to the whole problem can be constructed from the optimal solutions of its subproblems.

### Additional Concepts
- **Memoization**: Storing results of expensive function calls and returning the cached result when the same inputs occur again.
- **Tabulation**: Iteratively building up a table to store solutions to subproblems.
  
Both **Memoization** and **Tabulation** are ways to implement DP.

### Summary Table
| Term              | Description                                                 | Example                |
|-------------------|-------------------------------------------------------------|------------------------|
| Overlapping Subproblems | Repeatedly solve the same subproblems                      | Fibonacci Numbers      |
| Optimal Substructure | The optimal solution can be built from smaller optimal solutions | Shortest Path Problem  |
| Memoization       | Top-Down approach using a cache to store results            | `fib(n)` with array cache |
| Tabulation        | Bottom-Up approach to build solutions iteratively           | DP array for Fibonacci |

Dynamic Programming can be challenging for beginners, but breaking down problems into smaller subproblems, storing solutions, and reusing them are key ideas to mastering it. Practice with problems such as Fibonacci sequence, Coin Change, and Knapsack to get a better grasp of these concepts.

------------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------------------------

# 3. Overlapping Subproblems Property

The **Overlapping Subproblems Property** is a core concept in Dynamic Programming (DP). It means that a problem can be broken down into subproblems, and many of these subproblems are solved multiple times. Dynamic Programming aims to solve these repeated subproblems just once and store their solutions for reuse.

Let's take the **Fibonacci sequence** as an example to understand overlapping subproblems. The Fibonacci sequence is defined as follows:

- **fib(0) = 0**
- **fib(1) = 1**
- **fib(n) = fib(n - 1) + fib(n - 2)** for **n ≥ 2**

### Recursive Approach to Finding Fibonacci Numbers

Consider the following simple recursive code that calculates the **n-th** Fibonacci number:

```java
int fib(int n) {
   if (n <= 1) {
      return n;
   }
   return fib(n - 1) + fib(n - 2);
}
```

#### Recursion Tree for fib(5)

The following diagram represents the **recursion tree** for calculating `fib(5)`:

```
                         fib(5)
                     /             \
               fib(4)                fib(3)
             /      \                /     \
         fib(3)      fib(2)         fib(2)    fib(1)
        /     \        /    \       /    \
  fib(2)   fib(1)  fib(1) fib(0) fib(1) fib(0)
  /    \
fib(1) fib(0)
```

In the recursion tree:
- **fib(3)** is computed **twice**.
- **fib(2)** is computed **three times**.
- **fib(1)** is computed **five times**.

This repetition results in **exponential time complexity**, as the same subproblem is being solved multiple times, making it highly inefficient for larger values of `n`.

### Solution Using Dynamic Programming

To overcome this redundancy, Dynamic Programming uses two key techniques: **Memoization** (Top-Down Approach) and **Tabulation** (Bottom-Up Approach).

### 1. Memoization (Top-Down Approach)

**Memoization** stores the results of each subproblem as it is solved, which ensures that each problem is only solved once. The stored values are then reused whenever required.

#### Example Code - Memoization

Below is the memoized version of the Fibonacci number solution:

```java
public class Fibonacci {
    final int MAX = 100;
    final int NIL = -1;

    int lookup[] = new int[MAX];

    // Function to initialize NIL values in lookup table
    void initialize() {
        for (int i = 0; i < MAX; i++) {
            lookup[i] = NIL;
        }
    }

    // Function for nth Fibonacci number
    int fib(int n) {
        if (lookup[n] == NIL) {
            if (n <= 1) {
                lookup[n] = n;
            } else {
                lookup[n] = fib(n - 1) + fib(n - 2);
            }
        }
        return lookup[n];
    }

    // Driver Code
    public static void main(String[] args) {
        Fibonacci f = new Fibonacci();
        int n = 40;
        f.initialize();
        System.out.println("Fibonacci number is " + f.fib(n));
    }
}
```

**Output**:
```
Fibonacci number is 102334155
```

### How Memoization Works (Text Diagram)

**Memoization** stores the solution for each value of `fib(n)` in an array (`lookup[]`). For example:

- `fib(3)` is stored in `lookup[3]`.
- When `fib(5)` is being computed, the value of `fib(3)` is **reused** instead of recalculating it.

This drastically reduces the number of recursive calls and ensures a **linear time complexity O(n)**.

### 2. Tabulation (Bottom-Up Approach)

**Tabulation** solves the problem iteratively by starting from the smallest subproblem and building the solution for larger subproblems in a **bottom-up** manner.

#### Example Code - Tabulation

Below is the tabulated version for calculating the Fibonacci number:

```java
public class Fibonacci {
    int fib(int n) {
        int[] f = new int[n + 1];
        f[0] = 0;
        f[1] = 1;

        for (int i = 2; i <= n; i++) {
            f[i] = f[i - 1] + f[i - 2];
        }

        return f[n];
    }

    public static void main(String[] args) {
        Fibonacci f = new Fibonacci();
        int n = 9;
        System.out.println("Fibonacci number is " + f.fib(n));
    }
}
```

**Output**:
```
Fibonacci number is 34
```

### How Tabulation Works (Text Diagram)

Tabulation fills the array in a **sequential** order:

```
Step 1: f[0] = 0, f[1] = 1
Step 2: f[2] = f[1] + f[0] = 1
Step 3: f[3] = f[2] + f[1] = 2
Step 4: f[4] = f[3] + f[2] = 3
...
```

**Final Result**: `f[n]` contains the **n-th Fibonacci number**.

### Comparing Memoization and Tabulation

| **Approach**     | **How It Works**                                    | **Time Complexity** | **Space Complexity** |
|------------------|-----------------------------------------------------|---------------------|----------------------|
| Memoization      | Stores solutions during recursive calls (Top-Down)  | O(n)                | O(n) (Stack space)   |
| Tabulation       | Fills up a table iteratively (Bottom-Up)            | O(n)                | O(n) (Table space)   |

- **Memoization** is like writing a diary during a journey to avoid re-walking the same path.
- **Tabulation** is like building a staircase step-by-step from the ground.

------------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------------------------

# 4. Optimal Substructure Property

### What is Optimal Substructure?

A given problem has the **Optimal Substructure Property** if the optimal solution to the problem can be obtained by combining optimal solutions to its **subproblems**. Essentially, the optimal solution to the entire problem depends on the optimal solutions of its smaller parts.

In simple terms, if you can break down a problem into smaller subproblems, solve each of them optimally, and then combine these solutions to get the optimal solution to the overall problem, it means that the problem has an **optimal substructure**.

### Example 1: Shortest Path Problem

The **Shortest Path Problem** is a classic example of a problem that has an optimal substructure.

**Problem Statement**:
- Given a weighted graph, you need to find the shortest path from a **source node** to a **destination node**.

**Optimal Substructure Explanation**:
- If there is a node **x** that lies in the shortest path from source node **u** to destination node **v**, then the shortest path from **u** to **v** is made up of:
  1. **Shortest path from u to x**.
  2. **Shortest path from x to v**.

This means that the optimal solution to reach **v** is a combination of optimal subproblems involving reaching **x**.

Algorithms like **Dijkstra's**, **Floyd–Warshall**, and **Bellman–Ford** utilize this property to efficiently find the shortest paths in a graph.

### Text Diagram for Shortest Path

Consider the following graph where each edge represents the distance between nodes:

```
  (A)----3----(B)
   |           /|
   2          / 1
   |         /  
  (C)----4----(D)
```

To find the shortest path from **A** to **D**:
1. You need to determine the shortest path from **A** to some intermediate node **B**.
2. Then you find the shortest path from **B** to **D**.

In this case:
- **A to B** = 3
- **B to D** = 1

Total **A to D** = **4** (which is shorter than going through **C**).

### Example 2: 0-1 Knapsack Problem

The **0-1 Knapsack Problem** is another classic example where **optimal substructure** is applicable.

**Problem Statement**:
- You are given a set of items, each with a **weight** and a **value**.
- You need to determine the maximum value you can carry in a knapsack of **capacity W**.
- You can either include a complete item or exclude it (hence the name 0-1).

### Step-by-Step Example

Consider the following items:

- **value[] = {60, 100, 120}**
- **weight[] = {10, 20, 30}**
- **Capacity (W) = 50**

We need to maximize the value that we can carry in the knapsack of capacity **50**.

### Text Diagram for Knapsack Subproblems

We can divide the problem into subproblems where each subproblem represents the value that can be obtained from the first **n** items with a capacity **W**.

Consider the following two scenarios for each item:
1. **Exclude the item**: Solve the problem with the remaining **n-1** items and the same capacity **W**.
2. **Include the item**: Add the value of the item to the solution of the remaining **n-1** items, but with a reduced capacity of **W - weight of the item**.

#### Top-Down Solution for 0-1 Knapsack Problem

Here’s how you would implement a **top-down approach** for the **0-1 Knapsack Problem**:

1. Use a **recursive function** to solve the problem.
2. Use a **2D lookup table** (`memo[][]`) to store results of already computed subproblems.

Below is a Java implementation:

```java
class Knapsack {

    // Memoization table to store the results of subproblems
    static int[][] memo;

    // Function to solve the knapsack problem using top-down approach
    static int knapSack(int W, int[] wt, int[] val, int n) {
        // Initialize memoization table with -1 to indicate uncomputed values
        memo = new int[n + 1][W + 1];
        for (int i = 0; i <= n; i++) {
            for (int j = 0; j <= W; j++) {
                memo[i][j] = -1;
            }
        }

        return knapSackRec(W, wt, val, n);
    }

    // Recursive helper function for knapsack
    static int knapSackRec(int W, int[] wt, int[] val, int n) {
        // Base Case: If there are no items or the capacity is 0
        if (n == 0 || W == 0) {
            return 0;
        }

        // If the value is already computed, return it from memo
        if (memo[n][W] != -1) {
            return memo[n][W];
        }

        // If weight of the nth item is more than the knapsack capacity,
        // this item cannot be included
        if (wt[n - 1] > W) {
            memo[n][W] = knapSackRec(W, wt, val, n - 1);
        } else {
            // Otherwise, compute the maximum value obtained by:
            // (1) Including the item, and
            // (2) Not including the item
            memo[n][W] = Math.max(val[n - 1] + knapSackRec(W - wt[n - 1], wt, val, n - 1),
                                  knapSackRec(W, wt, val, n - 1));
        }

        return memo[n][W];
    }

    // Driver code
    public static void main(String[] args) {
        int[] val = {60, 100, 120};
        int[] wt = {10, 20, 30};
        int W = 50;
        int n = val.length;

        System.out.println("Maximum value in knapsack = " + knapSack(W, wt, val, n));
    }
}
```

**Explanation of Code**:

- **Base Case**: If there are no items left (`n == 0`) or the capacity is zero (`W == 0`), the value is `0`.
  
- **Memoization Check**: Before recalculating a subproblem, we **check if the result is already in `memo[][]`**.
  
- **Calculate and Store**: If the value has not been calculated before, we proceed to calculate it and **store it in the memoization table** (`memo[][]`).

- **Recursive Calls**:
  - If the item can be included, we **consider two cases**:
    1. Include the current item and calculate the remaining value.
    2. Do not include the current item and calculate the value.
  - We take the **maximum** of the two values.

- **Memo Table**: The `memo` table stores the value of each subproblem (`n` items and weight `W`).

#### Dry Run Example

Suppose you want to solve the **0-1 Knapsack Problem** using this approach for:

- Items: 
  1. Value = 60, Weight = 10
  2. Value = 100, Weight = 20
  3. Value = 120, Weight = 30
- Knapsack Capacity (W): 50

**Recursive Tree Representation**:

```
K(n=3, W=50)
  ↳ Option 1: Include Item 3
      ↳ K(n=2, W=20)
        ↳ Option 1: Include Item 2
            ↳ K(n=1, W=0) = 0
        ↳ Option 2: Do not include Item 2
            ↳ K(n=1, W=20)
  ↳ Option 2: Do not include Item 3
      ↳ K(n=2, W=50)
```

In each recursive step, we solve subproblems until we reach the **base case** or we have a solution that has been previously calculated and stored in `memo[][]`.

### Key Takeaways for Top-Down Approach

1. **Recursive Nature**: The **top-down approach** relies on **recursion**, which can be intuitive for problems that involve breaking down into smaller subproblems.

2. **Memoization**:
   - You use **extra space** for a lookup table to **store previously computed results**.
   - This helps avoid **redundant computations**, which would otherwise make the algorithm inefficient.

3. **Time Complexity**: The **time complexity** of the top-down approach is **O(n*W)**, where `n` is the number of items and `W` is the knapsack capacity, just like the bottom-up approach.

4. **Space Complexity**: The **space complexity** is **O(n*W)** because of the memoization table.

5. **Comparison with Bottom-Up**:
   - The **bottom-up approach** explicitly builds a solution from smaller subproblems to larger ones using iteration.
   - The **top-down approach** uses recursion to do the same but **memoizes** solutions to avoid redundant calculations.

### Text Diagram Example

Here's a text diagram to visualize the **memo table**:

| Item\Capacity (W) | 0  | 10 | 20 | 30 | 40 | 50 |
|-------------------|----|----|----|----|----|----|
| No items          | 0  | 0  | 0  | 0  | 0  | 0  |
| Item 1            | 0  | 60 | 60 | 60 | 60 | 60 |
| Item 2            | 0  | 60 | 100| 160| 160| 160|
| Item 3            | 0  | 60 | 100| 160| 180| 220|

The **memoization table** keeps the results of subproblems, reducing repeated calculations and helping you solve the entire problem efficiently.

### Dynamic Programming Solution - Bottom-Up Approach

To avoid recomputation, we use a **Dynamic Programming** table to store the solutions to subproblems in a **bottom-up** manner.

```java
// A Dynamic Programming based solution for 0-1 Knapsack problem
class Knapsack {
    // Returns the maximum value that can be put in a knapsack of capacity W
    static int knapSack(int W, int wt[], int val[], int n) {
        int K[][] = new int[n + 1][W + 1];

        // Build table K[][] in bottom-up manner
        for (int i = 0; i <= n; i++) {
            for (int w = 0; w <= W; w++) {
                if (i == 0 || w == 0) {
                    K[i][w] = 0;
                } else if (wt[i - 1] <= w) {
                    K[i][w] = Math.max(val[i - 1] + K[i - 1][w - wt[i - 1]], K[i - 1][w]);
                } else {
                    K[i][w] = K[i - 1][w];
                }
            }
        }
        return K[n][W];
    }

    // Driver Code
    public static void main(String args[]) {
        int val[] = {60, 100, 120};
        int wt[] = {10, 20, 30};
        int W = 50;
        int n = val.length;
        System.out.println(knapSack(W, wt, val, n));
    }
}
```

**Output**:
```
220
```

Let me make the **0-1 Knapsack Problem** much easier to understand for a complete beginner. I'll explain it step-by-step, provide text diagrams, and give a detailed dry run to help you see how the solution works.

### Understanding the 0-1 Knapsack Problem

**Problem Statement**:
- Imagine you are a thief, and you are trying to steal items from a store. Each item has a **weight** and a **value**.
- You have a **knapsack** (a bag) that has a **weight capacity**.
- The goal is to put items in the knapsack in such a way that you **maximize the total value** without exceeding the weight capacity.
- The catch is that you can either take an entire item or leave it (you can't take half of an item).

### Example

Let's take a very simple example:

- Items:  
  1. Value = 60, Weight = 10
  2. Value = 100, Weight = 20
  3. Value = 120, Weight = 30
- Knapsack Capacity (W): **50**

The objective is to find out which items to put in the knapsack to maximize the value while keeping the total weight within **50**.

### Text Diagram of Choices

For each item, we have two choices:
1. **Include** the item in the knapsack.
2. **Do not include** the item.

Let's represent this in a text diagram:

```
Items: [Item 1, Item 2, Item 3]
Knapsack Capacity: 50

Item 1 (Value = 60, Weight = 10)
Item 2 (Value = 100, Weight = 20)
Item 3 (Value = 120, Weight = 30)

Choices for Item 1:
    - Include it: Remaining capacity = 50 - 10 = 40
    - Don't include it: Capacity remains 50

Choices for Item 2:
    - If included, remaining capacity = 40 - 20 = 20
    - If not included, capacity remains unchanged

Choices for Item 3:
    - If included, remaining capacity = 20 - 30 = Negative (Cannot be done)
    - If not included, capacity remains the same
```

The **goal** is to figure out how to maximize the total value.

### Dry Run with Breakdown

We use a **bottom-up approach** (dynamic programming) to build a table that helps us figure out the best way to solve this problem.

**Step 1: Create a Table**

We'll create a table, where:

- **Rows** represent items (including a row for no items).
- **Columns** represent knapsack capacities from **0** to **W**.

| Items\Capacity | 0  | 10 | 20 | 30 | 40 | 50 |
|----------------|----|----|----|----|----|----|
| 0 items        | 0  | 0  | 0  | 0  | 0  | 0  |
| Item 1         | 0  | 60 | 60 | 60 | 60 | 60 |
| Item 2         | 0  | 60 | 100| 160| 160| 160|
| Item 3         | 0  | 60 | 100| 160| 180| 220|

**Explanation**:

1. **Row 0**: No items, so the value is always **0** for all capacities.
  
2. **Row 1** (Item 1 - Value = 60, Weight = 10):
   - For capacity **0-9**: The knapsack cannot include Item 1, so value is **0**.
   - For capacity **10-50**: We can include Item 1, so the value is **60**.

3. **Row 2** (Item 2 - Value = 100, Weight = 20):
   - For capacity **0-19**: The knapsack cannot include Item 2, so we take the value from above, which is **60**.
   - For capacity **20**: We can include Item 2, giving us **100**.
   - For capacity **30-50**: We can include both Item 1 and Item 2, giving us **160**.

4. **Row 3** (Item 3 - Value = 120, Weight = 30):
   - For capacity **0-29**: The knapsack cannot include Item 3, so we take the value from above.
   - For capacity **30**: We can include Item 3, giving us **160**.
   - For capacity **40**: We can include Item 1 and Item 3, giving us **180**.
   - For capacity **50**: We can include Item 2 and Item 3, giving us **220**.

### Dry Run - Detailed Steps

**Capacity = 50, Items = [Item 1, Item 2, Item 3]**

1. **Include Item 3** (Weight = 30):
   - Remaining Capacity = **50 - 30 = 20**.
   - Now we look at the optimal solution for **capacity 20** with **Item 1 and Item 2**.
   - **Item 2** (Value = 100, Weight = 20) fits perfectly, giving us **100**.
   - Total Value = **120 (Item 3) + 100 (Item 2) = 220**.

2. **Do Not Include Item 3**:
   - Now, look at the optimal solution for **capacity 50** with **Item 1 and Item 2**.
   - We can include **both** Item 1 and Item 2, which gives a value of **160**.

From these steps, we can see that **including Item 3** gives the highest value (**220**).

### Key Concepts for Beginners

1. **Breaking Down Choices**:
   - For each item, you either **include** it or **exclude** it.
   - Recursively, find the best combination of items.

2. **Dynamic Programming Table**:
   - Use a **table** to store the results of subproblems.
   - Avoid recalculating overlapping subproblems by storing their results.

3. **Bottom-Up Approach**:
   - Start by solving the simplest subproblems (like no items or zero capacity).
   - Gradually build up to solve the entire problem.

4. **Understanding the Optimal Substructure**:
   - The optimal solution for the whole problem depends on the optimal solutions of subproblems.
   - In this case, we combine the best solutions for smaller capacities and fewer items to build the overall solution.

------------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------------------------

# 5. Solving a Dynamic Programming Problem

**Dynamic Programming** (DP) is a problem-solving technique that helps break down a problem into smaller, manageable parts called **subproblems** and solves them to obtain the answer to the bigger problem. The beauty of DP lies in the fact that it stores the results of already solved subproblems to avoid redundant calculations, thus significantly improving efficiency.

For someone new to programming, let’s break this down step-by-step with explanations, examples, and diagrams.

### Key Concepts of Dynamic Programming

Before diving into solving a problem using DP, let’s understand some core properties that define a DP problem:

1. **Overlapping Subproblems**: 
   - Involves solving the same smaller problems multiple times.
   - A great example is calculating **Fibonacci numbers**, where the same subproblem (like finding `fib(3)`) might occur multiple times in the recursive structure.
2. **Optimal Substructure**: 
   - The problem can be broken into smaller problems whose optimal solutions can be combined to solve the overall problem optimally.
   - For example, finding the shortest path in a graph can be broken into finding the shortest paths between intermediate nodes.

### Steps to Solve a Dynamic Programming Problem

1. **Identify if it is a DP problem**.
2. **Decide a state expression with the fewest parameters**.
3. **Formulate state relationship**.
4. **Use Tabulation or Memoization to implement**.

Let’s go through each of these steps in more detail with examples.

#### Step 1: How to Classify a Problem as a DP Problem?

- Typically, DP problems involve finding the **maximum or minimum value** of something, **counting** the number of possible ways to achieve a result, or problems involving **probabilities**.
- A classic indication that DP can be applied is when a problem has **overlapping subproblems** and **optimal substructure**.

Example:

- Consider a problem where you have to maximize a **value** while respecting some **constraints**—such as choosing items for a knapsack. This is a strong indicator that DP could be applied.

#### Step 2: Deciding the State

A **state** in DP represents a certain standing or a snapshot of the solution at a given point. The goal is to represent each state using as few parameters as possible, making it easy to transition between states.

**Example**: 0-1 **Knapsack Problem**

In the **Knapsack problem**, our state can be defined by:

- `index`: Which item are we considering?
- `weight`: How much capacity is left in the knapsack?

So we define our **state** as `DP[index][weight]`, which gives us the maximum profit we can get from items in the range `[0...index]` with a knapsack capacity of `weight`.

#### Step 3: Formulating State Relationship

This is the most challenging part of solving DP problems, as it requires **intuition** and **practice**. To help beginners understand, let’s look at a simple example.

**Example: Number of Ways to Form a Number `N` Using Coins 1, 3, and 5**

You are given three coins with values `1`, `3`, and `5`, and you need to determine the number of ways to reach a total of `N`.

For example, if `N = 6`, there are `8` ways:

- 1 + 1 + 1 + 1 + 1 + 1
- 1 + 1 + 1 + 3
- 1 + 1 + 3 + 1
- 1 + 3 + 1 + 1
- 3 + 1 + 1 + 1
- 3 + 3
- 1 + 5
- 5 + 1

To solve this problem dynamically, let’s think about the **state**:

- Define `state(n)` as the total number of ways to reach `n` using coins `1`, `3`, and `5`.

Now, let’s derive a relationship for `state(n)`:

- We can add `1`, `3`, or `5` to previous states:
  - `state(n) = state(n-1) + state(n-3) + state(n-5)`

This means that for each value of `n`, we can derive the answer based on the values of the previous states.

### Deriving the State Relationship
We need to express the value of `state(n)` in terms of smaller values (i.e., values less than `n`). This is where the **state relationship** comes into play.

Let's use an example to understand how we can derive the relationship for `state(n)`.

#### Example: Calculating `state(7)`
If we want to find the **number of ways to form the value `7`**, let's think about what the last coin used could be. The last coin used could be one of `1`, `3`, or `5`.

**Case 1: Last coin is `1`**
- If the last coin is `1`, we need to know how many ways we can form the value `6` (because `7 - 1 = 6`). This means:
  - **state(7)** depends on **state(6)**.

**Case 2: Last coin is `3`**
- If the last coin is `3`, we need to know how many ways we can form the value `4` (because `7 - 3 = 4`). This means:
  - **state(7)** depends on **state(4)**.

**Case 3: Last coin is `5`**
- If the last coin is `5`, we need to know how many ways we can form the value `2` (because `7 - 5 = 2`). This means:
  - **state(7)** depends on **state(2)**.

Now, if we combine these three cases, we can write:
- `state(7) = state(6) + state(4) + state(2)`

This is the state relationship! It tells us that to determine the number of ways to form `7`, we just need to add up the number of ways to form `6`, `4`, and `2`.

##### Example Dry Run and Diagram

For `N = 6`:

- `state(6) = state(5) + state(3) + state(1)`

Let's see this with a diagram:

```
  N = 6
 /  |  \
5   3   1
/|  /|   |
4 2  2  0 (base case, returns 1)
```

**Base Cases**:
- `state(0) = 1`: There is one way to make `0` (by using no coins).
- `state(n) < 0 = 0`: If `n` is negative, there are no ways.
### Meaning of `if (n == 0) return 1;`
This line is saying that if the target value (`n`) is `0`, then there is exactly **one way** to achieve it. The **one way** is to do **nothing**—meaning we don't use any coins or numbers to reach `0`. This is an important concept because it establishes a valid "starting point" for our calculations.

Let’s illustrate this idea with a few examples:

#### Example: Ways to Make `0` Using Coins
Suppose you want to find the number of ways to **make a sum of `0`** using coins of values `1`, `3`, and `5`. There is **one way** to do this: **use no coins at all**.

In other words, you could think of "doing nothing" as a valid way to achieve a sum of `0`, which counts as **one solution**.

Therefore:
- `state(0) = 1`
  
### How Does It Help in Recursion and Dynamic Programming?
The base case `if (n == 0) return 1;` is crucial in **breaking the recursion**. When we break a problem into subproblems, at some point, we reach a point where `n` becomes `0`. If we do not have this base case, the function would keep calling itself indefinitely and never stop.

The base case essentially says:
- If you have **reduced the target value to `0`**, it means you have found a valid combination of choices that sums to `n`.

For example, if `n = 7`, you break it down into subproblems like `state(7) = state(6) + state(4) + state(2)`. Eventually, when `n` reduces to `0`, it counts as **one valid way**, meaning the combination of values used to reach `0` is one possible solution.

**Code**:

```java
int solve(int n) {
    if (n < 0) 
        return 0;
    if (n == 0)  
        return 1;
    return solve(n - 1) + solve(n - 3) + solve(n - 5);
}
```

#### Step 4: Adding Memoization or Tabulation

This is the **easiest** part of the process once you have the state and state relationship set. Memoization involves storing already-computed results so you don’t recompute them. **Tabulation** is more of a bottom-up approach where you build the solution step-by-step.

**Memoization** Example:

- Add an array called `dp[]` initialized to `-1` to store the intermediate results.

```java
// Initialize the dp array with -1
int dp[MAXN];

int solve(int n) {
    if (n < 0)  
        return 0;
    if (n == 0)  
        return 1;

    // Return if already calculated
    if (dp[n] != -1) 
        return dp[n];

    // Store the result and return
    return dp[n] = solve(n - 1) + solve(n - 3) + solve(n - 5);
}
```

**Tabulation** Example (Bottom-Up):

Instead of using recursion, you fill in a table for each possible value of `n` starting from `0`.

```java
int solveTabulation(int n) {
    int dp[] = new int[n + 1];
    dp[0] = 1;

    for (int i = 1; i <= n; i++) {
        dp[i] = 0;
        if (i - 1 >= 0) dp[i] += dp[i - 1];
        if (i - 3 >= 0) dp[i] += dp[i - 3];
        if (i - 5 >= 0) dp[i] += dp[i - 5];
    }
    
    return dp[n];
}
```

### Summary of Dynamic Programming Steps

1. **Identify the Problem** as a DP Problem:
   - Does it have **overlapping subproblems** and **optimal substructure**?
2. **Define the State**:
   - What is the state you need to track to solve the problem?
3. **Formulate State Transition**:
   - What is the relationship between current and previous states?
4. **Choose Memoization or Tabulation**:
   - Use **memoization** to optimize recursive calls.
   - Use **tabulation** to solve the problem iteratively.

------------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------------------------

# 6. Tabulation vs Memoization

Dynamic Programming (DP) is a powerful technique to solve problems with optimal substructure and overlapping subproblems properties. There are two common approaches to implement dynamic programming: **Tabulation (Bottom-Up)** and **Memoization (Top-Down)**. Let's explore these two approaches in detail.

#### Overview
- **Tabulation (Bottom-Up)**: This approach solves the problem by starting from the simplest, "base" cases and building up the solution step by step.
- **Memoization (Top-Down)**: This approach breaks the problem down into subproblems, solves them recursively, and stores their solutions to avoid redundant calculations.

Below is a summary table that compares these approaches:

| **Tabulation** | **Memoization** |
| --- | --- |
| Starts from base case and builds up | Starts from main problem and solves subproblems on demand |
| Iterative solution | Recursive solution |
| All subproblems are solved, even if not needed | Only the necessary subproblems are solved |
| More efficient in terms of function calls | More function calls and stack space needed |
| Uses a table to store results sequentially | Uses a lookup table to store results on demand |
| Easier for problems that require all solutions | Ideal for problems with many overlapping subproblems |

#### Detailed Explanation of Both Approaches

##### 1. Tabulation Method (Bottom-Up Dynamic Programming)

**Concept**: Tabulation involves starting from the smallest subproblem and iteratively solving larger problems until we reach the desired solution. It’s called **Bottom-Up** because we start from the ground level (base cases) and move up step by step to solve the main problem.

**Example: Factorial Calculation (Tabulation Approach)**
We can calculate the factorial of a number \( n \) using a **Bottom-Up** approach. Let's say our state is `dp[x]`, where `dp[x]` represents the factorial of `x`. We calculate the factorial starting from 0 up to \( n \).

```cpp
int dp[MAXN];

// Base case
dp[0] = 1; // Factorial of 0 is 1
for (int i = 1; i <= n; i++) {
    dp[i] = dp[i-1] * i; // Multiply current index to previous factorial
}
```

**Diagram to Illustrate Tabulation**:
- We start from `dp[0] = 1`.
- Then calculate `dp[1] = dp[0] * 1`.
- Continue calculating `dp[2] = dp[1] * 2` and so on until `dp[n]`.

```
0! -> 1,  1! -> 1,  2! -> 2,  3! -> 6,  ...,  n! -> dp[n]
```

**Visual Representation**:
```
Base Case -> Step 1 -> Step 2 -> Step 3 -> ... -> Step n
```
We fill in the values step-by-step, from left to right.

##### 2. Memoization Method (Top-Down Dynamic Programming)

**Concept**: In memoization, we solve the main problem recursively by breaking it down into subproblems. If a subproblem has already been solved, we use the stored solution. It’s called **Top-Down** because we start from the main problem and solve smaller subproblems as we need them.

**Example: Factorial Calculation (Memoization Approach)**
Below is an example of calculating factorial using the **Top-Down** approach with memoization. We use an array to store the solutions for each state and avoid recalculating them.

```cpp
int dp[MAXN];

// Initialize all values in dp to -1
fill(dp, dp + MAXN, -1);

// Function to solve the factorial
int factorial(int x) {
    if (x == 0) return 1; // Base case
    if (dp[x] != -1) return dp[x]; // Use previously calculated value
    return dp[x] = x * factorial(x - 1); // Store and return the solution
}
```

**Diagram to Illustrate Memoization**:
- Suppose we need `factorial(5)`.
- We first calculate `factorial(4)` and store the value in `dp[4]`.
- To calculate `factorial(3)`, we recursively call `factorial(2)` until we reach the base case.
- If we need to calculate `factorial(4)` again, we use the value stored in `dp[4]`.

```
factorial(5) -> factorial(4) -> factorial(3) -> factorial(2) -> ... -> factorial(0)
```
If a state has been solved already, we reuse it:
```
factorial(5) -> factorial(4) (already solved) -> reuse
```

#### Real-Life Example: Stairs Climbing Problem

Imagine you need to climb to the top of a staircase that has `n` steps, and you can either take 1 or 2 steps at a time. In how many distinct ways can you reach the top?

##### 1. Tabulation Approach:
- Define `dp[i]` as the number of ways to reach step `i`.
- Base cases: `dp[0] = 1` (1 way to be on the ground), `dp[1] = 1` (1 way to reach the first step).
- For each step `i` from 2 to `n`, we calculate:
  \[
  dp[i] = dp[i-1] + dp[i-2]
  \]
- This formula works because you can reach step `i` either from `i-1` or `i-2`.

**Code**:
```cpp
int dp[n+1];
dp[0] = 1; // Base case: on the ground
dp[1] = 1; // Base case: first step

for (int i = 2; i <= n; i++) {
    dp[i] = dp[i-1] + dp[i-2];
}
```

##### 2. Memoization Approach:
In the **Top-Down** approach, we start with `n` steps and recursively calculate the number of ways for smaller steps while storing the results.

```cpp
int dp[MAXN];
fill(dp, dp + MAXN, -1);

int climbStairs(int n) {
    if (n <= 1) return 1; // Base case
    if (dp[n] != -1) return dp[n]; // Use stored value
    return dp[n] = climbStairs(n - 1) + climbStairs(n - 2); // Store result
}
```

**Diagram to Understand the Problem Visually**:
- Starting from `n`:
  - Calculate ways to reach `n-1` and `n-2`.
  - Recursively reduce until you reach the base case (`0` or `1`).

```
n -> (n-1, n-2) -> (n-2, n-3) -> ...
```

#### Key Takeaways
- **Tabulation** (Bottom-Up): Solves problems iteratively, starting from the simplest cases, and builds up to the solution. Uses arrays or tables to store solutions sequentially.
- **Memoization** (Top-Down): Uses recursion to solve problems and stores the results of overlapping subproblems. It breaks the main problem into smaller parts on demand.
- **Performance**:
  - **Tabulation** is generally faster for certain problems since it avoids recursion.
  - **Memoization** can use more stack space due to recursion but is easier to implement for some problems with overlapping subproblems.

#### Example Problem: Fibonacci Sequence
The Fibonacci sequence can be solved with both methods:
- **Tabulation**: Start from the base cases (`F(0)`, `F(1)`) and build up.
- **Memoization**: Recursively calculate `F(n)` while storing previous values to prevent recalculations.

**Visual Representation**:
- **Tabulation**: Fill an array starting from `F(0)` to `F(n)`.
- **Memoization**: Break down `F(n)` recursively, store results, and reuse them.

##### Example Diagram (Tabulation vs Memoization)
| Approach | Step-by-Step |
| --- | --- |
| **Tabulation** | \[ `F(0)` -> `F(1)` -> `F(2)` -> `...` -> `F(n)` \] |
| **Memoization** | Start from `F(n)` -> Break down to `F(n-1)` and `F(n-2)` -> Break down further until `F(0)` |

Both approaches are useful in dynamic programming, and choosing the best approach depends on the specific problem, constraints, and performance requirements.

![image](https://github.com/user-attachments/assets/4fe7a4f9-3468-48a0-877e-af6521d560d8)

The image provided explains the differences between two approaches used in Dynamic Programming (DP): **Tabulation** and **Memoization**. Below is a detailed breakdown of each category compared in the table:

### 1. State Transition Relation
- **Tabulation**: The **state transition relation** is often difficult to think of upfront because this method relies on building up from the smallest states to the desired solution.
- **Memoization**: In contrast, the **state transition relation** in memoization is usually easier to think about because you break down the problem recursively, solving smaller subproblems as needed.

### 2. Code Complexity
- **Tabulation**: The code can become **complicated** as we need to keep track of multiple layers of dependencies, often using nested loops.
- **Memoization**: Memoization uses a **recursive approach**, which can be easier to implement and understand. It requires fewer conditions to manage directly since recursive function calls are made until the base condition is met.

### 3. Speed
- **Tabulation**: This approach is generally **faster** because it directly accesses values from a precomputed table without needing multiple recursive function calls.
- **Memoization**: Memoization can be **slower** due to the overhead from recursive calls, including storing and returning from the lookup table.

### 4. Subproblem Solving
- **Tabulation**: In **Tabulation**, **all subproblems** must be solved. This approach is suitable if every subproblem needs to be computed at least once. It often outperforms a top-down solution because of fewer function call overheads.
- **Memoization**: In **Memoization**, **only some subproblems** are solved—those that are necessary to get to the solution. This approach skips solving unnecessary subproblems, leading to optimization in certain cases.

### 5. Table Entries
- **Tabulation**: In the **tabulated version**, the **table is filled sequentially**, starting from the smallest state (usually `dp[0]`) to the desired state (`dp[n]`).
- **Memoization**: In **memoization**, the **table is filled on demand**, i.e., values are only computed when they are required for further calculations. Not all entries in the table may be filled.

### Summary:
- **Tabulation** (Bottom-Up approach) is more efficient when all subproblems need to be solved, as it avoids the overhead of recursion. It is fast since all values are directly accessed from the precomputed table.
- **Memoization** (Top-Down approach) is easier to implement for those who prefer recursive thinking. It optimizes by only solving the subproblems that are needed, making it a good fit when fewer subproblems are required.

### Example to Illustrate:
Consider the **Fibonacci Sequence**:
- In **Tabulation**, you solve `fib(0)`, `fib(1)`, `fib(2)`... all the way up to `fib(n)` iteratively. You start from the base cases and build up.
- In **Memoization**, you solve `fib(n)` by breaking it into `fib(n-1)` and `fib(n-2)` recursively, and cache the results to avoid recalculating subproblems.

------------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------------------------

# 7. Sample Problems on Dynamic Programming

### **Problem #1: Binomial Coefficients**
**Description:**
The **binomial coefficient** \( C(n, k) \) is a mathematical value that represents the number of ways to choose \( k \) items from \( n \) distinct items, without considering the order of selection. This is often referred to as "**n choose k**." Binomial coefficients are important in combinatorics and appear in many areas of mathematics, such as algebra and probability theory. They are used to solve problems that involve combinations, like selecting a subset from a larger set.

### **Understanding \( C(n, k) \)**
The **binomial coefficient** \( C(n, k) \) can be defined as:

\[
C(n, k) = \frac{n!}{k! \cdot (n-k)!}
\]

- \( n! \) (n factorial) represents the product of all positive integers up to \( n \).
- \( k! \) represents the factorial of \( k \).
- \( (n-k)! \) represents the factorial of the difference between \( n \) and \( k \).

The value \( C(n, k) \) gives us the number of ways to select \( k \) items out of \( n \) items. For example, if you have 4 items and you want to choose 2, \( C(4, 2) \) represents the number of ways you can do that.

### **How We Came Up with the Recursive Formula**
The recursive formula for the binomial coefficient is:

\[
C(n, k) = C(n-1, k-1) + C(n-1, k)
\]

**Explanation**:
- \( C(n, k) \) is derived from considering how we choose \( k \) elements out of \( n \) elements.
- This recursive relation can be understood by breaking down the selection process.

### **Intuitive Explanation with Examples**
Let's consider what happens when you have a set of \( n \) elements and you want to choose \( k \) of them. Imagine that you have \( n \) elements labeled from 1 to \( n \), and you need to decide whether to include a particular element, say element \( n \), in your selection.

1. **Including Element \( n \)**:
   - If we include element \( n \), then we only need to choose \( k-1 \) more elements from the remaining \( n-1 \) elements.
   - The number of ways to do this is \( C(n-1, k-1) \).

2. **Not Including Element \( n \)**:
   - If we do **not** include element \( n \), then we need to choose all \( k \) elements from the remaining \( n-1 \) elements.
   - The number of ways to do this is \( C(n-1, k) \).

Since the total number of ways to choose \( k \) elements from \( n \) elements is the sum of the two possibilities (including or excluding element \( n \)), we get:

\[
C(n, k) = C(n-1, k-1) + C(n-1, k)
\]

### **Base Cases**
The base cases for the binomial coefficient are:
1. \( C(n, 0) = 1 \): There is only one way to choose 0 items from \( n \) items (choose nothing).
2. \( C(n, n) = 1 \): There is only one way to choose all \( n \) items from \( n \) items.

### **Example: Calculating \( C(4, 2) \)**
Let's calculate \( C(4, 2) \) using the recursive formula:

\[
C(4, 2) = C(3, 1) + C(3, 2)
\]

Now, let's break down \( C(3, 1) \) and \( C(3, 2) \):
- \( C(3, 1) = C(2, 0) + C(2, 1) = 1 + 2 = 3 \)
- \( C(3, 2) = C(2, 1) + C(2, 2) = 2 + 1 = 3 \)

So,

\[
C(4, 2) = 3 + 3 = 6
\]

This means there are 6 ways to choose 2 elements from a set of 4 elements.

### **Diagram Representation**
Below is a visual representation of the recursive calculation for \( C(4, 2) \):

```
                     C(4, 2)
                   /         \
            C(3, 1)       C(3, 2)
           /      \        /      \
     C(2, 0)  C(2, 1) C(2, 1)  C(2, 2)
```
- Each node represents a call to the binomial coefficient function.
- The branches show how each problem is divided into smaller subproblems.

### **Conclusion**
The recursive formula \( C(n, k) = C(n-1, k-1) + C(n-1, k) \) is derived from the combinatorial idea of either **including** or **excluding** an element in the selection process. This way, it breaks down a larger problem into smaller subproblems that are easier to solve, which is the essence of **Dynamic Programming**.

**Overlapping Subproblems**:
The same subproblems are solved multiple times, for example:
- \( C(3, 1) \) is calculated twice when calculating \( C(5, 2) \).

Here is a recursion tree for \( n = 5, k = 2 \):

```
                             C(5, 2)
                     /                        \
           C(4, 1)                           C(4, 2)
            /   \                            /                \
    C(3, 0)   C(3, 1)          C(3, 1)                 C(3, 2)
                 /    \                /     \                       /        \
         C(2, 0)    C(2, 1) C(2, 0) C(2, 1)         C(2, 1)    C(2, 2)
```

This overlapping makes it suitable for a **Dynamic Programming** approach to avoid redundant calculations.

### Pseudo Code** (Using Tabulation):
```cpp
// Returns value of Binomial Coefficient C(n, k)
int binomialCoeff(int n, int k) {
    int C[n+1][k+1];  // Step 1: Create a 2D array to store results of subproblems.

    // Step 2: Fill in the values in a bottom-up manner
    for (int i = 0; i <= n; i++) {
        for (int j = 0; j <= min(i, k); j++) {
            // Base Cases: If j == 0 or j == i, the value is always 1
            if (j == 0 || j == i)
                C[i][j] = 1;
            // Recursive relation: C(n, k) = C(n-1, k-1) + C(n-1, k)
            else
                C[i][j] = C[i-1][j-1] + C[i-1][j];
        }
    }
    return C[n][k];  // Step 3: Return the value of C(n, k)
}
```

### **Step-by-Step Explanation**

1. **Initialize the Table**:
   ```cpp
   int C[n+1][k+1];
   ```
   - We create a 2D array `C[n+1][k+1]` to store intermediate values.
   - The array is used to store the values of smaller subproblems, so we don't need to recalculate them repeatedly.
   - `C[i][j]` will store the value of \( C(i, j) \) (i.e., the number of ways to choose `j` items from `i` items).

2. **Fill in the Values Using a Bottom-Up Approach**:
   ```cpp
   for (int i = 0; i <= n; i++) {
       for (int j = 0; j <= min(i, k); j++) {
           // Base Cases
           if (j == 0 || j == i)
               C[i][j] = 1;
           // Using the recursive formula
           else
               C[i][j] = C[i-1][j-1] + C[i-1][j];
       }
   }
   ```
   - We use **two nested loops** to iterate through each possible value of `i` and `j`.
   - The **outer loop** (`i`) runs from 0 to `n`, and the **inner loop** (`j`) runs from 0 to the minimum of `i` and `k`.
   - For each pair `(i, j)`:
     - If `j` is 0 or `j` is equal to `i`, then \( C(i, j) \) is 1. This is because there is only one way to choose 0 items (choose nothing) or all `i` items.
     - Otherwise, we use the **recursive formula**:
       \[
       C(i, j) = C(i-1, j-1) + C(i-1, j)
       \]
       This formula represents the two cases:
       - **Include the current item**: \( C(i-1, j-1) \)
       - **Exclude the current item**: \( C(i-1, j) \)
   
3. **Return the Final Value**:
   ```cpp
   return C[n][k];
   ```
   - After filling in all values, we return `C[n][k]`, which represents the number of ways to choose `k` items from `n` items.

### **Visual Representation with Example**
Let’s calculate \( C(4, 2) \) using this approach.

1. **Base Cases**:
   - \( C(i, 0) = 1 \) for all `i` (there’s only one way to choose zero items).
   - \( C(i, i) = 1 \) for all `i` (there’s only one way to choose all items).

2. **Filling the Table**:
   We create a 2D array, and fill it row by row:
   
   ```
   i \ j | 0   1   2
   -------------------
   0     |  1
   1     |  1   1
   2     |  1   2   1
   3     |  1   3   3
   4     |  1   4   6
   ```
   - **Row 0**: `C[0][0] = 1`
   - **Row 1**: `C[1][0] = 1`, `C[1][1] = 1`
   - **Row 2**: `C[2][0] = 1`, `C[2][1] = 2`, `C[2][2] = 1`
   - **Row 3**: `C[3][0] = 1`, `C[3][1] = 3`, `C[3][2] = 3`
   - **Row 4**: `C[4][0] = 1`, `C[4][1] = 4`, `C[4][2] = 6`

Thus, \( C(4, 2) = 6 \).

### **Why is This Approach Efficient?**
- **Avoiding Recalculation**: Instead of recalculating the same values multiple times (which happens in the naive recursive approach), this approach stores intermediate results in a table, making it much more efficient.
- **Time Complexity**: The time complexity is \( O(n \times k) \), which is much better compared to the exponential complexity of the naive recursive solution.
- **Space Complexity**: The space complexity is also \( O(n \times k) \) due to the storage requirements of the 2D array.

### **Summary**
The **binomial coefficient** \( C(n, k) \) represents the number of ways to choose `k` items from `n` items without considering the order. The code provided uses **Dynamic Programming** to solve this problem in a **bottom-up** manner, avoiding redundant calculations by storing subproblem results in a table. This approach is efficient in terms of both time and space compared to a recursive approach without memoization.

Using Dynamic Programming, we can efficiently compute binomial coefficients by storing results in a table and avoiding redundant calculations (overlapping subproblems). This leads to a much faster solution compared to computing everything recursively without storage.

Let’s analyze how the use of **Dynamic Programming (DP)** optimizes the complexity of a problem like calculating the **Binomial Coefficient**, and how the approach drastically reduces computation compared to a naive method.

### **Complexity Without Dynamic Programming:**
The naive approach for computing the **Binomial Coefficient** \( C(n, k) \) is through **recursion**. Let’s look at the formula:

\[
C(n, k) = C(n-1, k-1) + C(n-1, k)
\]

The base cases are:

\[
C(n, 0) = 1 \quad \text{and} \quad C(n, n) = 1
\]

In the recursive approach:
- We make **two recursive calls** for each state: \( C(n-1, k-1) \) and \( C(n-1, k) \).
- For each call, the two recursive branches continue until reaching a base case.

#### **Time Complexity Without DP:**
- The time complexity of this **recursive approach** is **exponential**: \( O(2^n) \).
- This is because each call branches into two, leading to a tree with **2^n** nodes in the worst case.

#### **Why is the Complexity Exponential?**
The reason the complexity is **exponential** is because **overlapping subproblems** are recalculated multiple times. For example:
- If we try to calculate \( C(5, 2) \), we need to compute \( C(4, 1) \) and \( C(4, 2) \).
- Then for \( C(4, 2) \), we will again need to compute \( C(3, 1) \), and **\( C(3, 1) \) will also be calculated while computing \( C(4, 1) \)**, leading to repeated calculations.

In the recursion tree, you end up recalculating the same subproblems many times, which results in **redundant computations** and hence an exponential number of operations.

### **Dynamic Programming Optimization:**
**Dynamic Programming (DP)** helps to solve this problem more efficiently by storing the intermediate results and using them when needed. This approach is known as **Memoization (Top-Down)** or **Tabulation (Bottom-Up)**.

#### **How DP Optimizes Complexity:**
1. **Storing Results (Memoization/Tabulation)**:
   - In DP, we use a **lookup table** (2D array `C[n+1][k+1]` in the code) to store the values of subproblems that have already been solved.
   - Once we calculate \( C(i, j) \), we store it in the table. When we need \( C(i, j) \) again, we simply **look it up** instead of recalculating it.
   
2. **Avoiding Recalculation**:
   - By storing the results of subproblems, DP ensures that each subproblem is calculated only **once**. This avoids the **redundant calculations** of the naive recursive approach.

3. **Bottom-Up Calculation**:
   - In the **bottom-up approach**, we start from the simplest subproblems (like \( C(0, 0) \), \( C(1, 0) \), \( C(1, 1) \)) and work our way up to the desired solution \( C(n, k) \). This avoids recursion altogether and eliminates the overhead associated with recursive calls.

#### **Time Complexity with DP**:
- The **time complexity** of the DP solution is **\( O(n \times k) \)**, which is **polynomial**.
  - We fill an \( (n+1) \times (k+1) \) table, and each cell is filled in **constant time**.
- This is a significant improvement over the naive recursive approach with an **exponential time complexity**.

#### **Space Complexity**:
- The **space complexity** is also \( O(n \times k) \) due to the size of the lookup table.
- However, if space optimization is required, the solution can be further optimized to use **\( O(k) \)** space by maintaining only the current and previous rows of the table.

### **Visual Example:**
Consider calculating \( C(5, 2) \):

- **Recursive Approach** (Without DP):
  - We calculate \( C(5, 2) \) by recursively finding \( C(4, 1) \) and \( C(4, 2) \).
  - To find \( C(4, 2) \), we again need \( C(3, 1) \) and \( C(3, 2) \).
  - Notice that \( C(3, 1) \) is calculated **multiple times**. This repetition is what causes the exponential growth.

- **DP Approach** (With Memoization/Tabulation):
  - In the DP approach, once \( C(3, 1) \) is calculated, it is **stored in the table**.
  - When \( C(4, 2) \) needs \( C(3, 1) \), we can directly **use the stored value** instead of recalculating it.
  - This drastically reduces the number of operations, resulting in polynomial complexity.





### **Problem #2: Minimum Number of Jumps to Reach End**
**Description**:
You are given an array where each element represents the **maximum number of steps** you can move forward from that element. The goal is to find the **minimum number of jumps** required to reach the end of the array starting from the first element.

**Example**:
- **Input**: `arr[] = {1, 3, 5, 8, 9, 2, 6, 7, 6, 8, 9}`
- **Output**: `3`
- The goal is to determine the **minimum number of jumps** needed to reach the last element (`9` at index `10`).

### Example Explanation Step-by-Step

1. **Start from Index 0 (`Value 1`)**:
   - We start at the first element which is `1`.
   - From index `0`, the value is `1`, which means we can jump to **index 1**. The number of jumps made so far is `1`.

2. **Move to Index 1 (`Value 3`)**:
   - At index `1`, the value is `3`. This means we can jump up to **3 steps forward**, meaning we can reach **index 2, 3, or 4**.
   - Among these options, **index 4** (`Value 9`) is the farthest we can reach in a single jump from index `1`.
   - Hence, we jump to **index 4**. Now the number of jumps made is `2`.

3. **Move to Index 4 (`Value 9`)**:
   - At index `4`, the value is `9`. This value allows us to jump up to **9 steps forward**.
   - Since the current position (`index 4`) is already close to the end of the array, this jump can take us directly to the **last element** at **index 10** (`Value 9`).
   - This is the third and **final jump**.

Thus, the sequence of jumps is:
- **Index 0** -> **Index 1** -> **Index 4** -> **Index 10**.

The **minimum number of jumps** required to reach the end is **3**.

The problem is to determine the fewest jumps required to reach the end, given that:
1. The first element is `1`, allowing a jump to element `3`.
2. The next jump from `3` can reach `5`, `8`, or `9`, and we choose the best one to minimize the total number of jumps.

**Solution Approach**:
We build an array called `jumps[]`, where `jumps[i]` indicates the minimum number of jumps needed to reach `arr[i]` from `arr[0]`.

### Naive Approach Code in Java

Let's first look at the **naive recursive solution** for the Minimum Number of Jumps to Reach the End problem. This approach tries all possible ways and returns the **minimum number of jumps** needed to reach the end.

```java
public class MinimumJumps {

    // Function to return the minimum number of jumps needed to reach the end
    public static int minJumps(int[] arr, int start, int n) {
        // Step 1: Base Cases
        // If the starting point is at or beyond the last index, no more jumps are needed
        if (start >= n - 1) {
            return 0;
        }

        // If the current value at `arr[start]` is 0, it's impossible to proceed
        if (arr[start] == 0) {
            return Integer.MAX_VALUE; // signifies that it is not possible
        }

        // Step 2: Initialize minimum jumps to infinity
        int minJumps = Integer.MAX_VALUE;

        // Step 3: Iterate over all possible jumps from the current position
        for (int i = 1; i <= arr[start] && start + i < n; i++) {
            // Recursively calculate the number of jumps needed from `start + i`
            int jumps = minJumps(arr, start + i, n);

            // Update `minJumps` if the calculated jumps are valid
            if (jumps != Integer.MAX_VALUE) {
                minJumps = Math.min(minJumps, jumps + 1);
            }
        }

        // Step 4: Return the minimum number of jumps found
        return minJumps;
    }

    // Driver code
    public static void main(String[] args) {
        int arr[] = {1, 3, 5, 8, 9, 2, 6, 7, 6, 8, 9};
        int n = arr.length;

        System.out.println("Minimum number of jumps: " + minJumps(arr, 0, n));
    }
}
```

### Detailed Explanation

#### 1. Base Cases
- **`if (start >= n - 1)`**:
  - This condition checks if we have reached or moved past the last element. If we have, no more jumps are needed, so we return `0`.
- **`if (arr[start] == 0)`**:
  - If the value at the current index is `0`, it's not possible to move forward, and we return `Integer.MAX_VALUE` to signify that reaching the end is impossible.

#### 2. Initialize `minJumps` to Infinity
- **`int minJumps = Integer.MAX_VALUE;`**:
  - Since we're trying to find the minimum number of jumps, we initialize `minJumps` to a very large value (`Integer.MAX_VALUE`) to represent an "infinite" number of jumps. This value will be updated during recursion to reflect the optimal number of jumps.

#### 3. Iterate Over All Possible Jumps
- **`for (int i = 1; i <= arr[start] && start + i < n; i++)`**:
  - This loop considers all possible jumps from the current position (`start`). The range of `i` is from `1` to `arr[start]`, representing the possible number of steps that can be taken from the current index.
  - For each possible jump, the function recursively calls `minJumps` to calculate how many jumps would be needed from that new position (`start + i`).
  
- **`int jumps = minJumps(arr, start + i, n);`**:
  - This recursive call calculates the number of jumps required from the position `start + i` to reach the end.
  
- **`if (jumps != Integer.MAX_VALUE)`**:
  - This condition ensures that we only consider valid paths (i.e., those for which the end can actually be reached). If a path is invalid (`Integer.MAX_VALUE`), we skip it.

- **`minJumps = Math.min(minJumps, jumps + 1);`**:
  - If a valid number of jumps (`jumps`) is found, we add `1` (for the current jump) and update `minJumps` to keep track of the minimum value.

#### 4. Return the Result
- The function returns the value of `minJumps`, which will contain the minimum number of jumps needed to reach the end of the array.

### Recap on Time Complexity
- The **time complexity** for the **brute-force solution** is **exponential** due to the **recursive branching** at each level.
- In the **worst-case**, the **recursion tree** could have **up to `2^n` nodes**, assuming each index generally leads to a small number of potential branches (usually about **2 on average**).
- In scenarios where there are consistently more options (e.g., each element has a value of `4` or `5`), the time complexity could trend closer to **`3^n`** or higher, depending on how many branches are created on average.

By contrast, using **dynamic programming** can reduce this complexity to **polynomial** (`O(n^2)`) since we **memoize** or use **tabulation** to store intermediate results and avoid recomputation.

**Pseudo Code**:
   ```java
   // Returns minimum number of jumps to reach arr[n-1] from arr[0]
   int minJumps(int arr[], int n) {
       // If the array is empty or the first element is 0, we cannot proceed
       if (n == 0 || arr[0] == 0)
           return Integer.MAX_VALUE;

       // jumps[n-1] will hold the result
       int jumps[] = new int[n];
       jumps[0] = 0;

       // Find the minimum number of jumps to reach arr[i] from arr[0]
       for (int i = 1; i < n; i++) {
           jumps[i] = Integer.MAX_VALUE; // Initialize as infinity (impossible to reach initially)
           for (int j = 0; j < i; j++) {
               if (i <= j + arr[j] && jumps[j] != Integer.MAX_VALUE) {
                   // If i is reachable from j and j itself is reachable
                   jumps[i] = Math.min(jumps[i], jumps[j] + 1);
                   break; // We found the minimum number of jumps to reach i
               }
           }
       }
       return jumps[n - 1];
   }
   ```

**Explanation of the Code:**
- **Initialization**:
  - If the array is empty or if the first element is `0`, it is **impossible** to move, so we return `INT_MAX` (or any large value to signify an unreachable state).
  - `jumps[0] = 0` because **no jump** is needed to be at the starting position.
  
- **Nested Loops**:
  - The **outer loop** iterates over all positions (`i`) from `1` to `n-1`.
  - The **inner loop** iterates from `0` to `i-1` to find the **minimum number of jumps** to reach `i`.
  - If **position `i` is reachable** from position `j` (`j + arr[j] >= i`) and position `j` is itself reachable (`jumps[j] != INT_MAX`), we update `jumps[i]` as `min(jumps[i], jumps[j] + 1)`.

- **`jumps[i] = min(jumps[i], jumps[j] + 1)`**:
   - This line calculates the **minimum number of jumps** required to reach position `i` from the starting point.
   - `jumps[j] + 1` means taking one jump from position `j` to `i`.
   - The `min` function ensures that the minimum number of jumps to reach `i` is chosen, considering all possible ways to reach `i`.
 - **`break;`**:
   - Once we find a valid jump from `j` to `i` and update `jumps[i]`, we break out of the inner loop. This is because we only need to find **one way** to reach `i` with the minimum number of jumps. Once a valid minimum is found, there's no need to keep checking.
 - **`return jumps[n - 1];`**:
   - This returns the value at `jumps[n - 1]`, which represents the **minimum number of jumps** needed to reach the last element (`arr[n-1]`) from the first element (`arr[0]`).

**Dynamic Programming** solves this problem in the following ways:
1. **Storing Intermediate Results**:
   - The array `jumps[]` stores the **minimum number of jumps** needed to reach each position.
   - Each position is calculated **only once**, and its value is reused whenever needed, which drastically reduces redundant calculations.

2. **Systematic Calculation**:
   - The DP approach works in a **bottom-up** manner, systematically calculating the minimum jumps for all positions starting from `0` to `n-1`.
   - Instead of trying all possible paths in an unsystematic manner (as in the recursive approach), the DP approach **builds up** the solution step by step.
  
### Complexity Comparison
- **Naive Approach**:
  - **Time Complexity**: **\( O(2^n) \)**. This is because every element leads to recursive calls, and every possible path is explored.
  - **Space Complexity**: **\( O(n) \)** due to the recursion stack.

- **Dynamic Programming Approach**:
  - **Time Complexity**: **\( O(n^2) \)** due to the nested loop structure.
  - **Space Complexity**: **\( O(n) \)** because of the `jumps[]` array.

### **Problem #3: Longest Increasing Subsequence (LIS)**
**Description**:
The **Longest Increasing Subsequence** is a subsequence of a given sequence, where the subsequence's elements are sorted in **strictly increasing order**, and it has the **maximum possible length** compared to other such subsequences in the array.

**Example:**
For an array `[10, 22, 9, 33, 21, 50, 41, 60, 80]`, the LIS is `[10, 22, 33, 50, 60, 80]` with a length of **6**.

### Naive Approach

```java
public class LongestIncreasingSubsequence {

    // Function to return the length of the LIS starting from index 'currIndex'
    static int lis(int[] arr, int prevIndex, int currIndex) {
        // Base case: if the current index reaches the end of the array, return 0
        if (currIndex == arr.length) {
            return 0;
        }

        // Option 1: Exclude the current element and move to the next element
        int exclude = lis(arr, prevIndex, currIndex + 1);

        // Option 2: Include the current element if it is greater than the previous one
        int include = 0;
        if (prevIndex == -1 || arr[currIndex] > arr[prevIndex]) {
            include = 1 + lis(arr, currIndex, currIndex + 1);
        }

        // Return the maximum of including or excluding the current element
        return Math.max(include, exclude);
    }

    // Driver code
    public static void main(String[] args) {
        int[] arr = {10, 22, 9, 33, 21, 50, 41, 60, 80};
        System.out.println("Length of LIS: " + lis(arr, -1, 0));
    }
}
```

The naive recursive solution can be described as follows:
- For each element, decide whether to include it in the current subsequence or to start a new subsequence.
- Recursively find all possible increasing subsequences.

This leads to an exponential number of function calls, similar to exploring all subsets.

### Complexity Analysis
- **Time Complexity**: `O(2^n)`:
  - At each step, we have two options: **include** or **exclude**. This leads to a **binary decision** tree where each node has **two children**, resulting in an exponential number of calls.
  - Specifically, for each element, we make two choices, leading to `2 * 2 * ... * 2 = 2^n` possible combinations in the worst case.
  
- **Space Complexity**: `O(n)`:
  - The space complexity is proportional to the **depth of the recursion tree**.
  - In the worst case, the depth of the recursion tree can be `n`, resulting in **linear space complexity** due to function call stacks.

### Limitations of the Naive Approach
The **naive approach** works well for understanding the problem and the recursive structure, but it is extremely inefficient for large input sizes. The number of possible subsequences grows exponentially, making it impractical for even moderately large arrays. To improve efficiency, we need to avoid redundant calculations, which can be done using **Dynamic Programming**.

### Dynamic Programming Approach Summary
The **Dynamic Programming (DP)** solution for LIS improves efficiency by **storing intermediate results** in an array, eliminating the need to recompute overlapping subproblems. The approach involves:

- Maintaining a `dp` array where `dp[i]` represents the **length of the longest increasing subsequence** ending at index `i`.
- Building up the solution in a **bottom-up manner** by iterating over all elements and computing the LIS length iteratively, avoiding redundant calculations.
- This approach reduces the time complexity to **`O(n^2)`** and space complexity to **`O(n)`**.

### Key Insights
- The **recursive approach** involves making **binary decisions** at each step: either include the current element or exclude it.
- **Dynamic Programming** helps by storing solutions to subproblems and efficiently building up the answer without recomputation.

### Dynamic Programming Approach
The **dynamic programming** approach to solve LIS is much more efficient, reducing the time complexity to **`O(n^2)`**. It involves storing intermediate results and building the solution in a **bottom-up** fashion.

Here's how to do it:

1. **Define the State**:
   - Let `dp[i]` be the **length of the longest increasing subsequence ending at index `i`**.

2. **State Transition**:
   - To determine `dp[i]`, look at all the previous elements (`j` from `0` to `i-1`).
   - If `arr[j] < arr[i]`, then `arr[i]` can be added to the increasing subsequence ending at `arr[j]`. Therefore, update `dp[i]` as `dp[i] = max(dp[i], dp[j] + 1)`.

3. **Initialization**:
   - Initialize `dp[i] = 1` for all `i`, as the minimum length of the LIS ending at any index is **1** (the element itself).

4. **Final Solution**:
   - The length of the LIS is the **maximum value** in the `dp` array.

### Pseudo Code for Dynamic Programming Approach
```java
int lis(int[] arr, int n) {
    // Create a dp array to store the length of the longest increasing subsequence ending at each index.
    int[] dp = new int[n];
    
    // Initialize LIS values for all indexes to 1, since the minimum length of the LIS ending at any index is at least 1 (i.e., the element itself).
    for (int i = 0; i < n; i++) {
        dp[i] = 1;
    }

    // Compute optimized LIS values for each element in a bottom-up manner.
    // Iterate over each element from the second element (i = 1) to the last element.
    for (int i = 1; i < n; i++) {
        // For each element, compare it with all the previous elements to determine if it forms an increasing subsequence.
        for (int j = 0; j < i; j++) {
            // If arr[i] is greater than arr[j], it means arr[i] can extend the increasing subsequence ending at arr[j].
            // We also check if extending dp[j] results in a longer subsequence for dp[i].
            if (arr[i] > arr[j] && dp[i] < dp[j] + 1) {
                dp[i] = dp[j] + 1;  // Update dp[i] to reflect the new longer subsequence ending at index i.
            }
        }
    }

    // Find the maximum value in the dp array, which represents the length of the longest increasing subsequence in the entire array.
    int max = 0;
    for (int i = 0; i < n; i++) {
        if (max < dp[i]) {
            max = dp[i];
        }
    }

    return max; // Return the length of the longest increasing subsequence.
}
```

### Example Walkthrough
Let’s take an example array `[10, 22, 9, 33, 21, 50, 41, 60, 80]` to see how the dynamic programming approach works.

1. **Initialization**:
   - `dp[]` initially looks like: `[1, 1, 1, 1, 1, 1, 1, 1, 1]` (each element itself is a subsequence of length 1).

2. **Filling `dp[]`**:
   - For `i = 1` (`arr[1] = 22`):
     - Compare with `j = 0` (`arr[0] = 10`):
       - Since `10 < 22`, `dp[1] = max(dp[1], dp[0] + 1) = 2`.
   - For `i = 2` (`arr[2] = 9`):
     - No `j` such that `arr[j] < arr[2]`.
   - For `i = 3` (`arr[3] = 33`):
     - Compare with `j = 0, 1, 2`:
       - Since `10 < 33`, `dp[3] = max(dp[3], dp[0] + 1) = 2`.
       - Since `22 < 33`, `dp[3] = max(dp[3], dp[1] + 1) = 3`.
       - Since `9 < 33`, `dp[3]` remains `3`.
   - Continue this process for all `i`.

3. **Final `dp[]`**:
   - `dp[]` will be `[1, 2, 1, 3, 2, 4, 4, 5, 6]`.
   - The **maximum value** in `dp[]` is `6`, which is the length of the **longest increasing subsequence**.

### Complexity Analysis
- **Time Complexity**: `O(n^2)` because there are **two nested loops**. The outer loop runs for each element, and the inner loop runs for all previous elements.
- **Space Complexity**: `O(n)` due to the `dp` array.

### Optimized Approach: Binary Search
There’s a more efficient approach using **Binary Search** that reduces the time complexity to **`O(n log n)`**. This involves using a dynamic list to keep track of the potential LIS:

1. Maintain an array (`tails`) where `tails[i]` stores the **minimum possible tail value** for all increasing subsequences of length `i+1`.
2. Use **binary search** to determine where in `tails` to place each element of `arr`. If the element is larger than all elements in `tails`, append it. Otherwise, replace the first element in `tails` that is greater than or equal to it.

### Summary
- The **naive solution** for LIS has an **exponential time complexity** (`O(2^n)`) due to the need to evaluate all subsequences.
- The **dynamic programming solution** reduces the time complexity to **`O(n^2)`** by avoiding recalculation of overlapping subproblems and storing intermediate results in a `dp` array.
- The **optimized approach** with **binary search** further reduces the complexity to **`O(n log n)`** but can be a bit harder to implement and understand for beginners.

The **dynamic programming approach** is often used in interviews because it demonstrates an understanding of problem breakdown, state representation, and complexity reduction using tabulation/memoization.

### **Problem #4: 0-1 Knapsack Problem**
**Description**:
Given weights and values of `n` items, put these items in a knapsack of capacity `W` to get the maximum value in the knapsack.

You can either include the entire item or exclude it (0-1 property). 
- **val[]** represents values.
- **wt[]** represents weights.
- **W** is the knapsack capacity.

**Optimal Substructure**:
There are two options for each item:
1. Include it in the knapsack.
2. Do not include it.

The optimal value is the maximum of:
- The value obtained by excluding the `n`-th item.
- The value obtained by including the `n`-th item plus the maximum value for the remaining capacity.

**Pseudo Code**:
```cpp
// Returns the maximum value that can be put in a knapsack of capacity W
int knapSack(int W, int wt[], int val[], int n) {
    int K[n+1][W+1];
    // Build table K[][] in bottom-up manner
    for (int i = 0; i <= n; i++) {
        for (int w = 0; w <= W; w++) {
            if (i == 0 || w == 0)
                K[i][w] = 0; // Base case
            else if (wt[i-1] <= w)
                K[i][w] = max(val[i-1] + K[i-1][w - wt[i-1]],  K[i-1][w]);
            else
                K[i][w] = K[i-1][w];
        }
    }
    return K[n][W];
}
```

### **Summary**
- **Optimal Substructure** and **Overlapping Subproblems** are two fundamental properties that determine if a problem can be solved using Dynamic Programming.
- Use **Tabulation** (Bottom-Up) or **Memoization** (Top-Down) to efficiently solve problems by avoiding recalculations.
- Examples like **Binomial Coefficients**, **Minimum Jumps**, **Longest Increasing Subsequence**, and the **0-1 Knapsack Problem** demonstrate different applications of these techniques.

For better understanding, break down complex problems into small subproblems and use diagrams to visualize the problem-solving process step by step.
