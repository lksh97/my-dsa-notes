## 🔹 **Sliding Window Technique - Theory, Explanation & Example** (Hinglish)

Sliding Window ek **efficient approach hai jo sequential data** (arrays ya strings) ke saath use hoti hai, **jab hume kisi contiguous (continuous) subarray ya substring par operations perform karni hoti hai**.

💡 **Key Idea**:
- Ek **"window" (subarray/substring)** select karte hain jo dynamically **expand** aur **contract** hoti hai.
- Window ko dynamically adjust karke optimal solution nikalte hain bina **brute-force (`O(n²)`)** approach use kiye.

---

## 🔹 **Types of Sliding Window**
1️⃣ **Fixed Sliding Window**  
   - **Jab window ka size fix hota hai.**  
   - Example: "Find maximum sum of subarray of size `k`".
   - **Expand karne ke saath ek fixed size tak maintain karna hota hai.**  

2️⃣ **Variable Sliding Window**  
   - **Jab window ka size dynamically badhta aur ghata hai.**  
   - Example: "Find the smallest subarray whose sum is `>= target`".
   - **Expand karna aur contract karna dono zaroori hota hai.**  

---

## 🔹 **Theory of Sliding Window**
💡 **Sliding Window 3 steps me kaam karta hai:**

### **Step 1: Expand Window (right pointer)**
- Jab tak **condition satisfy nahi hoti**, right pointer badhao aur window expand karo.  

### **Step 2: Shrink Window (left pointer)**
- Jab condition satisfy ho jaye, **left pointer move karke window ko chhota karo**, taaki **optimal solution mile**.

### **Step 3: Update Answer**
- **Har step pe current window ka answer update karte raho**, aur **sabse optimal window length ya sum ya substring track karo**.

---

## 🔹 **Text Diagram for Sliding Window**
💡 **Example:** `"Find the longest substring without repeating characters"`  
📌 **Input:** `"abcabcbb"`

```plaintext
Index:   0   1   2   3   4   5   6   7  
Chars:   a   b   c   a   b   c   b   b  
         ↑   ↑
      (start) (end)
      Expand Window
```
✅ **Step 1:** `"abc"` valid hai, `end` pointer aage badhao.  
❌ **Step 2:** `'a'` duplicate mil gaya → `start` pointer badhao.  
✅ **Step 3:** `"bca"` valid hai, max length update karo.  

---
## 🔹 **Example 1: Fixed Size Sliding Window**
📌 **Problem:** `Find the maximum sum of any subarray of size k`

```java
public int maxSum(int[] nums, int k) {
    int maxSum = Integer.MIN_VALUE; // Sabse chhota possible value rakho, taaki negative values bhi handle ho jaye
    int sum = 0; // Yeh sliding window ka sum track karega

    for (int i = 0; i < nums.length; i++) {
        sum += nums[i]; // Window ka ek element add kar rahe hain (Expand window)

        // Jab tak window ka size 'k' nahi hota, sirf sum collect karo
        if (i >= k - 1) {  // Jab i, k-1 ya usse bada ho tabhi window valid hai
            maxSum = Math.max(maxSum, sum); // Max sum ko update karte raho
            sum -= nums[i - k + 1]; // Window ka pehla element hatao (Shrink window)
        }
    }

    return maxSum; // Maximum sum return karo
}
```

---

### ✅ **Explanation (Step by Step in Hinglish)**
1. **Initialize variables:**
   - `maxSum` ko `Integer.MIN_VALUE` rakha, taaki negative values bhi properly handle ho.
   - `sum` ko `0` rakha, jo current window ka sum track karega.

2. **Traverse the array using a sliding window approach:**
   - `sum += nums[i]` -> Naya element add karo (expand window).
   - Jab tak `i < k - 1`, tab tak sirf sum collect karo, kyunki valid window nahi bani hai.

3. **Once window size `k` is reached:**
   - `maxSum = Math.max(maxSum, sum)` -> Maximum sum ko update karo.
   - `sum -= nums[i - k + 1]` -> Pehle wale element ko hatao (shrink window).

4. **Loop continues until we check all subarrays of size `k`.**
5. **Finally, return `maxSum`, which holds the maximum sum of any subarray of size `k`.**

---

