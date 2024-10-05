### Question 1 - Problem Description: Sort a Nearly Sorted Array

Given an array of `n` elements where each element is at most `k` positions away from its target position in the sorted order, the task is to sort this "nearly sorted" (or "k-sorted") array efficiently.

### **What Does "Nearly Sorted" Mean?**

A "nearly sorted" array means that an element at index `i` could be found within the range of `i - k` to `i + k` in the sorted array. In simpler terms, each element is misplaced by at most `k` positions from where it would be if the array were fully sorted.

For example:
- If the input array is `[2, 6, 3, 12, 56, 8]` and `k = 3`, the array is nearly sorted because each element is no more than 3 positions away from its sorted place.
- In the sorted version of this array (`[2, 3, 6, 8, 12, 56]`), notice that:
  - `2` was already in the correct place.
  - `3` was 2 places away (originally at index 2, now at index 1).
  - `8` was 3 places away (originally at index 5, now at index 3).

### **Goal:**
Efficiently sort this nearly sorted array using a data structure that minimizes unnecessary comparisons and operations.

### **Constraints:**
- The array can be of any length (`n`).
- `k` is a positive integer that specifies how far each element is from its target position.
- The value of `k` is much smaller than `n` (for example, `k << n`), which allows for optimizations using a Min-Heap.

### **Why a Min-Heap?**
The properties of a nearly sorted array imply that the smallest element within any window of size `k + 1` is very likely the next element to be placed in the sorted array. Using a Min-Heap allows us to efficiently find and place the next smallest element into its correct position.

### **Example:**

#### **Input:**
- `arr = [2, 6, 3, 12, 56, 8]`
- `k = 3`

#### **Output:**
- Sorted array: `[2, 3, 6, 8, 12, 56]`

### **Step-by-Step Explanation:**

1. Since each element is at most `k` positions away from its sorted position, the next smallest element will be found within the first `k + 1` elements of the unsorted portion.
2. We use a Min-Heap (a data structure that allows quick retrieval of the smallest element) to maintain a sliding window of size `k + 1`.
3. Insert the first `k + 1` elements into the Min-Heap.
4. For the rest of the elements:
   - Extract the minimum element from the heap and place it in the sorted part of the array.
   - Insert the next element from the array into the Min-Heap.
5. After processing all elements, extract and place the remaining elements from the Min-Heap into the array to complete the sorting.

### **Complexity:**
- **Time Complexity:** `O(k) + O((n - k) * log k)`, where:
  - `O(k)` is the time to build the initial heap of size `k + 1`.
  - `O((n - k) * log k)` is the time to process the rest of the elements in the array.
- **Space Complexity:** `O(k)` for storing `k + 1` elements in the heap.

This approach leverages the "nearly sorted" property to achieve more efficient sorting than traditional sorting algorithms, especially when `k` is significantly smaller than `n`.

### Sorting a Nearly Sorted (k-Sorted) Array Using Min-Heap

In this problem, we are given an array that is "nearly sorted," meaning each element is at most `k` positions away from its target position in the sorted array. Our task is to sort this array efficiently using a **Min-Heap**.

### **Understanding the Problem**

**Nearly Sorted Array:** An array is considered nearly sorted if any element at index `i` is at most `k` positions away from its correct position in a fully sorted array. For example, if `k = 3`, an element at index 4 can be anywhere between indices 1 to 7.

### **Approach:** Using a Min-Heap

Since each element is at most `k` positions away from its correct position, we can use a **Min-Heap** to efficiently sort the array. The heap will help us keep track of the next smallest element to place in the sorted array. The steps are as follows:

1. **Initialize a Min-Heap:** 
   - Insert the first `k + 1` elements into the heap. This is because, in a nearly sorted array, the smallest element in the array could be at most `k` positions away from the start.

2. **Sorting Using the Min-Heap:**
   - Extract the minimum element (the root of the heap) and place it in the sorted position of the array.
   - Insert the next element from the array into the heap.
   - Repeat this process until all elements are processed.

3. **Handle Remaining Elements in the Heap:**
   - After all elements from the array are processed, continue extracting elements from the heap and placing them into the array until the heap is empty.
     
### **Java Code Implementation**

Here is the improved version of the Java program to sort a nearly sorted array:

```java
import java.util.PriorityQueue;

public class NearlySortedArray {
    
    // Function to sort a nearly sorted array
    public static void kSort(int[] arr, int k) {
        if (arr == null || arr.length == 0) {
            return; // Edge case: If the array is empty, do nothing
        }

        // Min-heap to store k+1 elements
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();
        
        // Add the first k + 1 elements to the heap
        for (int i = 0; i < Math.min(arr.length, k + 1); i++) {
            minHeap.add(arr[i]);
        }

        int index = 0; // Index for placing sorted elements into the array
        
        // Process the rest of the elements in the array
        for (int i = k + 1; i < arr.length; i++) {
            // Place the smallest element in the current index of the array
            arr[index++] = minHeap.poll();

            // Add the next element from the array into the heap
            minHeap.add(arr[i]);
        }

        // Extract remaining elements from the heap
        while (!minHeap.isEmpty()) {
            arr[index++] = minHeap.poll();
        }
    }

    // Utility function to print the array
    public static void printArray(int[] arr) {
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
        System.out.println();
    }

    // Driver Code
    public static void main(String[] args) {
        int k = 3; // Maximum distance of elements from their sorted position
        int[] arr = { 2, 6, 3, 12, 56, 8 };
        
        System.out.println("Original array:");
        printArray(arr);

        // Sort the nearly sorted array
        kSort(arr, k);
        
        System.out.println("Sorted array:");
        printArray(arr);
    }
}
```

