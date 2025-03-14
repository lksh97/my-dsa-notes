## **🔹 Java Data Structures: `String` vs `List` vs `StringBuilder` vs `Array` (With Methods and Conversions)**

### **🔹 Interview Scenario:**
Agar **data structures ka proper knowledge nahi ho**, toh interview me pucha ja sakta hai:
- `"String, List aur StringBuilder me kya difference hai?"`
- `"Array ko directly `ArrayList` me convert kyun nahi kar sakte?"`
- `"Kaunse methods kaha available hain?"`
- `"Kaha mutable aur immutable behavior important hota hai?"`

---
# **🔹 String vs List vs StringBuilder vs Array - Feature Comparison**
| Feature | **String** | **Array (`T[]`)** | **List (`ArrayList<T>` or `LinkedList<T>`)** | **StringBuilder** |
|---------|-----------|-------------------|---------------------------------|-----------------|
| **Mutability** | **Immutable** | **Fixed Size** | **Dynamic Size (Resizable)** | **Mutable (Efficient for modification)** |
| **Size Modification** | ❌ No (Fixed) | ❌ No (Fixed) | ✅ Yes (`add()`, `remove()`) | ✅ Yes (`append()`, `insert()`) |
| **Performance** | ❌ Slow (New object on modification) | ✅ Fast Access (O(1)) | ❌ Slower Access (O(n)) | ✅ Fast (`O(1)` append) |
| **Methods Available** | **Limited (`charAt()`, `concat()`, `split()`)** | ❌ Only length (`arr.length`) | ✅ Rich methods (`add()`, `remove()`, `contains()`) | ✅ Efficient modifications (`append()`, `replace()`) |
| **Usage** | **Immutable text data** | **Fast lookup, static-sized data** | **Dynamic collections** | **Efficient string manipulation** |
| **Memory Usage** | ❌ High (New object on change) | ✅ Compact | ❌ Higher (Extra space for resizing) | ✅ Efficient |

---

# **🔹 Method Comparison (With Examples)**
## **🔹 1️⃣ String Methods**
| Method | Description |
|--------|-------------|
| `charAt(i)` | Get character at index `i` |
| `concat(str)` | Concatenate two strings |
| `split(delimiter)` | Split string into array |
| `substring(i, j)` | Extract substring from index `i` to `j` |
| `replace(old, new)` | Replace substring |
| `equals(str)` | Compare two strings |
| `toLowerCase()` / `toUpperCase()` | Convert case |

```java
String s = "hello";
System.out.println(s.charAt(1));  // Output: e
System.out.println(s.concat(" world"));  // Output: hello world
System.out.println(Arrays.toString(s.split("e")));  // Output: [h, llo]
```
---
## **🔹 2️⃣ List (`ArrayList`) Methods**
| Method | Description |
|--------|-------------|
| `add(value)` | Add element |
| `get(index)` | Get element |
| `set(index, value)` | Set element at index |
| `remove(index)` | Remove element |
| `contains(value)` | Check if element exists |
| `size()` | Get size of list |

```java
List<String> list = new ArrayList<>();
list.add("apple");
list.add("banana");
System.out.println(list.get(1));  // Output: banana
list.remove("banana");
System.out.println(list.contains("banana"));  // Output: false
```
---
## **🔹 3️⃣ StringBuilder Methods**
| Method | Description |
|--------|-------------|
| `append(str)` | Append a string |
| `insert(i, str)` | Insert at index |
| `replace(i, j, str)` | Replace substring |
| `reverse()` | Reverse string |
| `delete(i, j)` | Delete substring |
| `toString()` | Convert to `String` |

```java
StringBuilder sb = new StringBuilder("hello");
sb.append(" world");
sb.insert(5, ",");
System.out.println(sb.toString());  // Output: hello, world
```
---
## **🔹 4️⃣ Array Methods**
| Method | Description |
|--------|-------------|
| `arr.length` | Get size |
| `Arrays.sort(arr)` | Sort array |
| `Arrays.binarySearch(arr, key)` | Search element |
| `Arrays.toString(arr)` | Convert to string |
| `System.arraycopy(src, srcPos, dest, destPos, length)` | Copy array |

