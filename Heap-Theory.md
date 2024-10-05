### Enhanced Explanation of Heaps for Complete Beginners

### **1. What is a Heap?**
A **Heap** is a special Tree-based data structure that satisfies two properties:
1. **Completeness:** It is a complete binary tree, meaning all levels are fully filled except possibly the last level, which is filled from left to right.
2. **Heap Property:** A Heap can either be a **Min-Heap** or a **Max-Heap**:
   - **Min-Heap:** The value of each node is smaller than or equal to its children. The smallest element is always at the root.
   - **Max-Heap:** The value of each node is greater than or equal to its children. The largest element is always at the root.

### **2. Visual Representation**
Refer to the image for examples:
- **Min-Heap:** The root (10) is smaller than its children, and this property is recursively true for all sub-trees.
- **Max-Heap:** The root (100) is larger than its children, and this property is maintained throughout the tree.

### **3. Binary Heap: What is it?**
A **Binary Heap** is a heap where each node has at most two children. It’s also a complete binary tree, which allows it to be efficiently represented using an array.

### **4. Array Representation of Binary Heaps**
Since a Binary Heap is a complete binary tree, we can store it in an array. Here’s how:
- **Root:** The root element is stored at `Arr[0]`.
- **Parent Node:** For the element at index `i`, the parent is located at `Arr[(i - 1) / 2]`.
- **Left Child:** The left child of the element at index `i` is located at `Arr[(2 * i) + 1]`.
- **Right Child:** The right child of the element at index `i` is located at `Arr[(2 * i) + 2]`.

### **5. Basic Heap Operations**

#### **1. Finding the Minimum or Maximum Element**
- **Min-Heap:** The minimum element is always at the root (`Arr[0]`). Fetching this element takes `O(1)` time.
- **Max-Heap:** The maximum element is always at the root (`Arr[0]`). Fetching this element also takes `O(1)` time.

#### **2. Insertion in Heaps**
To insert an element in a heap while maintaining the heap properties:
1. Insert the new element at the **end** of the heap.
2. **Heapify Up:** Compare the new element with its parent. If it violates the heap property, swap it with its parent. Continue this process (recursively or iteratively) until the heap property is restored.

**Example: Insert `15` into a Max-Heap:**
Initial Heap:
```
       10
      /  \
     5    3
    / \
   2   4
```
- **Step 1:** Insert `15` at the end.
```
       10
      /  \
     5    3
    / \  /
   2   4 15
```
- **Step 2:** Heapify up. Swap `15` with its parent (`3`).
```
       10
      /  \
     5    15
    / \  /
   2   4 3
```
- **Step 3:** Swap `15` with its parent (`10`).
```
       15
      /  \
     5    10
    / \  /
   2   4 3
```
Final Heap:
```
       15
      /  \
     5    10
    / \  /
   2   4 3
```

**Java Implementation:**
```java
class MaxHeap {
    private int[] heap;
    private int size;

    public MaxHeap(int capacity) {
        heap = new int[capacity];
        size = 0;
    }

    public void insert(int value) {
        if (size == heap.length) {
            throw new IllegalStateException("Heap is full");
        }
        heap[size] = value;
        size++;
        heapifyUp(size - 1);
    }

    private void heapifyUp(int index) {
        int parentIndex = (index - 1) / 2;
        if (parentIndex >= 0 && heap[parentIndex] < heap[index]) {
            swap(index, parentIndex);
            heapifyUp(parentIndex);
        }
    }

    private void swap(int i, int j) {
        int temp = heap[i];
        heap[i] = heap[j];
        heap[j] = temp;
    }
}
```

#### **3. Deletion in Heaps**
- **Standard Deletion:** Removes the root element (maximum in Max-Heap, minimum in Min-Heap).
1. Replace the root with the **last element** in the heap.
2. Remove the last element.
3. **Heapify Down:** Compare the new root with its children and swap it with the larger child (in Max-Heap) or smaller child (in Min-Heap) if necessary. Repeat this process until the heap property is restored.

**Example: Delete root (`10`) in a Max-Heap:**
Initial Heap:
```
       10
      /  \
     5    3
    / \
   2   4
```
- **Step 1:** Replace root with the last element (`4`).
```
       4
      /  \
     5    3
    / 
   2   
```
- **Step 2:** Heapify down. Swap `4` with its larger child (`5`).
```
       5
      /  \
     4    3
    / 
   2   
```
Final Heap:
```
       5
      /  \
     4    3
    / 
   2   
```