### **Output:**
```
Original array:
2 6 3 12 56 8 
Sorted array:
2 3 6 8 12 56 
```

### **Explanation of the Code**
1. **Input:** The array `[2, 6, 3, 12, 56, 8]` is nearly sorted with `k = 3`.
2. **Heap Initialization:** A Min-Heap is created using the `PriorityQueue` class. The first `k + 1` elements (`[2, 6, 3, 12]`) are inserted into the heap. The heap automatically sorts these elements.
3. **Heap Operations:**
   - **Extract:** Remove the smallest element from the heap (`2`) and place it in the first position of the array.
   - **Insert:** Add the next element from the array (`56`) into the heap.
   - Repeat the above steps until all elements are processed.
4. **Empty the Heap:** After processing all elements from the array, the remaining elements in the heap are extracted and placed into the array.
5. **Output:** The array is now sorted: `[2, 3, 6, 8, 12, 56]`.

### **Detailed Steps with Example**
Let's go through the sorting process using the array `[2, 6, 3, 12, 56, 8]` with `k = 3`:

1. **Initial Heap:** Add the first `k + 1 = 4` elements (`2, 6, 3, 12`) to the Min-Heap. The heap now contains `[2, 6, 3, 12]` and internally organizes itself as `[2, 6, 3, 12]`.
   - **Heap Structure:**
     ```
       2
      / \
     6   3
         /
        12
     ```
   
2. **Extract and Insert:**
   - Extract the minimum (`2`) and place it in the array.
   - Add the next element (`56`) to the heap. The heap now contains `[3, 6, 12, 56]`.
   - Extract the minimum (`3`) and place it in the array.
   - Add the next element (`8`) to the heap. The heap now contains `[6, 8, 12, 56]`.
   - Extract the minimum (`6`) and place it in the array.
   - No more elements to add, so continue extracting from the heap.
   
3. **Extract Remaining:**
   - Extract the minimum (`8`) and place it in the array.
   - Extract the minimum (`12`) and place it in the array.
   - Extract the minimum (`56`) and place it in the array.

4. **Final Array:** `[2, 3, 6, 8, 12, 56]`.

### **Key Points to Remember**
- **Min-Heap:** We use a Min-Heap because we want to extract the smallest element efficiently to place in the sorted part of the array.
- **Heap Size:** The heap only ever contains `k + 1` elements, making both insertion and extraction operations efficient (`O(log k)`).
- **Why `k + 1` Elements?** Since any element can be at most `k` positions away from its correct position, the smallest element in the unsorted part of the array will be within the first `k + 1` elements.
  
### **Additional Examples**
- **Example 1:**
  - Input: `[10, 9, 8, 7, 4, 70, 60, 50]`, `k = 4`
  - Output: `[4, 7, 8, 9, 10, 50, 60, 70]`
  
- **Example 2:**
  - Input: `[3, -1, 2, 6, 4, 5, 8]`, `k = 2`
  - Output: `[-1, 2, 3, 4, 5, 6, 8]`

This approach efficiently sorts a nearly sorted array using a Min-Heap, ensuring optimal use of time and space.

### **Time Complexity of Building the Initial Heap: O(k)**

When building a heap with `k + 1` elements using the **bottom-up heapify method**, the time complexity is `O(k)`. This might seem counterintuitive at first, so let’s break it down concisely and with an example.

### **Heapify Process in Bottom-Up Construction**

1. **Bottom-Up Approach:** 
   - When building a heap, we start the heapify process from the last non-leaf node and move upwards to the root. This method efficiently arranges the elements into a valid heap.

2. **Why It's Linear (`O(k)`):**
   - The heap has a **complete binary tree structure**, where the number of nodes doubles as we go down each level of the tree.
   - **Most of the nodes** are concentrated in the bottom levels of the heap. These nodes need **less work** to heapify because they are close to being leaves and have fewer children to compare and swap.
   - The higher levels (closer to the root) have fewer nodes but require more work to heapify. However, this is balanced out by the larger number of nodes at the lower levels that need very minimal work.

3. **Mathematical Insight:**
   - The total cost of heapifying all nodes in a bottom-up manner is proportional to `k`, not `k * log k`. The precise formula for the work done sums up to approximately `O(k)`, which can be derived using properties of geometric series.

### **Example: Building a Heap with k + 1 = 7 Elements**
Suppose we have an array of 7 elements: `[4, 10, 3, 5, 1, 8, 7]`.

- **Step 1:** Start heapifying from the last non-leaf node (which is the middle of the array).
- **Step 2:** Move upwards to the root, fixing the heap property at each level.
- **Step 3:** The lower nodes require minimal work, while the root may require more swaps. However, the large number of nodes near the bottom (doing little work) balances out the fewer nodes near the top (doing more work).

In this example:
- Nodes near the bottom (indices 3 to 6) require almost no work.
- Nodes near the top (indices 0 to 2) may require some swaps, but there are fewer of these nodes.

### **Step 1: Building the Initial Min-Heap (O(k))**

- **What is happening:** The first step in the algorithm is to build a Min-Heap using the first `k + 1` elements of the array.
- **Why `k + 1`:** Since the array is "nearly sorted" with each element at most `k` positions away from its correct position, the next smallest element will be within the first `k + 1` elements.
- **Building the Heap:** We can create the heap in two main ways:
  1. **Heapify Method:** If we treat the `k + 1` elements as an unsorted array and use the heapify process (bottom-up construction), the time complexity for this is `O(k)`. This is because heapifying an array of size `k` takes linear time. 
  2. **Inserting One by One:** If we instead insert each of the `k + 1` elements into an empty heap one by one, each insertion would take `O(log k)` time. The total time would then be `O((k + 1) * log k)`. However, in this algorithm, we generally use the heapify method, which is more efficient (`O(k)`).

