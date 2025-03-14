## **🔹 Problem Explanation - Top K Frequent Elements**  
You can view the problem description [here](https://leetcode.com/problems/top-k-frequent-elements/description/).

Given an array `nums[]`, we need to **return the `k` most frequent elements** in the array.

---
## **🔹 Solution Approach**
We use **hash maps and heaps (priority queues)** to efficiently find the `k` most frequent elements.

1. **Step 1:** **Count the frequency of each element** using a **HashMap**.
2. **Step 2:** **Use a Min Heap** (`PriorityQueue`) to keep track of the **top `k` frequent elements**.
3. **Step 3:** **Extract `k` elements from the heap** to return the result.

---
## **🔹 Code with Detailed Hinglish Comments**
```java
import java.util.*;

class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        // Agar k == nums.length hai, toh har element return kar dena hai
        // O(1) time complexity hai kyunki koi processing nahi karni
        if (k == nums.length) {
            return nums;
        }
        
        // **Step 1: Frequency count** (O(N) time)
        // Ek HashMap banao jo har number ka frequency store karega
        Map<Integer, Integer> count = new HashMap<>();
        for (int n : nums) {
            count.put(n, count.getOrDefault(n, 0) + 1);
        }

        // **Step 2: Min Heap use karke Top-K frequent elements track karna** (O(N log k) time)
        // PriorityQueue Min Heap maintain karega jisme **kam se kam frequency wale elements pehle aayenge**
        Queue<Integer> heap = new PriorityQueue<>(
            (n1, n2) -> count.get(n1) - count.get(n2) // Frequency ke basis pe sorting
        );

        // **Heap mein sirf Top-K elements store karna hai**
        for (int n : count.keySet()) {  // HashMap ke har unique element pe iterate karna
            heap.add(n);
            if (heap.size() > k) {
                heap.poll();  // Agar heap ka size k se zyada ho gaya toh sabse kam frequent element hatao
            }
        }

        // **Step 3: Extract K elements from Min Heap** (O(k log k) time)
        int[] top = new int[k];
        for (int i = k - 1; i >= 0; --i) {  // Heap se elements nikalke array mein store karenge
            top[i] = heap.poll();
        }
        
        return top;
    }
}
```
---

## **🔹 Dry Run Example**
```plaintext
Input:
nums = [1,1,1,2,2,3]
k = 2

Step 1: Frequency count (HashMap)
{
  1 → 3
  2 → 2
  3 → 1
}

Step 2: Min Heap to store top-k frequent elements
1. Insert 1 → Heap = [1] ✅
2. Insert 2 → Heap = [2, 1] ✅
3. Insert 3 → Heap = [3, 1, 2] ❌ (Size > k)
   - Remove 3 (Least frequent)
   - Heap = [1, 2]

Step 3: Extract k elements from Heap
Heap: [1, 2]
Output: [1, 2]
```
### **Final Output**
```plaintext
[1, 2]
```

---
## **🔹 Time Complexity Analysis**
### **Step 1: Frequency Count using HashMap**
- **O(N)** time because we iterate over all elements in `nums[]` once.

### **Step 2: Inserting into Min Heap**
- We iterate over **unique elements** in HashMap (**O(N) in the worst case**).
- Insertion/removal in a heap takes **O(log k)** time.
- So, this step takes **O(N log k)** time.

### **Step 3: Extract `k` elements from Heap**
- We extract `k` elements from the heap → **O(k log k)**.

### **Final Time Complexity**
```plaintext
O(N) + O(N log k) + O(k log k) ≈ O(N log k)
```
This is **much better than sorting-based solutions** (O(N log N)).

---
## **🔹 Space Complexity Analysis**
### **Step 1: HashMap**
- Stores **N unique elements** in the worst case → **O(N)**.

### **Step 2: Min Heap**
- Stores **k elements** → **O(k)**.

### **Final Space Complexity**
```plaintext
O(N + k)
```
---
## **🔹 Alternative Approach: Max Heap**
**💡 If we use a Max Heap, how will the complexity change?**
1. **Building a Max Heap from `N` elements** → **O(N)**
2. **Extracting `k` elements** (`k` deletions from heap) → **O(k log N)**

**Final Complexity using Max Heap:**
```plaintext
O(N + k log N)
```
This is slightly **worse than Min Heap** (`O(N log k)`) when `k << N`.

---
## **🔹 Comparison of Approaches**
| Approach | Time Complexity | Space Complexity | Notes |
|----------|---------------|----------------|--------|
| **Sorting (Brute Force)** | O(N log N) | O(N) | Expensive |
| **Min Heap (Priority Queue)** | **O(N log k)** | O(N + k) | Most efficient for small `k` |
| **Max Heap** | O(N + k log N) | O(N) | Better if `k` is large |

---
## **🔹 Summary**
✔ **Min Heap is the best approach** when `k << N`, giving **O(N log k) time complexity**.  
✔ **Max Heap can be used**, but gives **O(N + k log N) complexity**, which is slightly worse.  
✔ **Sorting all elements** is **inefficient** (`O(N log N)`).  
✔ **Best Space Complexity:** `O(N + k)` (because of HashMap & Heap).  

---
## **🔹 Understanding Heap Time Complexity (`O(N log k)`) in Detail**
Heap is a **binary tree-based data structure** where:
- **Insertion and deletion** take **O(log k)** time.
- **Extracting `k` elements** takes **O(k log k)** time.

---
## **🔹 Why is the Time Complexity O(N log k)?**
### **Step 1: Building a Frequency HashMap**
We **iterate over all elements** in `nums[]` to store their frequency in a **HashMap**.

- **Time Complexity:** `O(N)`  
- **Reason:** One traversal of `nums[]`, and inserting/updating in HashMap is `O(1)`.

---
### **Step 2: Maintaining a Min Heap of size `k`**
- We **insert each unique element** (from HashMap) into a **Min Heap**.
- If the heap size **exceeds `k`**, we **remove the smallest frequency element** (heap poll).

💡 **Key Observations:**
1. Each **insertion takes `O(log k)`** (heap maintains sorted order).
2. We **iterate over unique elements (`O(N)`)** in the worst case.
3. **Each insertion and removal operation** (when heap size exceeds `k`) takes **`O(log k)`**.

📌 **Total operations for `N` unique elements:**  
```plaintext
O(N log k)  (because each insert/remove operation is O(log k))
```

---
### **Step 3: Extracting `k` Elements from Heap**
Once the heap contains the **top `k` most frequent elements**, we extract them one by one.

- **Removing `k` elements from Min Heap** takes `O(k log k)`.

---
## **🔹 Step-by-Step Example**
Consider the array:
```plaintext
nums = [1,1,1,2,2,3]
k = 2
```
### **Step 1: Build Frequency HashMap**
```plaintext
1 → 3
2 → 2
3 → 1
```
**Time Complexity:** `O(N)`

---
### **Step 2: Insert Elements into a Min Heap**
We **insert each unique element** into the **Min Heap** (of size at most `k = 2`) based on frequency.

#### **Heap Insertion Process**
1. Insert `1` → Heap = `[1]` ✅
2. Insert `2` → Heap = `[2, 1]` ✅
3. Insert `3` → Heap = `[3, 1, 2]` ❌ (Size > `k` → Remove `3`)  
   **New Heap:** `[1, 2]`

---
### **Heap Tree Representation**
```
Step 1: Insert 1
   1

Step 2: Insert 2
   2
  /
 1

Step 3: Insert 3 → Remove smallest (3)
   2
  /
 1
```
---
### **Step 3: Extract `k` Elements**
1. **Extract `1`** → Heap = `[2]`
2. **Extract `2`** → Heap = `[]`

✔ **Output:** `[1, 2]`  
✔ **Time Complexity of Extraction:** `O(k log k)`

---
## **🔹 Breaking Down Heap Operations**
| Operation | Time Complexity |
|-----------|----------------|
| **Building HashMap** | `O(N)` |
| **Inserting N elements in Heap** | `O(N log k)` |
| **Extracting `k` elements** | `O(k log k)` |
| **Total Time Complexity** | `O(N log k)` |

---
## **🔹 Why `O(N log k)`, Not `O(N log N)`?**
- **If we used a Max Heap:** We would insert all `N` elements, leading to **`O(N log N)`**.
- **Using a Min Heap:** We maintain only `k` elements at a time, reducing **heap operations** to `O(log k)`, giving us **`O(N log k)`** instead.

---
# **Understanding Heap Time Complexity: `O(log k)` for Insertion/Deletion and `O(k log k)` for Extraction**

## **🔹 What is a Heap?**
A **heap** is a **binary tree-based data structure** that satisfies the **heap property**:
- **Min Heap:** The parent node is **smaller** than its children.
- **Max Heap:** The parent node is **larger** than its children.

💡 **Key Property of Heaps:**  
- **Balanced Binary Tree** → Ensures `O(log k)` insertions & deletions.
- **Complete Tree** → Always fills left to right.

---

## **🔹 Why Does Insertion and Deletion Take `O(log k)`?**
### **1️⃣ Heap is a Binary Tree with Height `O(log k)`**
A heap is a **binary tree**, and in a **binary tree of size `k`**, the height is:

```plaintext
Height of binary tree = O(log k)
```
Since we always maintain a **balanced** structure, each operation **(insert/delete)** only traverses at most **O(log k) levels**.

---
### **2️⃣ Heap Insertion (`O(log k)`) - "Bubble Up"**
**How insertion works in a heap:**
1. Insert the new element **at the next available spot (bottom right)** to maintain completeness.
2. Compare it with its **parent node**.
3. **If the heap property is violated, swap with the parent**.
4. **Repeat until the heap property is restored**.

📌 **Example - Inserting into a Min Heap**
```plaintext
Before inserting 1:
       2
      / \
     5   6
    /
   8

Insert 1:
       2
      / \
     5   6
    /
   8
  /
 1  (New Element)
```
**Now, Bubble Up (Swap 1 & 8, then 1 & 2)**:
```plaintext
       1
      / \
     2   6
    /
   5
  /
 8
```
🔹 **Each swap moves up one level (log k levels in the worst case).**
🔹 **Time Complexity:** `O(log k)`

---
### **3️⃣ Heap Deletion (`O(log k)`) - "Bubble Down"**
**How deletion works in a heap:**
1. **Remove the root node** (smallest/largest element in Min/Max Heap).
2. **Replace the root with the last element** (to maintain completeness).
3. **Compare with children and swap with the smaller (Min Heap) / larger (Max Heap) child**.
4. **Repeat until the heap property is restored**.

📌 **Example - Removing the Root in a Min Heap**
```plaintext
Before deleting root:
       1
      / \
     2   6
    /
   5
  /
 8

Step 1: Remove 1 and replace with last element (8)
       8
      / \
     2   6
    /
   5

Step 2: Bubble Down (Swap 8 & 2)
       2
      / \
     8   6
    /
   5

Step 3: Bubble Down Again (Swap 8 & 5)
       2
      / \
     5   6
    /
   8
```
🔹 **Each swap moves down one level (`log k` levels in the worst case).**  
🔹 **Time Complexity:** `O(log k)`

---

## **🔹 Why Extracting `k` Elements Takes `O(k log k)`?**
Extracting `k` elements means **repeating the deletion process `k` times**.

### **Step-by-Step Process**
1. **Each extraction (heap poll) takes `O(log k)`.**
2. **Since we extract `k` elements, total complexity = `O(k log k)`.**

📌 **Example - Extracting Top `k` Elements**
Assume we have a Min Heap:

```plaintext
       2
      / \
     5   6
    /
   8
```
### **Extracting 3 Elements (k = 3)**
| Step | Heap (Before) | Extracted | Heap (After) |
|------|-------------|-----------|-------------|
| **1** | `[2, 5, 6, 8]` | `2` | `[5, 8, 6]`  |
| **2** | `[5, 8, 6]` | `5` | `[6, 8]` |
| **3** | `[6, 8]` | `6` | `[8]` |

🔹 **Each extraction takes `O(log k)`.**  
🔹 **Total extractions = `k` times.**  
🔹 **Final Complexity = `O(k log k)`.**

---

## **🔹 Final Time Complexity Breakdown**
| Operation | Time Complexity | Explanation |
|-----------|---------------|-------------|
| **Insertion** | `O(log k)` | Elements "bubble up" in a balanced binary heap (height `log k`) |
| **Deletion (Poll)** | `O(log k)` | Elements "bubble down" in a balanced binary heap |
| **Extracting `k` elements** | `O(k log k)` | `k` deletions, each taking `O(log k)` |
| **Building Heap (Heapify)** | `O(N log k)` | Inserting `N` elements into a heap of size `k` |
| **Final Complexity** | **`O(N log k)`** | Best for finding top `k` frequent elements |

---

## **🔹 Summary**
✔ **Insertion and deletion in a heap take `O(log k)` because the tree height is `O(log k)`.**  
✔ **Extracting `k` elements takes `O(k log k)` because each extraction takes `O(log k)`.**  
✔ **Maintaining a heap of size `k` (not `N`) reduces complexity to `O(N log k)`, making it more efficient than sorting (`O(N log N)`).**  

---