**Java Implementation:**
```java
class MaxHeap {
    private int[] heap;
    private int size;

    public MaxHeap(int capacity) {
        heap = new int[capacity];
        size = 0;
    }

    public void deleteRoot() {
        if (size == 0) {
            throw new IllegalStateException("Heap is empty");
        }
        heap[0] = heap[size - 1]; // Replace root with last element
        size--;
        heapifyDown(0);
    }

    private void heapifyDown(int index) {
        int largest = index;
        int leftChild = 2 * index + 1;
        int rightChild = 2 * index + 2;

        if (leftChild < size && heap[leftChild] > heap[largest]) {
            largest = leftChild;
        }
        if (rightChild < size && heap[rightChild] > heap[largest]) {
            largest = rightChild;
        }
        if (largest != index) {
            swap(index, largest);
            heapifyDown(largest);
        }
    }

    private void swap(int i, int j) {
        int temp = heap[i];
        heap[i] = heap[j];
        heap[j] = temp;
    }
}
```

### **6. Time and Space Complexity**
- **Insertion:** `O(log n)` - Because we may need to "heapify up" the inserted element to restore the heap property.
- **Deletion:** `O(log n)` - Because we may need to "heapify down" the new root element.
- **Getting Maximum (Max-Heap) or Minimum (Min-Heap):** `O(1)` - Direct access to the root of the heap.

### **Summary**
- A **Heap** is a complete binary tree that satisfies the heap property (either Min-Heap or Max-Heap).
- It is often represented as an array for efficient indexing.
- **Insertion** and **deletion** operations involve heapifying to maintain the heap property.
- Heaps are widely used in algorithms like **Heap Sort**, and for implementing priority queues.

### **Additional Points**
- **Heapify Up vs. Heapify Down:** "Heapify up" is used during insertion to adjust the position of the new element. "Heapify down" is used during deletion to adjust the position of the root.
- **Priority Queue:** Heaps are used to implement priority queues, which efficiently support operations like finding the highest (or lowest) priority element.
  

-------------------------------

### Improved Notes on Heap Data Structure for Beginners

---

## **Heap Data Structure: Basics**
A **Heap** is a specialized Tree-based data structure that follows specific rules:
1. **Complete Binary Tree:** All levels are completely filled except possibly the last, which is filled from left to right.
2. **Heap Property:** A heap can be either:
   - **Min-Heap:** The value of each node is smaller than or equal to its children. The root always contains the smallest element.
   - **Max-Heap:** The value of each node is greater than or equal to its children. The root always contains the largest element.

### **Examples**
- **Min-Heap:** Root = 10, children have larger values.
- **Max-Heap:** Root = 100, children have smaller values.

The above properties make heaps useful for implementing **priority queues** and algorithms like **Heap Sort**.

---

## **Binary Heap**
A **Binary Heap** is a complete binary tree where each node has at most two children:
- **Min-Heap:** The parent node's value is always less than or equal to its children.
- **Max-Heap:** The parent node's value is always greater than or equal to its children.

### **Array Representation**
Since a binary heap is a complete binary tree, it can be efficiently stored in an array:
- **Root:** `Arr[0]`
- **Parent of Node at `i`:** `Arr[(i-1)/2]`
- **Left Child of Node at `i`:** `Arr[(2*i)+1]`
- **Right Child of Node at `i`:** `Arr[(2*i)+2]`

---

## **Heap Operations**
### 1. **Getting the Minimum/Maximum**
- **Min-Heap:** The minimum element is at the root (`Arr[0]`), which can be retrieved in `O(1)` time.
- **Max-Heap:** The maximum element is at the root (`Arr[0]`), retrievable in `O(1)` time.

### 2. **Insertion (`insertKey`)**
- **Steps:**
  1. Add the new element at the end of the heap.
  2. **Heapify Up:** Compare the new element with its parent. Swap if the heap property is violated (smaller than the parent in a Min-Heap, larger in a Max-Heap).
  3. Repeat this process until the heap property is restored.
- **Time Complexity:** `O(log n)`, where `n` is the number of elements in the heap.
  