```java
int[] arr = {5, 2, 9};
Arrays.sort(arr);
System.out.println(Arrays.toString(arr));  // Output: [2, 5, 9]
```
---

# **🔹 Can We Convert an Array to `ArrayList` Directly?**
💡 **Nahi! Direct conversion `ArrayList` nahi bana sakti, pehle `Arrays.asList()` use karna padta hai.**

### **🚨 Wrong Approach (Does Not Work)**
```java
int[] arr = {1, 2, 3};
List<Integer> list = new ArrayList<>(arr);  // ❌ Compilation Error!
```
**Reason:**  
- `ArrayList` ka constructor kisi `array` ko accept **nahi** karta.
- `Arrays.asList(arr)` bhi **primitive arrays ke liye kaam nahi karega**.

---

### **✅ Correct Approach (Primitive Array)**
```java
int[] arr = {1, 2, 3};
List<Integer> list = new ArrayList<>();
for (int num : arr) {
    list.add(num);
}
System.out.println(list);  // Output: [1, 2, 3]
```
---

### **✅ Correct Approach (`Arrays.asList()` for Objects)**
```java
String[] arr = {"apple", "banana"};
List<String> list = new ArrayList<>(Arrays.asList(arr));  // ✅ Works fine
System.out.println(list);  // Output: [apple, banana]
```
**`Arrays.asList()` tabhi kaam karega jab array me objects hon (`String`, `Integer`), primitive arrays ke liye nahi!**

---

## **🔹 Interview-Ready Explanation**
### ✅ **Q: `String`, `List`, `StringBuilder`, aur `Array` me kya difference hai?**
💡 **A:**  
- **`String` Immutable hota hai** → Har modification pe naya object banta hai.  
- **`StringBuilder` Mutable hota hai** → Efficient hai jab multiple modifications karni ho.  
- **`Array` Fixed-size hota hai** → Fast access but size modify nahi hota.  
- **`ArrayList` Dynamic size hota hai** → More flexible, but access slower ho sakta hai.

---
### ✅ **Q: `Array` ko `ArrayList` me directly convert kyun nahi kar sakte?**
💡 **A:**  
- **Primitive arrays ko `ArrayList` me directly pass nahi kar sakte.**
- **`Arrays.asList(arr)` fixed-size list return karta hai**, jo `ArrayList` jaisa behave nahi karta.
- **Mutable `ArrayList` banane ke liye `new ArrayList<>(Arrays.asList(arr))` use karna chahiye.**

---
### ✅ **Q: `ArrayList` vs `LinkedList` difference?**
💡 **A:**  
- **`ArrayList` index-based fast access deta hai (`O(1)`), but insert/delete slow hota hai (`O(n)`).**  
- **`LinkedList` insertion/deletion fast hota hai (`O(1)`), but access slow hota hai (`O(n)`).**  


## **🔹 Problem 1: - Top K Frequent Elements**  
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