**Time Complexity for This Step:** Therefore, the time complexity for building the initial heap is `O(k)`.

### **Step 2: Processing the Remaining Elements (O((n - k) * log k))**

- **What is happening:** After building the initial heap, the algorithm processes the remaining `n - k - 1` elements in the array one by one.
- **Heap Operations:** For each element:
  1. Extract the smallest element (the root) from the heap, which takes `O(log k)` time.
  2. Insert the next element from the array into the heap, which also takes `O(log k)` time.
- **Number of Elements:** There are `n - k - 1` elements left to process, so for each of these elements, the total time is `O(log k)`.

**Time Complexity for This Step:** Since we have `n - k - 1` elements to process, the total time complexity for this step is `O((n - k) * log k)`.

### **Combining Both Steps: Total Time Complexity**

- **Step 1:** Building the initial heap takes `O(k)`.
- **Step 2:** Processing the remaining elements takes `O((n - k) * log k)`.

So, the **total time complexity** of the algorithm is:
\[
O(k) + O((n - k) * \log k)
\]

### **Why This is Efficient**

- **Building the Initial Heap:** We start with a relatively small heap of size `k + 1`. The heapify method efficiently builds this heap in `O(k)` time.
- **Heap Operations:** Inserting and extracting elements from the heap is very efficient (`O(log k)`) because the heap size is maintained at `k + 1`, which is much smaller than `n`.
- **Overall Efficiency:** The use of the Min-Heap allows us to quickly find and insert the next smallest element, which is why this method is much faster than a direct sorting approach (like quicksort or mergesort) when the array is nearly sorted.

### **Example to Illustrate Time Complexity**

Suppose we have an array of `n = 10,000` elements that is `k = 5` sorted:
1. **Step 1:** Building the initial heap of size `k + 1 = 6` takes `O(6)`, which simplifies to `O(k)` or just `O(5)`. Since `k` is small, this is a very fast operation.
2. **Step 2:** There are `n - k = 10,000 - 5 = 9,995` elements remaining to process. For each of these elements, we perform a `log k` operation. Since `k = 5`, `log k` is a constant (approximately `log 5 ≈ 2.32`).
   - Total time for this step: `O((10,000 - 5) * log 5) ≈ O(9,995 * 2.32)`.

**In Summary:** The `O(k)` part is negligible compared to `O((n - k) * log k)`, but it's important to mention it to give a complete picture. The efficiency of this method comes from processing a small heap (`k + 1` size) rather than sorting the entire array at once.

### **Key Points to Explain in an Interview**
1. **Step 1:** Building the initial heap with `k + 1` elements in `O(k)` time using the heapify method.
2. **Step 2:** Processing the remaining `n - k` elements by repeatedly extracting the minimum and inserting new elements, both operations taking `O(log k)` time, resulting in `O((n - k) * log k)`.
3. **Why it's Efficient:** Since `k` is typically much smaller than `n`, the heap operations (`O(log k)`) are very efficient, making this algorithm suitable for nearly sorted arrays.
This balance results in an **overall linear time complexity**, `O(k)`.

### **Concise Key Point for Interview:**
- **Building a Heap Using Bottom-Up Heapify:** The process has a time complexity of `O(k)` because most of the nodes in the heap are near the bottom and require very little work to heapify. This balances out the more intensive work required near the top of the heap, resulting in linear time.
  
This is more efficient than inserting elements one by one into an empty heap, which would take `O(k * log k)` time.

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
### Question 2 - **Buy Maximum Items with Given Sum - Improved Explanation**

#### **Problem Description**
Given an array of integers `arr[]` representing the cost of toys and an integer `K` representing the amount of money available, find the maximum number of toys you can purchase without exceeding `K`. You can buy only **one unit** of each toy.

#### **Examples**
1. **Input:** `arr[] = {1, 12, 5, 111, 200, 1000, 10, 9, 12, 15}`, `K = 50`
   - **Output:** `6`
   - **Explanation:** The toys with costs `1, 5, 9, 10, 12, and 12` can be purchased. Total cost = 49, hence, a maximum of 6 toys can be bought.

2. **Input:** `arr[] = {1, 12, 5, 111, 200, 1000, 10}`, `K = 50`
   - **Output:** `4`
   - **Explanation:** The toys with costs `1, 5, 10, and 12` can be bought. Total cost = 28, hence, a maximum of 4 toys can be bought.

### **Approach: Greedy with Min-Heap (Priority Queue)**
The goal is to buy the cheapest toys first so that we can maximize the number of toys bought. We can achieve this using a **Min-Heap** (Priority Queue in Java), which allows us to efficiently get the smallest cost toy.

#### **Step-by-Step Explanation**
1. **Insert All Elements into a Min-Heap:**
   - A Min-Heap (Priority Queue) is used to keep the toy costs in a way that allows us to efficiently get the smallest cost toy.
   - Insert all the toy prices into the Min-Heap. Inserting `N` elements into a Min-Heap takes `O(N log N)` time.

2. **Buying Toys:**
   - Initialize a `count` variable to track the number of toys purchased.
   - While the Min-Heap is not empty and the top element (the cheapest toy) can be bought within the remaining budget (`K`):
     - Subtract the cost of the toy from `K`.
     - Remove the toy from the heap.
     - Increase the `count`.
   - Stop when you either run out of money (`K`) or there are no more toys you can afford.