### ✅ **Example Walkthrough**
**Input:**
```java
int[] nums = {2, 1, 5, 1, 3, 2};
int k = 3;
System.out.println(maxSum(nums, k)); 
```

**Steps:**
| Index (`i`) | `nums[i]` | `sum` (After Add) | `maxSum` (After Compare) | `sum` (After Remove) |
|-------------|----------|------------------|---------------------|----------------|
| 0           | 2        | 2                | -                   | -              |
| 1           | 1        | 3                | -                   | -              |
| 2           | 5        | 8                | 8                   | 6 (Remove `2`) |
| 3           | 1        | 7                | 8                   | 6 (Remove `1`) |
| 4           | 3        | 9                | 9                   | 4 (Remove `5`) |
| 5           | 2        | 6                | 9                   | 5 (Remove `1`) |

**Output:**  
```
9
```
(Maximum sum subarray `{5, 1, 3}` gives sum `9`.)

---

### ✅ **Time Complexity**
- **O(N)**, because we traverse the array once (`N` elements), and each element is added & removed from the window exactly once.

### ✅ **Space Complexity**
- **O(1)**, as we are using only a few integer variables (`sum`, `maxSum`), no extra space needed.

---
## 🔹 **Example 2: Variable Size Sliding Window**
📌 **Problem:** `Find the smallest subarray with sum >= target`

```java
public int minSubArrayLen(int target, int[] nums) {
    int start = 0, sum = 0, minLength = Integer.MAX_VALUE; // Minimum length track karne ke liye

    for (int end = 0; end < nums.length; end++) {
        sum += nums[end]; // Window expand kar rahe hain (naya element add karo)

        // Jab tak sum >= target ho, window ko shrink karne ki koshish karo
        while (sum >= target) {
            minLength = Math.min(minLength, end - start + 1); // Minimum length update karo
            sum -= nums[start]; // Window ka pehla element hatao (shrink window)
            start++; // Start pointer ko aage badhao
        }
    }

    // Agar koi valid subarray nahi mila, toh 0 return karo
    return (minLength == Integer.MAX_VALUE) ? 0 : minLength;
}
```

---

### ✅ **Explanation (Step by Step in Hinglish)**

1. **Initialize variables:**
   - `start = 0` → Window ka start pointer.
   - `sum = 0` → Window ka current sum.
   - `minLength = Integer.MAX_VALUE` → Minimum subarray length track karne ke liye.

2. **Expand window (Increase `end` pointer)**:
   - `sum += nums[end]` → Current window mein naya element add karo.

3. **Shrink window (Move `start` pointer)**:
   - Jab tak `sum >= target`, minimum subarray length update karo.
   - `sum -= nums[start]` → Pehla element hatao.
   - `start++` → Window ka start aage badhao.

4. **Final check**:
   - Agar `minLength` ab bhi `Integer.MAX_VALUE` hai, iska matlab koi valid subarray nahi mila → Return `0`.
   - Warna, `minLength` return karo.

---

### ✅ **Example Walkthrough**
#### **Input:**
```java
int target = 7;
int[] nums = {2, 3, 1, 2, 4, 3};
System.out.println(minSubArrayLen(target, nums));
```

#### **Steps:**
| `end` | `nums[end]` | `sum` (After Add) | `minLength` (After Shrink) | `sum` (After Remove) | `start` (Updated) |
|------|------------|------------------|----------------------|----------------|---------|
| 0    | 2          | 2                | -                    | -              | 0       |
| 1    | 3          | 5                | -                    | -              | 0       |
| 2    | 1          | 6                | -                    | -              | 0       |
| 3    | 2          | 8                | **4** (`[2,3,1,2]`)  | 6 (Remove `2`) | 1       |
| 4    | 4          | 10               | **3** (`[3,1,2,4]`)  | 7 (Remove `3`) | 2       |
| 5    | 3          | 10               | **2** (`[4,3]`)      | 6 (Remove `1`) | 3       |
| 5    | 3          | 6                | 2                    | -              | -       |

#### **Output:**
```java
2
```
(Smallest subarray `[4, 3]` has sum `7`.)

---

### ✅ **Time & Space Complexity**
- **Time Complexity:** `O(N)`, because each element is processed at most twice (once when added, once when removed).
- **Space Complexity:** `O(1)`, since we only use a few integer variables.