**Example: Insert `15` in a Max-Heap:**
```
Initial Heap:
       10
      /  \
     5    3
    / \  
   2   4

1. Insert `15` at the end:
       10
      /  \
     5    3
    / \  /
   2   4 15

2. Heapify Up:
   Swap `15` with `3`:
       10
      /  \
     5    15
    / \  /
   2   4 3

   Swap `15` with `10`:
       15
      /  \
     5    10
    / \  /
   2   4 3
```

### 3. **Decrease Key (`decreaseKey`)**
- **Purpose:** Reduces the value of a key at a given index. Mainly used in Min-Heaps.
- **Steps:**
  1. Set the new value.
  2. **Heapify Up:** Check if the new value violates the heap property. If yes, swap it with its parent until the property is restored.
- **Time Complexity:** `O(log n)`.

### 4. **Extract Minimum/Maximum (`extractMin`/`extractMax`)**
- **Purpose:** Removes the root (minimum in Min-Heap, maximum in Max-Heap).
- **Steps:**
  1. Replace the root with the last element in the heap.
  2. Remove the last element.
  3. **Heapify Down:** Swap the new root with its smaller (in Min-Heap) or larger (in Max-Heap) child until the heap property is restored.
- **Time Complexity:** `O(log n)`.

### 5. **Delete Key (`deleteKey`)**
- **Purpose:** Deletes a key at a specific index.
- **Steps:**
  1. Reduce the key to a very small value (negative infinity for Min-Heap) using `decreaseKey`.
  2. Call `extractMin` to remove the new root.
- **Time Complexity:** `O(log n)`.

### **Java Code for Basic Heap Operations**
```java
class MinHeap {
    private int[] heap;
    private int size;
    private int capacity;

    public MinHeap(int capacity) {
        this.capacity = capacity;
        this.size = 0;
        heap = new int[capacity];
    }

    // Insert a new key
    public void insertKey(int key) {
        if (size == capacity) {
            System.out.println("Overflow: Cannot insertKey");
            return;
        }
        size++;
        int i = size - 1;
        heap[i] = key;

        while (i != 0 && heap[parent(i)] > heap[i]) {
            swap(i, parent(i));
            i = parent(i);
        }
    }

    // Get parent index
    private int parent(int i) {
        return (i - 1) / 2;
    }

    // Swap helper
    private void swap(int i, int j) {
        int temp = heap[i];
        heap[i] = heap[j];
        heap[j] = temp;
    }

    // Extract the minimum
    public int extractMin() {
        if (size <= 0) return Integer.MAX_VALUE;
        if (size == 1) {
            size--;
            return heap[0];
        }
        int root = heap[0];
        heap[0] = heap[size - 1];
        size--;
        heapify(0);
        return root;
    }

    // Heapify Down
    private void heapify(int i) {
        int smallest = i;
        int left = 2 * i + 1;
        int right = 2 * i + 2;
        
        if (left < size && heap[left] < heap[smallest]) smallest = left;
        if (right < size && heap[right] < heap[smallest]) smallest = right;
        
        if (smallest != i) {
            swap(i, smallest);
            heapify(smallest);
        }
    }
}
```

---

## **Heap Sort**
### **What is Heap Sort?**
Heap Sort is a comparison-based sorting algorithm that uses a binary heap to sort an array.
- **Steps:**
  1. **Build a Max-Heap:** Convert the array into a heap.
  2. **Sort:** Swap the root (maximum element) with the last element of the heap, reduce the heap size by 1, and heapify the root.
  3. **Repeat:** Continue until the heap size is greater than 1.

### **Time Complexity:** `O(n log n)`

### **Java Code for Heap Sort**
```java
public class HeapSort {
    public void sort(int arr[]) {
        int n = arr.length;

        // Build heap (rearrange array)
        for (int i = n / 2 - 1; i >= 0; i--)
            heapify(arr, n, i);

        // One by one extract an element from heap
        for (int i = n - 1; i > 0; i--) {
            // Move current root to end
            int temp = arr[0];
            arr[0] = arr[i];
            arr[i] = temp;

            // call max heapify on the reduced heap
            heapify(arr, i, 0);
        }
    }

    void heapify(int arr[], int n, int i) {
        int largest = i; 
        int left = 2 * i + 1;
        int right = 2 * i + 2;

        if (left < n && arr[left] > arr[largest]) largest = left;
        if (right < n && arr[right] > arr[largest]) largest = right;

        if (largest != i) {
            int swap = arr[i];
            arr[i] = arr[largest];
            arr[largest] = swap;
            heapify(arr, n, largest);
        }
    }

    static void printArray(int arr[]) {
        for (int i : arr) System.out.print(i + " ");
        System.out.println();
    }

    public static void main(String args[]) {
        int arr[] = {12, 11, 13, 5, 6, 7};
        HeapSort hs = new HeapSort();
        hs.sort(arr);
        System.out.println("Sorted array:");
        printArray(arr);
    }
}
```