## **🔹 Problem 2 : Solution Deep Dive - Encode and Decode Strings (LeetCode 271)**  
LeetCode Link: [Encode and Decode Strings](https://leetcode.com/problems/encode-and-decode-strings/)

---

## **🔹 Problem Statement**
Hume ek **list of strings** ko **ek single string** me convert karna hai (**encoding**) aur phir **wapas list me laana hai** (**decoding**), taaki **koi bhi data loss na ho**.

**Challenges:**  
- **Direct joining of strings fail karega** agar koi string already `,` ya `|` jaisa separator contain karti ho.  
- **Strings me spaces ya newlines ho sakti hain**, jo split operation ko tod sakti hain.  
- **Efficient encode aur decode hona chahiye**, bina extra parsing overhead ke.

---

## **🔹 Code with Hinglish Comments**
```java
import java.util.*;

public class Codec {

    // 🔹 Encodes a list of strings to a single string.
    public String encode(List<String> strs) {
        
        // 🛑 Edge Case: Agar list empty hai, ek special character return karenge
        if (strs.size() == 0) {
            return Character.toString((char)258); // Special character jo normally strings me nahi hota
        }

        // 🔹 Use `257` as separator because ye ek rare Unicode character hai
        String separate = Character.toString((char)257);

        // 🔹 StringBuilder use kar rahe hain taaki efficiently string build ho
        StringBuilder sb = new StringBuilder();
        for (String s : strs) {
            sb.append(s);        // String ko append karo
            sb.append(separate); // Separator lagao taaki decode kar sakein
        }

        // 🔹 Last separator remove karne ke liye `.deleteCharAt` use kiya
        sb.deleteCharAt(sb.length() - 1);

        return sb.toString();
    }

    // 🔹 Decodes a single string back to a list of strings.
    public List<String> decode(String s) {

        // 🛑 Edge Case: Agar input encoded string special character hai, empty list return karenge
        if (s.equals(Character.toString((char)258))) {
            return new ArrayList<>();
        }

        // 🔹 `257` ko separator ke roop me use kar rahe hain
        String separate = Character.toString((char)257);

        // 🔹 String ko split karke list me convert kar diya
        return Arrays.asList(s.split(separate, -1));
    }
}
```

---

## **🔹 Edge Cases Handled**
### ✅ **Case 1: Empty List**
```java
List<String> strs = new ArrayList<>();
String encoded = codec.encode(strs);  // Output: Special char "258"
List<String> decoded = codec.decode(encoded);  // Output: []
```
---
### ✅ **Case 2: Normal Strings**
```java
List<String> strs = Arrays.asList("hello", "world");
String encoded = codec.encode(strs);   // Output: "hello␁world" (using `257` as separator)
List<String> decoded = codec.decode(encoded);  // Output: ["hello", "world"]
```
---
### ✅ **Case 3: Strings With Separator Character**
```java
List<String> strs = Arrays.asList("apple", "banana␁mango", "cherry");
String encoded = codec.encode(strs);   // Output: "apple␁banana␁mango␁cherry"
List<String> decoded = codec.decode(encoded);  // Output: ["apple", "banana␁mango", "cherry"]
```
---
### ✅ **Case 4: Strings With Spaces & New Lines**
```java
List<String> strs = Arrays.asList("line1\nline2", "  spaces  ", "special!@#");
String encoded = codec.encode(strs);
List<String> decoded = codec.decode(encoded); 
```
🚀 **No data loss, spaces & new lines are handled properly!**

---

## **🔹 Explanation of Minor Details**
### 🔹 **1. `Character.toString((char)258)` - Special Character for Edge Case**
```java
Character.toString((char)258);
```
- **`(char)258`** ek Unicode character hai jo **normally kisi bhi normal string me nahi hota**.
- **Iska use** `"empty list"` ko encode karne ke liye ho raha hai taaki decode karte time identify ho sake.

---
### 🔹 **2. `String separate = Character.toString((char)257);` - Unique Separator**
- Normal delimiters like `,` ya `|` problematic ho sakte hain **agar strings ke andar already exist karein**.
- `257` ek **rare Unicode character hai jo normal user input me nahi aata**, isliye safe hai.

---
### 🔹 **3. `StringBuilder sb = new StringBuilder();` - Why Use StringBuilder?**
- **`String` immutable hota hai**, agar hum directly `s1 + s2 + s3` karein to **nayi string create hoti hai**, jo inefficient hai.
- **`StringBuilder` mutable hai** aur **`append()` O(1) me kaam karta hai**, isliye efficient hai.

---
### 🔹 **4. `sb.deleteCharAt(sb.length() - 1);` - Removing Last Separator**
```java
sb.deleteCharAt(sb.length() - 1);
```
- Last separator `"␁"` extra add ho jata hai, usko **remove karne ke liye** `.deleteCharAt()` use kiya.

---
### 🔹 **5. `s.split(separate, -1)` - Why Use `-1` in `split()`?**
```java
s.split(separate, -1);
```
- Java ka **default `split()` trailing empty strings remove kar deta hai**, jo **loss of data cause kar sakta hai**.
- **`-1` flag ensure karta hai ki sabhi empty strings bhi preserve ho.**

---

## **🔹 Time & Space Complexity Analysis**
| Step | Operation | Time Complexity | Space Complexity |
|------|-----------|----------------|------------------|
| **Encoding** | **Join all strings** | **O(N)** | **O(N)** |
| **Decoding** | **Split the string** | **O(N)** | **O(N)** |
| **Overall Complexity** | **O(N)** | **O(N)** |

✔ **Efficient solution!** Only `O(N)` time and space are required.

---

## **🔹 Alternative Approach - Length Prefix Encoding**
💡 **Instead of a separator, we can use a length prefix before each string!**
### **Example**
```plaintext
Input: ["hello", "world"]
Encoded: "5:hello5:world"
```
✔ **Handles all edge cases**  
❌ **Parsing is complex** compared to separator-based approach.

---

## **🔹 Final Summary**
✅ **Minimally encoded string using rare character separator (`257`)**  
✅ **Handles empty lists, spaces, newlines, and special characters**  
✅ **Uses efficient `StringBuilder` and `split()` handling**  
✅ **Time Complexity: O(N), Space Complexity: O(N)**  

🔥 **Ab tum ready ho is solution ko confidently interview me explain karne ke liye!** 🚀

----


## **🔹 Understanding `s.split(separate, -1)` in Detail for Interviews**

### **🛑 Problem with Default `split()` Behavior**
In Java, when we use `s.split(delimiter)`, the default behavior is:

- If the **delimiter is at the end**, Java **removes trailing empty strings**.
- This can **cause data loss** when encoding/decoding lists of strings.

---
## **🔹 Example Showing Default Behavior (`split()` Without `-1`)**
```java
String s = "apple␁banana␁cherry␁";  // Trailing separator at end
String[] parts = s.split("␁");

System.out.println(Arrays.toString(parts));
```
### **🔹 Expected Output (If Java Preserved Empty Strings)**
```plaintext
["apple", "banana", "cherry", ""]
```
### **❌ Actual Output Without `-1`**
```plaintext
["apple", "banana", "cherry"]
```
**Java automatically removes the last empty string**, which means that if we encoded an empty string at the end, it would be lost after decoding!

---

## **🔹 Why We Use `-1` in `split()`**
To **preserve all empty strings**, we use:
```java
s.split(separate, -1);
```
- The **`-1` flag tells Java**:  
  **"Don't remove trailing empty strings, keep everything!"**  
- This ensures that our **decode operation reconstructs the exact original list of strings**.

---

## **🔹 Corrected Example (`split()` With `-1`)**
```java
String s = "apple␁banana␁cherry␁";  // Trailing separator at end
String[] parts = s.split("␁", -1);

System.out.println(Arrays.toString(parts));
```
### **✅ Correct Output**
```plaintext
["apple", "banana", "cherry", ""]
```
✅ **Now the last empty string is preserved!**

---

## **🔹 Why This Matters for Encode-Decode**
### **Case: If Original List Contains an Empty String**
```java
List<String> strs = Arrays.asList("apple", "banana", "", "cherry");
String encoded = codec.encode(strs);
List<String> decoded = codec.decode(encoded);
```
- If we **don't use `-1`**, the empty string will be **lost** during decoding!
- If we **use `-1`**, the empty string is **properly preserved**.

---

## **🔹 Interview-Ready Explanation**
🔹 **Q: Why do we use `split(separate, -1)` instead of `split(separate)`?**  
💡 **A:** Java’s default `split()` removes trailing empty strings, which causes data loss when decoding.  
Using `-1` **ensures that all empty strings are preserved**, so our encoded string can be reconstructed exactly.  

🔹 **Q: What happens if we don’t use `-1`?**  
💡 **A:** If a string had an empty value at the end (`["apple", "banana", ""]`), it would be lost during decoding, leading to an incorrect result.  

---

## **🔹 Summary**
✔ **Default `split()` removes trailing empty values (causes data loss).**  
✔ **Using `split(separate, -1)` ensures that all parts of the string are preserved.**  
✔ **Essential for encoding-decoding problems to maintain data integrity.**  

🔥 **Now you can confidently explain this in interviews!** 🚀

---

## **🔹 Java Methods Deep Dive: `Arrays.asList()`, `s.split()`, and `s.equals()` vs `==` in Strings**
Interview **mein yeh basic cheezein clarity ke sath explain karna zaroori hota hai** because these are commonly used in coding problems.

---

# **🔹 `Arrays.asList()`**
### **🛑 Problem it Solves**
Jab hume kisi **array ko List me convert karna hota hai**, tab hum **`Arrays.asList()`** ka use karte hain.

### **✅ Syntax**
```java
List<String> list = Arrays.asList("apple", "banana", "cherry");
```
💡 **Yeh internally ek `Fixed-Size` List return karta hai!**

---

## **🔹 Example of `Arrays.asList()`**
```java
String[] arr = {"apple", "banana", "cherry"};
List<String> list = Arrays.asList(arr);

System.out.println(list);  // Output: [apple, banana, cherry]
```

---

## **🛑 Issue with `Arrays.asList()`**
🚨 **Problem:** `Arrays.asList()` se jo List banta hai, **mutable nahi hota (fixed-size hota hai)**.  
Agar hum `list.add("mango")` karne ki koshish karein to **`UnsupportedOperationException`** aayega.

```java
List<String> list = Arrays.asList("apple", "banana");
list.add("mango");  // ❌ Runtime Exception: UnsupportedOperationException
```
✅ **Solution:** Agar dynamic list chahiye, to **`new ArrayList<>(Arrays.asList(arr))`** use karo.
```java
List<String> list = new ArrayList<>(Arrays.asList("apple", "banana"));
list.add("mango");  // ✅ Works fine
```

---

# **🔹 `s.split()`**
### **🛑 Problem it Solves**
Jab **string ko multiple parts me todna hota hai** based on some delimiter, tab **`split()`** use hota hai.

### **✅ Syntax**
```java
String[] parts = s.split(delimiter);
```
---
## **🔹 Example of `s.split()`**
```java
String s = "apple,banana,cherry";
String[] parts = s.split(",");

System.out.println(Arrays.toString(parts));  // Output: [apple, banana, cherry]
```
---
## **🛑 Issue with `s.split()`**
🚨 **Problem:** Agar delimiter last me ho to **default split empty string ko hata deta hai!**
```java
String s = "apple,banana,cherry,";  
String[] parts = s.split(",");

System.out.println(Arrays.toString(parts));  // Output: [apple, banana, cherry] ❌ (Last empty part missing)
```
✅ **Solution:** `split(delimiter, -1)` use karo to **sabhi empty parts preserve ho**.
```java
String[] parts = s.split(",", -1);
System.out.println(Arrays.toString(parts));  // Output: [apple, banana, cherry, ""] ✅ (Last empty part preserved)
```

---
# **🔹 `s.equals()` vs `==` in Strings**
## **1️⃣ `s.equals(t)` (Content Comparison)**
✅ `equals()` **content compare karta hai, reference nahi**.

```java
String s1 = new String("apple");
String s2 = new String("apple");

System.out.println(s1.equals(s2));  // ✅ Output: true  (Content same hai)
```

---
## **2️⃣ `s1 == s2` (Reference Comparison)**
🚨 `==` **reference compare karta hai, content nahi**.

```java
String s1 = new String("apple");
String s2 = new String("apple");

System.out.println(s1 == s2);  // ❌ Output: false (Alag-alag objects hain)
```
---
## **3️⃣ `String Interning` (Optimization)**
Java **string literals ko memory optimize karta hai** using **String Pool**.
```java
String s1 = "apple";
String s2 = "apple";

System.out.println(s1 == s2);  // ✅ Output: true (Same reference because of String Pool)
```

---
# **🔹 Hamare Use Case me Kya Ho Raha hai?**
### **`Arrays.asList(s.split(separate, -1))`**
- **`s.split(separate, -1)`** string ko **separator ke basis pe tod raha hai**, aur **sabhi empty values preserve kar raha hai**.
- **`Arrays.asList()`** **inhe ek `Fixed-Size List` me convert kar raha hai**.
- **Final List immutable hoti hai**.

---
### **✅ Correct Way (To Handle Mutability)**
```java
List<String> list = new ArrayList<>(Arrays.asList(s.split(separate, -1)));
```
🔥 **Ab hum `list.add()`, `list.remove()` properly use kar sakte hain!** 🔥

---
# **🔹 Interview-Ready Summary**
| **Concept** | **Key Takeaway** |
|------------|----------------|
| `Arrays.asList()` | **Fixed-size list banata hai**, `add()` aur `remove()` nahi chalega. |
| `s.split()` | **String todne ke liye use hota hai**, lekin default trailing empty strings remove kar deta hai. |
| `s.split(separate, -1)` | **Sabhi empty strings preserve karta hai**, jo encoding-decoding ke liye zaroori hai. |



-----

# **🔹  Problem 3: Longest Consecutive Sequence - Deep Explanation for Interview**
Yeh problem **unsorted array me longest consecutive sequence** dhoondhne ke liye hai.  
Aur hume **O(N) time complexity** me solution likhna hai.

---

## **🔹 Problem Understanding**
### **✅ Problem Statement**
Given an **unsorted array**, hume **longest consecutive sequence ka length** return karna hai.

- **Consecutive sequence** → Ek aisa sequence jisme **back-to-back numbers** ho.
- **Duplicate elements ignore karne hai.**
- **Sorting allowed nahi hai** kyunki **O(N log N) se fast hona chahiye.**

---

## **🔹 Example Walkthrough**
### **Example 1**
```plaintext
Input: nums = [100,4,200,1,3,2]
```
🔹 **Sorted View** (just for clarity): `[1, 2, 3, 4, 100, 200]`

✅ **Longest consecutive sequence** = `[1, 2, 3, 4]` → **Length = 4**  

```plaintext
Output: 4
```
---
### **Example 2**
```plaintext
Input: nums = [0,3,7,2,5,8,4,6,0,1]
```
🔹 **Sorted View**: `[0, 1, 2, 3, 4, 5, 6, 7, 8]`

✅ **Longest consecutive sequence** = `[0, 1, 2, 3, 4, 5, 6, 7, 8]` → **Length = 9**  

```plaintext
Output: 9
```
---
### **Example 3**
```plaintext
Input: nums = [1,0,1,2]
```
🔹 **Sorted View**: `[0, 1, 1, 2]`

✅ **Longest consecutive sequence** = `[0, 1, 2]` → **Length = 3**  

```plaintext
Output: 3
```

---

## **🔹 Approach: Using HashSet**
### **⚡ Why HashSet?**
- **Fast Lookup (O(1))**: Har number ko **quickly check kar sakte hain** HashSet me.
- **Duplicate Handling**: HashSet **automatically duplicate elements hata deta hai**.
- **Sorting Avoid**: Sorting nahi use karna, kyunki **O(N log N)** se fast solution chahiye.

---

## **🔹 Dry Run (With Text Diagram)**
### **Example: `nums = [100,4,200,1,3,2]`**
### **Step 1: HashSet me store karo**
```plaintext
numSet = {100, 4, 200, 1, 3, 2}
```

---
### **Step 2: Consecutive Sequence Find Karo**
Har number ke liye **ye check karo ki kya ye sequence ka starting point ho sakta hai?**  
**Start condition:** Agar `num - 1` **set me nahi hai**, to iska matlab ye ek **naya sequence ka start ho sakta hai**.

---
| **Number** | **num - 1 Exists?** | **Sequence Start?** | **Consecutive Length** |
|-----------|---------------------|---------------------|-------------------|
| `100` | ❌ `99` nahi hai | ✅ **New Sequence Start** | `100` **length = 1** |
| `4` | ✅ `3` exists | ❌ Skip |
| `200` | ❌ `199` nahi hai | ✅ **New Sequence Start** | `200` **length = 1** |
| `1` | ❌ `0` nahi hai | ✅ **New Sequence Start** | `[1 → 2 → 3 → 4]` **length = 4** |
| `3` | ✅ `2` exists | ❌ Skip |
| `2` | ✅ `1` exists | ❌ Skip |

**✅ Maximum Consecutive Length = `4` (from [1,2,3,4])**

```plaintext
Output: 4
```

---

## **🔹 Code with Hinglish Comments**
```java
import java.util.HashSet;

class Solution {
    public int longestConsecutive(int[] nums) {
        // ✅ Step 1: HashSet me saare elements daal do (Duplicate handle ho jayenge)
        HashSet<Integer> numSet = new HashSet<>();
        for (int num : nums) {
            numSet.add(num);
        }

        int longestStreak = 0;  // ✅ Final longest sequence ka length store karne ke liye

        // ✅ Step 2: Iterate through set and find longest sequence
        for (int num : numSet) {
            // ❌ Agar num-1 set me hai, to yeh middle ka element hai, isko ignore karo
            if (!numSet.contains(num - 1)) {
                int currNum = num;      // ✅ Current sequence ka starting point
                int currStreak = 1;     // ✅ Streak length ko 1 se start karo

                // ✅ Jab tak `currNum+1` set me hai, sequence continue karo
                while (numSet.contains(currNum + 1)) {
                    currNum++;   // ✅ Next number check karo
                    currStreak++;  // ✅ Streak badhaya
                }

                // ✅ Maximum streak ko update karo
                longestStreak = Math.max(longestStreak, currStreak);
            }
        }

        return longestStreak;  // ✅ Final answer return karo
    }
}
```

---

## **🔹 Time & Space Complexity Analysis**
| **Operation** | **Complexity** | **Explanation** |
|--------------|--------------|--------------|
| **Insert in HashSet** | `O(N)` | `N` elements HashSet me daal rahe hain |
| **Iterate over HashSet** | `O(N)` | Har element ko ek hi baar check karenge |
| **Total Complexity** | **`O(N)`** | **Optimized solution** |
| **Space Complexity** | `O(N)` | HashSet me `N` elements store ho rahe hain |

✅ **Final Complexity = `O(N)` time & `O(N)` space (HashSet)**

---

## **🔹 Interview-Ready Explanation**
### ✅ **Q1: Tumhara approach kya hai?**
💡 **A:**  
- **HashSet me saare numbers store kar rahe hain** (taaki lookup fast ho).  
- **Har number ke liye check kar rahe hain** ki kya **ye ek new sequence ka start ho sakta hai**.  
- **Agar sequence start ho sakta hai**, to **usko expand kar rahe hain jab tak numbers mil rahe hain**.  

---
### ✅ **Q2: `num - 1` check karne ka kya logic hai?**
💡 **A:**  
Agar **`num - 1` HashSet me hai**, iska matlab **ye kisi existing sequence ka part hai**.  
Agar **`num - 1` nahi hai**, iska matlab **ye ek naye sequence ka starting point ho sakta hai**.

---
### ✅ **Q3: Sorting se kyu nahi kiya?**
💡 **A:**  
- **Sorting `O(N log N)` time leti** hai, jo **O(N) se slow hai**.  
- **HashSet fast lookup deta hai (`O(1)`)**, jo **better approach hai**.  

---
### ✅ **Q4: Edge Cases kaise handle kiye?**
💡 **A:**  
- **Empty Array (`[]`) → Output = 0**
- **Single Element ([5]) → Output = 1**
- **Duplicate Elements ([1,2,2,3]) → Ignore duplicates**
- **Negative Numbers ([-1,-2,0,1]) → Works Fine**

---
## **🔹 Final Summary**
✔ **Best approach: HashSet + Linear Scan**  
✔ **`O(N)` Time Complexity, `O(N)` Space Complexity**  
✔ **Fastest solution without sorting**  
✔ **Interview me confidently explain kar sakte ho!** 🚀🔥  

---
🔥 **Ab tum ye problem interview me confidently explain kar sakte ho! 🚀💪**
| `s.equals(t)` | **Content compare karta hai, reference nahi.** |
| `s1 == s2` | **Reference compare karta hai, content nahi.** |

---
🚀 **Ab tum confident ho `Arrays.asList()`, `s.split()`, aur `s.equals()` vs `==` explain karne ke liye!** 🚀