---

## 🔹 **Sliding Window Vs. Brute Force**
| **Approach** | **Time Complexity** | **Space Complexity** | **Why Efficient?** |
|-------------|--------------------|--------------------|-----------------|
| **Brute Force** | `O(n²)` | `O(1)` | Har possible subarray check karega |
| **Sliding Window** | `O(n)` | `O(1)` | **Dynamic adjustment se unnecessary checks avoid hote hain** |

---

## 🔹 **Key Points to Mention in Interviews**
1️⃣ **Sliding Window Ka Use Case**  
   - Jab **continuous subarray ya substring** pe operation karni ho.  
   - **Fixed ya Variable Size window** problems ke liye best approach hai.  

2️⃣ **Optimality of O(n) Complexity**  
   - Ek hi loop me **window expand aur contract hota hai**, isiliye `O(n²)` se fast hai.  

3️⃣ **Fixed vs. Variable Window Difference**  
   - **Fixed Window:** **Window ka size fixed hota hai** (`k` given hota hai).  
   - **Variable Window:** **Window dynamically badhta/ghatta hai** (`sum ≥ target` jaisa condition hota hai).  

---

## 🔹 **Interview Me Answer Kaise De**
**Q:** `Sliding Window kya hota hai?`
✅ **A:** `"Sliding Window ek efficient approach hai jisme ek dynamically expanding aur contracting window ka use hota hai taaki kisi substring ya subarray par operations fast ho sake."`

**Q:** `Sliding Window Brute Force se better kyu hai?`
✅ **A:** `"Brute force har subarray ko check karta hai, jo `O(n²)` slow hai. Sliding Window dynamically window adjust karta hai jo `O(n)` me fast hota hai."`

---


## 📌 Problem 1: **Minimum Size Subarray Sum (Hinglish Explanation + Dry Run + Text Diagrams)**  

---

## 🔹 **Problem Samajhne Ka Tarika**
Hume ek **integer array `nums[]`** aur ek **target integer** diya hai. Hume **sabse chhoti subarray ka length** batana hai **jiska sum target ya usse bada ho**.

💡 **Constraints:**
- **Subarray continuous hona chahiye.**
- **Agar koi valid subarray nahi mila, toh `0` return karna hai.**
- **Optimized tarika dhoondhna hai, brute-force nahi chalega!**

---

## 🔹 **Example & Text Diagram**  
📌 **Example 1:**  
```java
target = 7, nums = [2, 3, 1, 2, 4, 3]
```
📌 **Output:**  
```
2
```
📌 **Explanation:**  
**[4, 3]** ek **smallest subarray** hai **jiska sum ≥ 7 hai.**  

💡 **Text Diagram (Sliding Window Concept)**:
```
Index:   0   1   2   3   4   5
Nums:    2   3   1   2   4   3
         ↑               ↑   
      (left)          (right)   
        sum = 2+3+1+2+4 = 12 (window too big, try shrinking!)
```

- **Jab tak sum >= target, left badhakar window chhoti karo.**
- **Minimum window track karte jao.**

---

## 🔹 **Brute Force Approach 🐢 (TLE Hoga!)**
Har subarray ka sum nikalna (`O(n²)` complexity).

```java
public int minSubArrayLen(int target, int[] nums) {
    int minLength = Integer.MAX_VALUE;
    
    for (int i = 0; i < nums.length; i++) {
        int sum = 0;
        for (int j = i; j < nums.length; j++) {
            sum += nums[j];
            if (sum >= target) {
                minLength = Math.min(minLength, j - i + 1);
                break;  // No need to check longer subarrays
            }
        }
    }
    
    return (minLength == Integer.MAX_VALUE) ? 0 : minLength;
}
```
⛔ **Problem:** **O(n²) complexity**, bahut slow for large `n` (max `10^5`).

---

## 🔹 **Optimized Approach - Sliding Window 🚀**
**Strategy:**
1. **Left aur Right Pointers Lo:** `left = 0`, `right = 0`
2. **Expand Window Jab Tak Sum < Target**  
3. **Jab Sum ≥ Target Ho Jaye:**  
   - **Minimum Length Update Karo**  
   - **Left Pointer Move Karke Window Chhoti Karo**  