Let's break down the `sort` function step-by-step to explain how it performs **Heap Sort** with an example.

### **Overview of Heap Sort**
Heap Sort is a comparison-based sorting algorithm that uses a **binary heap** (either max-heap or min-heap) to sort elements. The basic idea is:
1. Build a **max-heap** from the input array.
2. Extract the maximum element (root of the heap) and move it to the end of the array.
3. Reduce the heap size and heapify the new root to maintain the max-heap property.
4. Repeat the process until the heap size is reduced to 1.

### **Explanation of the Code**

#### 1. **Building the Heap (`heapify` loop)**
```java
// Build heap (rearrange array)
for (int i = n / 2 - 1; i >= 0; i--)
    heapify(arr, n, i);
```
- This loop starts at the last non-leaf node (index `n/2 - 1`) and moves towards the root (index `0`), calling the `heapify` function to turn the array into a **max-heap**.
- **Why start from `n/2 - 1`?** In a binary heap, elements from `n/2` to `n-1` are leaf nodes. Leaf nodes are already heaps by themselves, so we only need to heapify non-leaf nodes.

#### 2. **Extracting Elements One by One**
```java
// One by one extract an element from heap
for (int i = n - 1; i > 0; i--) {
    // Move current root to end
    int temp = arr[0];
    arr[0] = arr[i];
    arr[i] = temp;

    // call max heapify on the reduced heap
    heapify(arr, i, 0);
}
```
- This loop runs from the end of the array (`n-1`) to the second element (`1`).
- **Step-by-step process:**
  1. **Swap Root with Last Element:** Swap the root (largest element in the max-heap) with the last element in the array. This moves the largest element to its correct position.
  2. **Reduce Heap Size:** The heap size is reduced by one since the largest element is now in its correct place.
  3. **Heapify:** Call `heapify` on the new root to restore the max-heap property for the reduced heap.

### **Dry Run Example**
Let's go through a dry run of the function with an example array:
```
Input: arr[] = [4, 10, 3, 5, 1]
```

#### **Step 1: Building the Max-Heap**
- **Initial array:** `[4, 10, 3, 5, 1]`
- The loop `for (int i = n / 2 - 1; i >= 0; i--)` will start at `i = 1` and go to `i = 0`.

1. **Heapify at index 1:** Subtree rooted at index 1: `[10, 5, 1]`
   - No changes needed as 10 is already the largest.
2. **Heapify at index 0:** Subtree rooted at index 0: `[4, 10, 3, 5, 1]`
   - Swap `4` with `10`. Now, the array becomes `[10, 4, 3, 5, 1]`.
   - Heapify on the affected subtree at index 1: `[4, 5, 1]`
   - Swap `4` with `5`. Now, the array becomes `[10, 5, 3, 4, 1]`.

- **Max-Heap built:** `[10, 5, 3, 4, 1]`

#### **Step 2: Extract Elements from the Heap**
Now, we extract the maximum element (root) and place it at the end of the array.

1. **Iteration 1 (i = 4):**
   - Swap root `10` with `arr[4]` (last element `1`).
   - Array: `[1, 5, 3, 4, 10]`
   - Heapify the new root at index 0 for heap size = 4:
     - Swap `1` with `5`. Array becomes `[5, 1, 3, 4, 10]`.
     - Heapify the affected subtree at index 1: Swap `1` with `4`. Array becomes `[5, 4, 3, 1, 10]`.

2. **Iteration 2 (i = 3):**
   - Swap root `5` with `arr[3]` (last element in the reduced heap `1`).
   - Array: `[1, 4, 3, 5, 10]`
   - Heapify the new root at index 0 for heap size = 3:
     - Swap `1` with `4`. Array becomes `[4, 1, 3, 5, 10]`.

3. **Iteration 3 (i = 2):**
   - Swap root `4` with `arr[2]` (last element in the reduced heap `3`).
   - Array: `[3, 1, 4, 5, 10]`
   - Heapify the new root at index 0 for heap size = 2:
     - Swap `3` with `1`. Array becomes `[3, 1, 4, 5, 10]`.