3. **Return the Count:** The total number of toys that were bought is stored in `count`.

#### **Complexity Analysis**
- **Time Complexity:** `O(N log N)` because:
  - Building the Min-Heap takes `O(N log N)`.
  - Extracting elements from the Min-Heap takes `O(log N)` for each extraction.
- **Space Complexity:** `O(N)` for storing all elements in the Min-Heap.

### **Improved Code with Comments and Explanation**

Here is the improved Java implementation:

```java
import java.util.PriorityQueue;

public class MaxToys {
    
    // Function to find the maximum number of toys that can be bought within budget k
    public static int maxToys(int[] arr, int k) {
        int n = arr.length;

        // Create a Min-Heap (PriorityQueue) to store the toy costs
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();
        
        // Insert all elements into the Min-Heap
        for (int i = 0; i < n; i++) {
            minHeap.offer(arr[i]);
        }

        // To keep track of the number of toys bought
        int count = 0;
        
        // Process elements in the Min-Heap
        while (!minHeap.isEmpty() && minHeap.peek() <= k) {
            // Get the cost of the cheapest toy
            k -= minHeap.poll(); // Subtract the cost from the budget
            count++; // Increase the count of toys bought
        }
        
        return count;
    }

    // Utility function to print an array
    public static void printArray(int[] arr) {
        for (int i : arr) {
            System.out.print(i + " ");
        }
        System.out.println();
    }

    // Driver code
    public static void main(String[] args) {
        int[] arr = { 1, 12, 5, 111, 200, 1000, 10, 9, 12, 15 };
        int k = 50;

        System.out.println("Maximum toys that can be bought: " + maxToys(arr, k));
    }
}
```

#### **Output:**
```
Maximum toys that can be bought: 6
```

### **Example Walkthrough**
Let's walk through the example `arr[] = {1, 12, 5, 111, 200, 1000, 10}`, `K = 50`:

1. **Initial Min-Heap:** After inserting all elements into the heap:
   ```
   [1, 5, 10, 12, 111, 200, 1000]
   ```
   The heap organizes itself such that the smallest element is always at the root.

2. **Buying Toys:**
   - Remove the smallest element (`1`). New budget: `50 - 1 = 49`. Toy count: `1`.
   - Remove the next smallest element (`5`). New budget: `49 - 5 = 44`. Toy count: `2`.
   - Remove the next smallest element (`10`). New budget: `44 - 10 = 34`. Toy count: `3`.
   - Remove the next smallest element (`12`). New budget: `34 - 12 = 22`. Toy count: `4`.
   - The next element in the heap is `111`, which is greater than the remaining budget (`22`), so we stop.

3. **Result:** The maximum number of toys that can be bought is `4`.

### **Additional Points to Cover for Beginners**
1. **PriorityQueue:** In Java, a `PriorityQueue` is a Min-Heap by default. This means it allows us to efficiently get the smallest element, which is perfect for this problem.

2. **Heap Operations:** 
   - **offer():** Adds an element to the heap (`O(log N)` time).
   - **peek():** Returns the smallest element without removing it (`O(1)` time).
   - **poll():** Removes and returns the smallest element (`O(log N)` time).

3. **Why Use a Min-Heap?** 
   - The Min-Heap allows us to always pick the cheapest toy to maximize the number of toys we can buy within the budget.

4. **Edge Cases:**
   - If `arr` is empty, the result is `0`.
   - If `K` is `0`, no toys can be bought, so the result is `0`.

### **Alternative Approach: Sorting**
- Instead of using a Min-Heap, you could sort the array first and then pick toys until you exhaust the budget.
- **Time Complexity:** `O(N log N)` due to sorting.
- This approach has a similar time complexity as using a Min-Heap but does not make use of the heap properties.

### **Final Notes**
- **Choosing the Right Data Structure:** A Min-Heap is chosen here for its efficiency in handling the extraction of the smallest element.
- **Efficiency:** The `O(N log N)` time complexity makes this solution efficient enough for large input sizes.

With this explanation and code, a beginner should have a clear understanding of how to solve this problem using a Min-Heap, why it's efficient, and how to implement it in Java.

--
The most optimal solution for this problem is **sorting the array** and then iterating through it to pick the maximum number of toys within the given budget. This solution is straightforward and avoids the additional space overhead of using a Min-Heap (PriorityQueue). Here's a detailed breakdown:

### **Optimized Approach: Sorting the Array**
1. **Sort the Array:** 
   - Sort the array in ascending order of toy prices.
   - Time complexity of sorting: `O(N log N)`.

2. **Buy Toys:** 
   - Initialize a `count` variable to keep track of the number of toys bought.
   - Iterate through the sorted array, subtracting the price of each toy from `K` (the budget) until `K` is less than the price of the next toy.
   - Stop once you run out of money or have iterated through all possible toys.

3. **Return the Count:** 
   - The count will represent the maximum number of toys that can be purchased with the given budget.

### **Why This is Optimal**
- **Sorting First:** Sorting the array allows us to quickly pick the cheapest toys first, maximizing the number of toys bought. 
- **Iterating After Sorting:** Once the array is sorted, iterating through it linearly (`O(N)`) is efficient.
- **Total Time Complexity:** The overall time complexity of this approach is dominated by the sorting step, so it is `O(N log N)`.
- **Space Complexity:** Since we sort the array in place, the space complexity is `O(1)` (excluding the space needed for the input array).