4. **Loop Chalega Jab Tak Right Pointer End Tak Na Pahunch Jaye**

---

## 🔹 **Code Implementation with Hinglish Comments**
```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        
        int left = 0; // Yeh sliding window ka left pointer hai
        int sum = 0; // Yeh current window ka sum store karega
        int minLength = Integer.MAX_VALUE; // Minimum length track karega

        // Right pointer ko array traverse karne do
        for (int right = 0; right < nums.length; right++) {
            sum += nums[right]; // Window ko expand karo

            // Jab tak sum >= target hai, left pointer move karo (window chhoti karo)
            while (sum >= target) {
                minLength = Math.min(minLength, right - left + 1); // Minimum length update karo
                sum -= nums[left]; // Left pointer se element hatao
                left++; // Left pointer ko aage badhao
            }
        }

        // Agar minLength kabhi update nahi hua, toh koi valid subarray nahi mila
        return minLength == Integer.MAX_VALUE ? 0 : minLength;
    }
}
```

---

## 🔹 **Dry Run - Step-by-Step Execution**
📌 **Input:**  
```java
target = 7, nums = [2, 3, 1, 2, 4, 3]
```
📌 **Initialization:**  
```
left = 0, sum = 0, minLength = ∞
```

| **Step** | **right** | **nums[right]** | **sum** | **left** | **minLength** | **Action** |
|---------|---------|-------------|------|------|------|-------------|
| 1 | 0 | 2 | 2 | 0 | ∞ | Expand |
| 2 | 1 | 3 | 5 | 0 | ∞ | Expand |
| 3 | 2 | 1 | 6 | 0 | ∞ | Expand |
| 4 | 3 | 2 | 8 | 0 | ∞ | Shrink |
| 5 | 3 | 2 | 6 | 1 | 4 | Expand |
| 6 | 4 | 4 | 10 | 1 | 4 | Shrink |
| 7 | 4 | 4 | 7 | 2 | 3 | Shrink |
| 8 | 4 | 4 | 6 | 3 | 2 | Expand |
| 9 | 5 | 3 | 9 | 3 | 2 | Shrink |
| ✅ | - | - | 7 | 4 | **2** | ✅ **Final Answer** |

✅ **Final Answer:** `2` (Subarray `[4, 3]`)

---

## 🔹 **Complexity Analysis**
| **Operation** | **Time Complexity** |
|--------------|--------------------|
| **Looping through array** | `O(n)` |
| **Total Complexity** | **O(n) (Efficient)** |

✅ **Best Possible Complexity!** 🚀  

---
## 🔹 **Key Observations**
1. **Why Sliding Window Works?**
   - **Jab tak sum chhota hai, right pointer badhao (Expand Window).**
   - **Jab sum bada ya equal ho jaye, left pointer badhakar window shrink karo.**
   - **Min length ko track karte raho.**

2. **Why Not Use Prefix Sum?**
   - **Binary Search Prefix Sum `O(n log n)` hota hai**, jo sliding window se slow hai.

3. **Two-Pointer Technique ka Fayda**
   - **O(n) complexity** banata hai, jo **O(n²) brute force se bahut fast hai**.

4. **Interview Me Explain Karne Ka Best Tareeka**
   - **Dry run** aur **text diagram** ke saath explain karo.
   - **Sliding Window ka concept acche se batao.**
   - **Minimum length update hone ka logic dikhana zaroori hai.**

---

## 🔹 **Alternative Approaches**
| **Approach** | **Time Complexity** | **Space Complexity** | **Notes** |
|-------------|--------------------|--------------------|------------|
| **Brute Force (Nested Loops)** | `O(n²)` | `O(1)` | Too slow |
| **Binary Search (Prefix Sum)** | `O(n log n)` | `O(n)` | Slower than O(n) |
| **Sliding Window (Two-Pointer)** | `O(n)` | `O(1)` | ✅ Best Approach |

---

## 🔹 **FAQs**
### ❓ **Q1: Kya Brute Force Approach chalega?**
⛔ **Nahi**, kyunki `O(n²)` **TLE dega jab `n` bada hoga**.