4. **Iteration 4 (i = 1):**
   - Swap root `3` with `arr[1]` (last element in the reduced heap `1`).
   - Array: `[1, 3, 4, 5, 10]`
   - Heap size = 1, no need to heapify.

### **Final Output:** `[1, 3, 4, 5, 10]`

### **Summary of Steps in the Code**
1. **Build a max-heap** from the input array.
2. **Iteratively swap** the root (maximum element) with the last element in the current heap.
3. **Reduce the heap size** and **heapify** the new root to maintain the max-heap property.
4. Repeat until the array is sorted.

### **Complexity**
- **Time Complexity:** `O(n log n)` - Building the heap takes `O(n)`, and each extraction (heapify) takes `O(log n)`.
- **Space Complexity:** `O(1)` - Sorting is done in-place, so no extra space is used.

This code sorts the input array using the heap data structure and is an in-place sorting algorithm.

---

## **Java’s PriorityQueue: Using Heaps Directly**
Java provides a built-in **PriorityQueue** class to handle Min-Heap and Max-Heap:
- **Min-Heap:** Default implementation.
  ```java
  Queue<Integer> minHeap = new PriorityQueue<>();
  ```
- **Max-Heap:** Using a comparator.
  ```java
  Queue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
  ```

### **Common Methods**
- **`add(E element)`:** Inserts an element.
- **`poll()`:** Retrieves and removes the head.


- **`peek()`:** Retrieves but does not remove the head.
- **`clear()`:** Removes all elements.
- **`size()`:** Returns the number of elements.

---

### **Summary**
- A **Heap** is a complete binary tree, mainly used for implementing priority queues.
- Heaps support efficient insertion, deletion, and extraction of the minimum/maximum element in `O(log n)` time.
- **Heap Sort** uses heaps to sort an array in `O(n log n)` time.
- Java's `PriorityQueue` provides a built-in way to implement heaps with additional methods for efficient priority queue operations.
  
---------------------------------------

### Heaps in Java Using `PriorityQueue`

Java provides a built-in class named `PriorityQueue` in the `java.util` package to easily implement both **Min-Heap** and **Max-Heap** data structures. This class allows us to handle elements in a heap-like structure, supporting operations such as insertion, deletion, and retrieval of the minimum or maximum elements efficiently.

By default, **`PriorityQueue` implements a Min-Heap**, which means the smallest element is always at the root. However, you can modify it to act as a Max-Heap using a **custom comparator**.

### **1. Creating Heaps in Java with `PriorityQueue`**

#### **Syntax for Implementing Heaps**
- **Min-Heap (Default):**
  ```java
  Queue<Integer> minHeap = new PriorityQueue<>();
  ```
  This creates a Min-Heap, where the smallest element is at the root.

- **Max-Heap (Using Comparator):**
  ```java
  Queue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
  ```
  This creates a Max-Heap by reversing the natural ordering of elements, so the largest element is at the root.

### **2. Common Methods of `PriorityQueue`**

Here are some useful methods provided by the `PriorityQueue` class:
- **`add(E element)`:** Inserts the specified element into the priority queue.
- **`poll()`:** Retrieves and removes the head (root) of the queue. Returns `null` if the queue is empty.
- **`peek()`:** Retrieves, but does not remove, the head of the queue. Returns `null` if the queue is empty.
- **`remove()`:** Removes a specific element from the queue, if present.
- **`clear()`:** Removes all elements from the queue.
- **`size()`:** Returns the number of elements currently in the queue.

> **Note:** These methods work for both Min-Heap and Max-Heap implementations of the `PriorityQueue`.

### **3. Example: Using `PriorityQueue` for Max-Heap and Min-Heap**
The following code demonstrates how to create a Max-Heap and a Min-Heap using `PriorityQueue`, add elements, and extract them in sorted order.