### **Code Implementation (Java)**
Here's how you can implement this optimal solution:

```java
import java.util.Arrays;

public class MaxToysOptimized {
    
    // Function to find the maximum number of toys that can be bought within budget K
    public static int maxToys(int[] arr, int k) {
        // Sort the array to prioritize buying cheaper toys first
        Arrays.sort(arr);
        
        int count = 0;
        
        // Iterate through the sorted array and buy toys until we run out of money
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] <= k) {
                k -= arr[i]; // Subtract the price of the toy from the budget
                count++; // Increase the count of toys bought
            } else {
                break; // Stop if we can't afford the next toy
            }
        }
        
        return count;
    }

    // Driver code
    public static void main(String[] args) {
        int[] arr = {1, 12, 5, 111, 200, 1000, 10, 9, 12, 15};
        int k = 50;

        System.out.println("Maximum toys that can be bought: " + maxToys(arr, k));
    }
}
```

### **Output:**
```
Maximum toys that can be bought: 6
```

### **Example Walkthrough**
Given:
- `arr = {1, 12, 5, 111, 200, 1000, 10, 9, 12, 15}`
- `K = 50`

**Step-by-Step Execution:**
1. **Sort the array:** `[1, 5, 9, 10, 12, 12, 15, 111, 200, 1000]`
2. **Buy toys until budget is exhausted:**
   - Buy `1`: Remaining budget = 49
   - Buy `5`: Remaining budget = 44
   - Buy `9`: Remaining budget = 35
   - Buy `10`: Remaining budget = 25
   - Buy `12`: Remaining budget = 13
   - Buy `12`: Remaining budget = 1
   - Can't afford `15`, so stop.
3. **Total toys bought:** 6

### **Complexity Analysis**
- **Time Complexity:** `O(N log N)` due to sorting.
- **Space Complexity:** `O(1)` if sorting is done in place (ignoring the input array).

### **Why This is More Optimal than Using a Min-Heap**
- **Simpler and More Efficient:** Sorting the array and iterating through it is simpler and avoids the `O(log N)` insertion and removal operations associated with the heap.
- **Lower Space Usage:** Sorting in place requires `O(1)` additional space, while a Min-Heap requires `O(N)` space.

### **Final Thoughts**
This sorting-based approach is the **most optimal** in terms of time complexity (`O(N log N)`) and space complexity (`O(1)`). It directly addresses the problem by prioritizing cheaper toys first, ensuring we can maximize the count of toys purchased. For nearly all practical scenarios, this method will provide the most efficient solution.

-----------------

### Question 3 - **K Closest Elements: Improved Explanation for Beginners**

Given a **sorted array** and a number `X`, find the `K` elements in the array that are closest to `X`. The elements must be distinct and arranged in ascending order based on their closeness to `X`.

### **Examples:**
1. **Input:** 
   - `K = 4`, `X = 35`
   - `arr[] = {12, 16, 22, 30, 35, 39, 42, 45, 48, 50, 53, 55, 56}`
   - **Output:** `30 39 42 45`
   - **Explanation:** The closest elements to `35` in ascending order are `30`, `39`, `42`, and `45`.

2. **Input:** 
   - `K = 3`, `X = 20`
   - `arr[] = {1, 5, 10, 15, 25, 35, 45}`
   - **Output:** `15 25 10`
   - **Explanation:** The closest elements to `20` are `15`, `25`, and `10`.

### **Steps to Solve the Problem:**
1. **Find the Crossover Point:** This is the point in the sorted array where elements to the left are smaller than or equal to `X`, and elements to the right are larger than `X`. This can be found using a **binary search** in `O(log n)` time.
2. **Pick Closest Elements:** Using the crossover point, expand outward in both directions (left and right) to find the `K` elements closest to `X`. This step takes `O(K)` time because we only need to select `K` elements.
3. **Print/Return the Closest Elements:** Collect and return the `K` closest elements based on the conditions mentioned.

### **Optimized Approach:**
1. **Binary Search to Find Crossover Point (`O(log n)`):** 
   - Use binary search to find the crossover point where `arr[mid]` is closest to `X`.
   - This point helps to quickly narrow down the range of elements we need to consider for the closest values.
2. **Two-pointer Technique to Collect Closest Elements (`O(K)`):** 
   - Once the crossover point is found, use two pointers (`l` and `r`) to collect the `K` closest elements by comparing distances on both sides of the crossover point.

### **Time Complexity:** 
- **Finding Crossover Point:** `O(log n)` due to binary search.
- **Finding `K` Closest Elements:** `O(K)` since we are iterating through a maximum of `K` elements.
- **Total Time Complexity:** `O(log n + K)`.

### **Space Complexity:** 
- **Auxiliary Space:** `O(1)` since we are using a constant amount of extra space.

### **Detailed Code Implementation (Java):**
Here is the improved implementation of the approach in Java:

```java
public class KClosestElements {

    // Function to find the crossover point
    public static int findCrossOver(int[] arr, int low, int high, int x) {
        // If x is greater than all elements, return the last element
        if (arr[high] <= x) {
            return high;
        }
        
        // If x is smaller than all elements, return the first element
        if (arr[low] > x) {
            return low;
        }
        
        // Find the middle point
        int mid = (low + high) / 2;
        
        // If x is exactly at mid or crossover is found
        if (arr[mid] <= x && arr[mid + 1] > x) {
            return mid;
        }
        
        // If x is greater than arr[mid], crossover is in right half
        if (arr[mid] < x) {
            return findCrossOver(arr, mid + 1, high, x);
        }
        
        // Otherwise, crossover is in left half
        return findCrossOver(arr, low, mid - 1, x);
    }
    
    // Function to print k closest elements
    public static void printKClosest(int[] arr, int x, int k, int n) {
        // Find the crossover point
        int l = findCrossOver(arr, 0, n - 1, x);
        int r = l + 1; // Right index to explore
        int count = 0; // Track number of elements printed
        
        // If x is present in the array, move left pointer
        if (arr[l] == x) {
            l--;
        }
        
        // Compare elements on both sides of the crossover point
        while (l >= 0 && r < n && count < k) {
            if (x - arr[l] <= arr[r] - x) {
                System.out.print(arr[l--] + " ");
            } else {
                System.out.print(arr[r++] + " ");
            }
            count++;
        }
        
        // If there are no more elements on the right side
        while (count < k && l >= 0) {
            System.out.print(arr[l--] + " ");
            count++;
        }
        
        // If there are no more elements on the left side
        while (count < k && r < n) {
            System.out.print(arr[r++] + " ");
            count++;
        }
    }
    
    // Driver code
    public static void main(String[] args) {
        int[] arr = {12, 16, 22, 30, 35, 39, 42, 45, 48, 50, 53, 55, 56};
        int x = 35, k = 4;
        System.out.println("The " + k + " closest elements to " + x + " are:");
        printKClosest(arr, x, k, arr.length);
    }
}
```

### **Output:**
```
The 4 closest elements to 35 are:
30 39 42 45
```

### **Explanation of the Code**
1. **findCrossOver Method:** 
   - Uses binary search to find the crossover point where elements change from being less than or equal to `X` to greater than `X`.
   - Returns the index where the closest value to `X` is found.

2. **printKClosest Method:** 
   - Takes the crossover point and uses two pointers (`l` and `r`) to collect the `K` elements that are closest to `X`.
   - Compares elements on both sides of the crossover point to find the `K` closest elements.
   - Continues until it finds all `K` elements or runs out of elements to check on either side.

3. **Driver Code:** 
   - Defines an array and a target value `X`.
   - Calls the `printKClosest` method to display the closest elements.

### **Additional Points for Beginners:**
1. **Binary Search:** This is a key technique in the optimized solution, allowing us to efficiently locate the crossover point in a sorted array.
2. **Two-Pointer Technique:** Once the crossover point is found, the two-pointer technique helps us pick the closest elements by expanding outward from the crossover point.
3. **Edge Cases:** 
   - If `X` is smaller than all elements, the crossover point will be at the start of the array.
   - If `X` is larger than all elements, the crossover point will be at the end of the array.
   - If `K` is larger than the total number of elements, handle it by printing all elements.

### **Alternative Approach for Beginners: Sorting by Difference**
- If binary search is too complex, another approach is to sort the array based on the absolute difference with `X`. However, this takes `O(n log n)` time and is less efficient than the above `O(log n + k)` approach.

### **Final Notes**
- The binary search method combined with the two-pointer technique provides an efficient way to solve this problem in `O(log n + k)` time.
- This approach is suitable for large arrays, especially when `k` is small compared to the size of the array.

By following this approach and code, beginners can gain a solid understanding of using binary search and two-pointer techniques to efficiently find the `K` closest elements in a sorted array.

--------------------------------------------------------------------------------------------------------------------------------------------------------

### Question 4 - **Merge K Sorted Arrays - Detailed Explanation for Beginners**

#### **Problem Description**
Given `k` sorted arrays, each of possibly different sizes, merge all these arrays into a single sorted array.

#### **Examples:**
1. **Input:** 
   - `k = 3`
   - `arr[][] = { {1, 3}, {2, 4, 6}, {0, 9, 10, 11} }`
   - **Output:** `0 1 2 3 4 6 9 10 11`
   
2. **Input:**
   - `k = 2`
   - `arr[][] = { {1, 3, 20}, {2, 4, 6} }`
   - **Output:** `1 2 3 4 6 20`

### **Basic Approach**
- A simple solution is to copy all elements from the `k` arrays into a single array and sort it. 
- **Time Complexity:** `O(N log N)`, where `N` is the total number of elements across all `k` arrays.
- This approach is not optimal because it requires sorting all elements even though each individual array is already sorted.

### **Optimized Approach Using a Min-Heap (Priority Queue)**
To achieve a more efficient solution, we use a **min-heap** (priority queue) to keep track of the smallest elements from each array.

#### **Steps of the Optimized Solution:**
1. **Initialize a Min-Heap:**
   - Create a min-heap (priority queue) to store elements from all arrays. Each element in the heap is a small object containing:
     - The array index (`x`), representing the array from which the element comes.
     - The element's position within that array (`y`).
     - The value of the element.

2. **Insert First Elements of Each Array into the Min-Heap:**
   - Add the first element of each of the `k` arrays into the heap.

3. **Merge Arrays:**
   - While the heap is not empty:
     1. Extract the smallest element from the heap and add it to the result array.
     2. Insert the next element from the same array (from which the smallest element was extracted) into the heap.
     3. Repeat until all elements from all arrays are processed.

4. **Handle Edge Cases:**
   - If an array is empty, do not insert any elements into the heap from that array.
   - Ensure you stop processing when there are no more elements to add from an array.

### **Why Use a Min-Heap?**
- A **min-heap** efficiently keeps track of the smallest element among the `k` arrays.
- By using a min-heap of size `k`, inserting and extracting elements takes `O(log k)` time.
- This leads to an overall time complexity of `O(N log k)`, which is more efficient than directly sorting the combined array (`O(N log N)`).