### ❓ **Q2: Kya Prefix-Sum + Binary Search better hai?**
✅ **Nahi**, kyunki `O(n log n)` slower hota hai as compared to `O(n)`.

### ❓ **Q3: Kya har case me Sliding Window best hai?**
✅ **Haan**, **lekin array me negative numbers nahi hone chahiye, warna sliding window kaam nahi karega.**

---

## 🔹 **Final Summary**
✅ **Optimal Sliding Window Approach (O(n))**  

---

### 📌 Problem 2: **Longest Substring Without Repeating Characters - Interview Preparation (Hinglish Explanation + Dry Run + Text Diagrams)**  

---

## 🔹 **Problem Samajhne Ka Tarika**
Hume ek **string `s`** di gayi hai, aur hume **sabse lambi substring ka length return karna hai jisme koi character repeat na ho**.

💡 **Constraints:**
- **Substring continuous honi chahiye.**
- **String me sirf lowercase English letters ho sakte hain.**
- **Optimized tarika dhoondhna hai, brute-force nahi chalega!**

---

## 🔹 **Example & Text Diagram**  
📌 **Example 1:**  
```java
Input: s = "abcabcbb"
Output: 3
```
📌 **Explanation:**  
**"abc"** ek **longest substring** hai **jisme koi character repeat nahi ho raha**.  

💡 **Text Diagram (Sliding Window Concept)**:
```
Index:   0   1   2   3   4   5   6   7  
Chars:   a   b   c   a   b   c   b   b  
         ↑   ↑   ↑     
      (start)   
      (end) → Expand  
```
- **Jab tak koi duplicate nahi mile, end pointer badhao.**
- **Jab duplicate mile, start pointer aage badhakar window adjust karo.**
- **Har step pe max length track karte jao.**

---

## 🔹 **Brute Force Approach 🐢 (TLE Hoga!)**
Har substring check karna (`O(n²)` complexity).

```java
public int lengthOfLongestSubstring(String s) {
    int maxLength = 0;
    
    for (int i = 0; i < s.length(); i++) {
        Set<Character> set = new HashSet<>();
        for (int j = i; j < s.length(); j++) {
            if (set.contains(s.charAt(j))) {
                break;
            }
            set.add(s.charAt(j));
            maxLength = Math.max(maxLength, j - i + 1);
        }
    }
    
    return maxLength;
}
```
⛔ **Problem:** **O(n²) complexity**, bahut slow for large `n` (max `10^5`).

---

## 🔹 **Optimized Approach - Sliding Window 🚀**
**Strategy:**
1. **Left aur Right Pointers Lo:** `start = 0`, `end = 0`
2. **Expand Window Jab Tak Koi Duplicate Na Mile**  
3. **Jab Duplicate Mile:**  
   - **Start Pointer Move Karo (Window Shrink Karo)**  
   - **HashMap Use Karke Character Ke Last Index Ko Track Karo**  
4. **Har Step Pe Max Length Update Karo**

---

## 🔹 **Code Implementation with Hinglish Comments**
```java
import java.util.*;

class Solution {
    public int lengthOfLongestSubstring(String s) {
        
        // Har character ka last seen index track karne ke liye HashMap use kar rahe hain
        Map<Character, Integer> map = new HashMap<>();

        // Yeh variable sabse lambi substring ka length store karega
        int maxLength = 0;

        // Yeh pointer current substring ka start track karega
        int start = 0;

        // Right pointer loop chalayega (Sliding Window ka right end)
        for (int end = 0; end < s.length(); end++) {
            char c = s.charAt(end); // Current character uthao

            /*
             - **Agar character pehle aaya hai, toh start ko update karo:**
               - `start = map.get(c) + 1`
               - Iska matlab hai ki ab window ka naya start wo hoga jo pichle repeated character ke just baad hai.
               - `Math.max(start, map.get(c) + 1)` ka use isliye kiya hai kyunki start kabhi piche nahi jaana chahiye.
            */
            if (map.containsKey(c)) {
                start = Math.max(map.get(c) + 1, start);
            }

            // Current character ka index store karo
            map.put(c, end);

            // Maximum substring length update karo
            maxLength = Math.max(maxLength, end - start + 1);
        }

        // Final max length return karo
        return maxLength;
    }
}
```

---