#### **Java Code Implementation**
```java
import java.util.Collections;
import java.util.PriorityQueue;
import java.util.Queue;

public class HeapExample {
    public static void main(String[] args) {
        // DECLARING MAX HEAP
        Queue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
        
        // Add elements to the Max Heap
        maxHeap.add(10);
        maxHeap.add(30);
        maxHeap.add(20);
        maxHeap.add(5);
        maxHeap.add(1);
        
        // Print elements in the order they are removed from the Max Heap
        System.out.print("Elements removed from Max Heap in descending order:\n");
        while (!maxHeap.isEmpty()) {
            // Print and remove the top element
            System.out.print(maxHeap.poll() + " ");
        }

        // DECLARING MIN HEAP
        Queue<Integer> minHeap = new PriorityQueue<>();
        
        // Add elements to the Min Heap
        minHeap.add(10);
        minHeap.add(30);
        minHeap.add(20);
        minHeap.add(5);
        minHeap.add(1);
        
        // Print elements in the order they are removed from the Min Heap
        System.out.print("\n\nElements removed from Min Heap in ascending order:\n");
        while (!minHeap.isEmpty()) {
            // Print and remove the top element
            System.out.print(minHeap.poll() + " ");
        }
    }
}
```

#### **Output:**
```
Elements removed from Max Heap in descending order:
30 20 10 5 1 

Elements removed from Min Heap in ascending order:
1 5 10 20 30 
```

### **4. Detailed Explanation of the Code**
1. **Max-Heap Creation:**
   - We use `PriorityQueue<>(Collections.reverseOrder())` to create a Max-Heap.
   - We add elements in arbitrary order.
   - The elements are extracted in descending order due to the Max-Heap property.
   
2. **Min-Heap Creation:**
   - We use the default constructor `new PriorityQueue<>()` to create a Min-Heap.
   - Elements are added in arbitrary order.
   - The elements are extracted in ascending order because of the Min-Heap property.

3. **Using `poll()` Method:**
   - Both heaps use the `poll()` method to retrieve and remove the head of the queue (root of the heap). In a Max-Heap, it retrieves the largest element, and in a Min-Heap, it retrieves the smallest element.

4. **Why Use `PriorityQueue`?**
   - The `PriorityQueue` class efficiently maintains the heap property with insertion (`add()`) and removal (`poll()`) operations in `O(log n)` time.
   - This makes `PriorityQueue` an excellent choice for implementing priority queues where you need to process elements based on their natural order or priority.

### **5. Additional Example: Custom Comparator**
If you want to create a Max-Heap of a custom class, you can use a comparator:
```java
import java.util.PriorityQueue;
import java.util.Queue;
import java.util.Comparator;

class Person {
    String name;
    int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

public class CustomHeap {
    public static void main(String[] args) {
        // Max-Heap based on the age of the person
        Queue<Person> maxHeap = new PriorityQueue<>(new Comparator<Person>() {
            public int compare(Person p1, Person p2) {
                return Integer.compare(p2.age, p1.age); // Reverse order for Max-Heap
            }
        });

        maxHeap.add(new Person("Alice", 30));
        maxHeap.add(new Person("Bob", 20));
        maxHeap.add(new Person("Charlie", 25));
        
        // Print people in descending order of age
        while (!maxHeap.isEmpty()) {
            Person person = maxHeap.poll();
            System.out.println(person.name + ": " + person.age);
        }
    }
}
```

#### **Output:**
```
Alice: 30
Charlie: 25
Bob: 20
```

### **6. Important Points to Remember**
- **Min-Heap by Default:** `PriorityQueue` is a Min-Heap by default.
- **Custom Comparator for Max-Heap:** To implement a Max-Heap, use `Collections.reverseOrder()` or provide a custom comparator.
- **Operations and Complexity:** 
  - `add()`, `poll()`, and `remove()` have a time complexity of `O(log n)`.
  - `peek()` has a time complexity of `O(1)`.
- **Usage:** `PriorityQueue` is commonly used for tasks like **task scheduling**, **Dijkstra's algorithm**, and any situation requiring **dynamic retrieval** of the highest or lowest priority elements.

### **7. Why Use Heaps in Java?**
- Heaps provide an efficient way to manage a collection of elements where you need quick access to the largest or smallest element.
- Using `PriorityQueue` simplifies the implementation, as you do not need to manually manage the heap properties during insertion or deletion.

### **Conclusion**
- The `PriorityQueue` class in Java is a versatile tool for implementing heaps.
- By default, it behaves as a Min-Heap, but with a custom comparator, it can easily function as a Max-Heap.
- Understanding and using `PriorityQueue` can help solve problems that require dynamic retrieval of minimum or maximum elements efficiently.

These detailed notes and examples should give a beginner a clear understanding of how to work with heaps in Java using the `PriorityQueue` class. The provided code snippets demonstrate how to use both Min-Heap and Max-Heap in practical scenarios.