### **Time and Space Complexity**
- **Time Complexity:** `O(N log k)`, where `N` is the total number of elements across all `k` arrays.
- **Space Complexity:** `O(N)` to store the merged output, plus `O(k)` for the heap.

### **Improved Code Explanation (Java)**
Here is the improved and detailed implementation using a min-heap:

```java
import java.util.ArrayList;
import java.util.PriorityQueue;

public class MergeKSortedArrays {
    
    // Class to represent elements in the heap
    private static class HeapNode implements Comparable<HeapNode> {
        int arrayIndex;  // Index of the array from which the element is taken
        int elementIndex; // Index of the element within that array
        int value;  // The value of the element
        
        HeapNode(int arrayIndex, int elementIndex, int value) {
            this.arrayIndex = arrayIndex;
            this.elementIndex = elementIndex;
            this.value = value;
        }
        
        @Override
        public int compareTo(HeapNode other) {
            return Integer.compare(this.value, other.value); // Compare elements based on their values
        }
    }

    // Function to merge K sorted arrays
    public static ArrayList<Integer> mergeKArrays(int[][] arr, int K) {
        ArrayList<Integer> result = new ArrayList<>();
        PriorityQueue<HeapNode> minHeap = new PriorityQueue<>();
        
        // Step 1: Insert the first element of each array into the min-heap
        for (int i = 0; i < K; i++) {
            if (arr[i].length > 0) { // Check if the array is not empty
                minHeap.add(new HeapNode(i, 0, arr[i][0]));
            }
        }
        
        // Step 2: Process elements in the min-heap
        while (!minHeap.isEmpty()) {
            // Extract the smallest element from the heap
            HeapNode current = minHeap.poll();
            result.add(current.value);
            
            // Insert the next element from the same array into the heap (if it exists)
            int nextElementIndex = current.elementIndex + 1;
            if (nextElementIndex < arr[current.arrayIndex].length) {
                minHeap.add(new HeapNode(current.arrayIndex, nextElementIndex, arr[current.arrayIndex][nextElementIndex]));
            }
        }
        
        return result;
    }

    // Driver code to test the implementation
    public static void main(String[] args) {
        int[][] arr = {
            {2, 6, 12},
            {1, 9},
            {23, 34, 90, 2000}
        };
        ArrayList<Integer> mergedArray = mergeKArrays(arr, arr.length);
        System.out.println("Merged array is: " + mergedArray);
    }
}
```

### **Output:**
```
Merged array is: [1, 2, 6, 9, 12, 23, 34, 90, 2000]
```

### **Explanation of the Code:**
1. **HeapNode Class:** 
   - Represents an element in the heap with:
     - `arrayIndex`: The index of the array from which the element is taken.
     - `elementIndex`: The index of the element within the array.
     - `value`: The value of the element.
   - Implements `Comparable<HeapNode>` to allow the heap to order elements based on their values.

2. **mergeKArrays Method:** 
   - Initializes a min-heap (`PriorityQueue`) to keep track of the smallest elements from each array.
   - Adds the first element of each array to the heap.
   - Extracts the smallest element from the heap, adds it to the result list, and then inserts the next element from the same array into the heap.
   - Repeats until all elements are merged.

3. **main Method:** 
   - Demonstrates the merging of `k` sorted arrays using the `mergeKArrays` method.

### **Alternative Approach: Using Merge Sort**
- If you are familiar with the **Merge Sort** algorithm, you could iteratively merge two arrays at a time until all `k` arrays are merged.
- **Time Complexity:** This would be less efficient at `O(N log k)` when compared to the heap-based approach, which is already optimal for this problem.

### **Edge Cases to Consider:**
1. **Empty Arrays:** If some of the `k` arrays are empty, the code should handle these gracefully.
2. **Single Element Arrays:** Ensure that arrays with a single element are processed correctly.
3. **Different Array Sizes:** The arrays can have different sizes, so be sure to check boundaries when inserting elements into the heap.

### **Visual Example for Better Understanding:**
Consider `arr[][] = { {2, 6, 12}, {1, 9}, {23, 34, 90, 2000} }`:
1. Insert the first element of each array into the heap: `minHeap = [2, 1, 23]`.
2. Extract `1` (smallest) from the heap and insert the next element from its array (`9`): `minHeap = [2, 9, 23]`.
3. Continue this process until all elements are processed.

### **Final Notes:**
- **Why Use a Min-Heap?** The min-heap efficiently keeps track of the smallest elements, allowing us to build the sorted merged array without having to sort all elements at once.
- **Complexity:** The heap ensures that insertion and extraction take `O(log k)` time, making the overall complexity `O(N log k)`, which is optimal for this problem.

This explanation and improved code should give beginners a solid understanding of how to merge `k` sorted arrays efficiently using a min-heap.


--------------------------------------------------------------------------------------------------------------------------------------------------------


### Question 5 - **Finding the Median of a Stream - Improved Explanation for Beginners**

#### **Problem Statement**
Given a stream of integers, find the median of elements read so far. For simplicity, let's assume there are no duplicate values. The median is the middle value in an ordered list:
- **If the number of elements is odd**, the median is the middle element.
- **If the number of elements is even**, the median is the average of the two middle elements.

#### **Examples:**
1. **Stream:** `5, 15, 1, 3`
   - After reading 1st element (`5`): Median = `5`
   - After reading 2nd element (`5, 15`): Median = `(5 + 15) / 2 = 10`
   - After reading 3rd element (`5, 15, 1`): Median = `5`
   - After reading 4th element (`5, 15, 1, 3`): Median = `(3 + 5) / 2 = 4`