## 🔹 **Dry Run - Step-by-Step Execution**
📌 **Input:**  
```java
s = "abcabcbb"
```
📌 **Initialization:**  
```
start = 0, maxLength = 0
```

| **Step** | **end** | **s[end]** | **Map (char → index)** | **start** | **maxLength** | **Action** |
|---------|--------|---------|----------------|------|------|-------------|
| 1 | 0 | 'a' | `{a → 0}` | 0 | 1 | Expand |
| 2 | 1 | 'b' | `{a → 0, b → 1}` | 0 | 2 | Expand |
| 3 | 2 | 'c' | `{a → 0, b → 1, c → 2}` | 0 | 3 | Expand |
| 4 | 3 | 'a' | `{a → 3, b → 1, c → 2}` | 1 | 3 | Start Moved |
| 5 | 4 | 'b' | `{a → 3, b → 4, c → 2}` | 2 | 3 | Start Moved |
| 6 | 5 | 'c' | `{a → 3, b → 4, c → 5}` | 3 | 3 | Start Moved |
| 7 | 6 | 'b' | `{a → 3, b → 6, c → 5}` | 5 | 3 | Start Moved |
| 8 | 7 | 'b' | `{a → 3, b → 7, c → 5}` | 7 | 3 | Start Moved |

✅ **Final Answer:** `3` (Substring "abc")

---

## 🔹 **Complexity Analysis**
| **Operation** | **Time Complexity** |
|--------------|--------------------|
| **Looping through string** | `O(n)` |
| **Total Complexity** | **O(n) (Efficient)** |

✅ **Best Possible Complexity!** 🚀  

---
## 🔹 **Key Observations**
1. **Why Sliding Window Works?**
   - **Jab tak duplicate nahi mile, right pointer badhao (Expand Window).**
   - **Jab duplicate mile, start pointer aage badhakar window adjust karo.**
   - **Max length ko track karte raho.**

2. **Why Not Use Brute Force?**
   - **Brute force `O(n²)` slow hoga, jabki sliding window `O(n)` fast hai.**

3. **HashMap Ka Kya Role Hai?**
   - **Har character ka last seen index store karne ke liye use ho raha hai.**
   - **Jab duplicate mile, uske last index ka use karke `start` adjust kar rahe hain.**

4. **Interview Me Explain Karne Ka Best Tareeka**
   - **Dry run** aur **text diagram** ke saath explain karo.
   - **Sliding Window ka concept acche se batao.**
   - **Duplicate mile toh start pointer adjust hone ka logic samjhao.**

---

## 🔹 **Alternative Approaches**
| **Approach** | **Time Complexity** | **Space Complexity** | **Notes** |
|-------------|--------------------|--------------------|------------|
| **Brute Force (Nested Loops)** | `O(n²)` | `O(1)` | Too slow |
| **Sliding Window + HashSet** | `O(n)` | `O(26) = O(1)` | Less efficient than HashMap |
| **Sliding Window + HashMap** | `O(n)` | `O(min(m, n))` | ✅ Best Approach |

---

## 🔹 **FAQs**
### ❓ **Q1: Kya Brute Force Approach chalega?**
⛔ **Nahi**, kyunki `O(n²)` **TLE dega jab `n` bada hoga**.

### ❓ **Q2: Kya HashSet use kar sakte hain?**
✅ **Haan**, lekin HashMap se efficient nahi hoga jab start adjust karna pade.

### ❓ **Q3: Kya har case me Sliding Window best hai?**
✅ **Haan**, **kyunki ye `O(n)` me optimal hai.**

---

## 🔹 **Final Summary**
✅ **Optimal Sliding Window Approach (O(n))**  
✅ **Expand Window, Shrink When Duplicate Found**  
✅ **Dry-run aur text diagram se interview me explain karna zaroori**  
✅ **Best solution for "Longest Substring Without Repeating Characters" problem!** 🚀  

💡 **Ye approach aapko interviews me edge degi!** 🔥
✅ **Expand Window, Shrink When Sum ≥ Target**  
✅ **Dry-run aur text diagram se interview me explain karna zaroori**  
✅ **Best solution for "Minimum Size Subarray Sum" problem!** 🚀

---

💡 **Ye approach aapko interviews me edge degi!** 🔥