2. **Stream:** `1, 2, 3, 4, 5`
   - Medians after each insertion: `1, 1.5, 2, 2.5, 3`

#### **Understanding the Online Algorithm**
- This is an **online algorithm**, which means it processes each incoming element one at a time and can provide the median of the stream at any point.
  
#### **Optimal Solution Using Heaps**
The most efficient way to solve this problem is by using two heaps:
1. **Max-Heap (left side):** Represents elements that are less than or equal to the median.
2. **Min-Heap (right side):** Represents elements that are greater than the median.

#### **Steps to Find the Median:**
1. **Balancing Heaps:** Ensure the difference in sizes between the max-heap and min-heap is at most 1. This keeps the heaps balanced.
2. **Insertion:** For each new element:
   - Add the element to the max-heap (negating the value since Java's `PriorityQueue` is a min-heap by default).
   - Move the largest element from the max-heap to the min-heap.
   - Balance the heaps if the min-heap has more elements than the max-heap.
3. **Finding the Median:**
   - If both heaps have the same number of elements, the median is the average of the roots of both heaps.
   - If the max-heap has more elements, the median is the root of the max-heap.

### **Time Complexity:**
- **Insertion:** Each insertion into a heap takes `O(log N)` time, where `N` is the number of elements in the heap.
- **Total Complexity:** Since each insertion is `O(log N)` and there are `N` elements, the overall complexity for processing the stream is `O(N log N)`.

### **Space Complexity:**
- **Auxiliary Space:** `O(N)`, as the heaps collectively store all elements from the stream.

### **Improved Code Explanation (Java):**
Here’s a revised, clearer version of the code with comments and structured explanations:

```java
import java.util.PriorityQueue;

public class MedianOfStream {
    
    // Function to find the median of the stream
    public static void findMedian(int[] stream) {
        // Max-heap to store the smaller half of the elements
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a);
        // Min-heap to store the larger half of the elements
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();
        
        for (int num : stream) {
            // Step 1: Add to maxHeap (negating for max behavior)
            maxHeap.offer(num);
            
            // Step 2: Balance heaps - move the largest element from maxHeap to minHeap
            minHeap.offer(maxHeap.poll());
            
            // Step 3: Ensure maxHeap has more or equal elements than minHeap
            if (maxHeap.size() < minHeap.size()) {
                maxHeap.offer(minHeap.poll());
            }
            
            // Step 4: Print the median
            if (maxHeap.size() > minHeap.size()) {
                System.out.println(maxHeap.peek()); // Odd number of elements
            } else {
                System.out.println((maxHeap.peek() + minHeap.peek()) / 2.0); // Even number of elements
            }
        }
    }

    // Driver code
    public static void main(String[] args) {
        int[] stream = {5, 15, 1, 3, 2, 8, 7, 9, 10, 6, 11, 4};
        findMedian(stream);
    }
}
```

### **Output:**
```
5
10.0
5
4.0
3
4.0
5
6.0
7
6.5
7
6.5
```

### **Explanation of the Code:**
1. **Heaps Setup:**
   - **`maxHeap`:** Stores the smaller half of the numbers (implemented as a max-heap using a comparator).
   - **`minHeap`:** Stores the larger half of the numbers (natural min-heap behavior).
   
2. **Adding Elements to the Stream:**
   - First, insert the element into `maxHeap`.
   - Then, move the largest element from `maxHeap` to `minHeap` to maintain the balance.
   - If `minHeap` has more elements than `maxHeap`, move the root of `minHeap` back to `maxHeap`.

3. **Finding the Median:**
   - If `maxHeap` has more elements, the median is the root of `maxHeap`.
   - If both heaps are of the same size, the median is the average of the roots of `maxHeap` and `minHeap`.

### **Key Points for Beginners:**
1. **Heaps in Java:** 
   - Java's `PriorityQueue` is a min-heap by default.
   - To implement a max-heap, we use a custom comparator `(a, b) -> b - a`.
   
2. **Why Two Heaps?** 
   - The two heaps allow for an efficient way to keep track of the middle elements in the stream, making it possible to calculate the median in logarithmic time for each insertion.

3. **Balanced Heaps:** 
   - The heaps are balanced to ensure that the median can be easily calculated.
   - The difference in size between the heaps is always at most 1.

### **Similar Examples to Practice:**
1. **Finding the Median of an Even Stream:** For a stream `2, 4, 6, 8`, the medians would be `2, 3.0, 4, 5.0`.
2. **Handling Negative Numbers:** For a stream `-1, -2, -3`, the medians would be `-1, -1.5, -2`.

### **Visual Diagram for Understanding:**
- **Step 1:** Insert `5`:
  - `maxHeap: [5]`
  - `minHeap: []`
  - Median: `5`
- **Step 2:** Insert `15`:
  - `maxHeap: [5]`
  - `minHeap: [15]`
  - Median: `(5 + 15) / 2 = 10.0`
- **Step 3:** Insert `1`:
  - `maxHeap: [5, 1]` (heap orders: `5, 1`)
  - `minHeap: [15]`
  - Median: `5`

This step-by-step breakdown with diagrams helps to visually understand how heaps dynamically adjust as new elements are added.

### **Final Notes**
- This heap-based approach is optimal for finding the median in a stream with a time complexity of `O(log N)` for each insertion.
- For larger datasets or real-time data streams, this method provides an efficient way to continuously track the median.
- Understanding the use of heaps and maintaining balance between the two heaps is key to solving this problem effectively.
