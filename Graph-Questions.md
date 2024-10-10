## Question 1 - Problem: **[Clone a Graph](https://leetcode.com/problems/clone-graph/)**

The problem is to clone (or create a deep copy of) a graph. A graph is made up of **nodes** and **edges**. Each node can have multiple **neighbors** (which are connected nodes).

In this problem, the input is a single node of the graph, and you have to return the "clone" of the entire graph. The clone should have the same structure, i.e., all nodes and their neighbors must be copied without sharing any of the original objects.

### Graph Structure:

Each node has:
- A value (`val`): an integer that represents the identity of the node.
- A list of **neighbors** (`neighbors`): a list of nodes connected to the current node.

### Key Concept:
- **Graph Traversal**: To solve this problem, we need to traverse through the graph and visit each node. For each node, we create a new copy (clone) and connect it to its cloned neighbors.

- **HashMap to Track Visited Nodes**: Since graphs can have cycles (where nodes can be visited more than once), we use a `visited` hashmap. It helps to keep track of nodes we've already cloned, so we don't clone the same node multiple times or fall into infinite loops in case of cycles.

### Solution Breakdown:

1. **Base Case:**
   - If the graph is empty (`node == null`), return `null`.
   
2. **Check if Node is Already Visited:**
   - If the node has already been visited (cloned), return the cloned node from the `visited` hashmap to avoid re-cloning it.

3. **Create a New Node:**
   - If the node hasn’t been cloned yet, create a new node with the same value as the original.
   - Store this cloned node in the `visited` hashmap so that if this node appears again during traversal, we return the already cloned node.

4. **Clone Neighbors:**
   - For each of the original node’s neighbors, recursively clone them and add the cloned neighbors to the current node's `neighbors` list.

5. **Return the Cloned Node:**
   - After cloning all neighbors, return the newly cloned node.

### Code Explanation:

```java
class Solution {

    // HashMap to keep track of all the visited and cloned nodes
    HashMap<Node, Node> visited = new HashMap<>();

    public Node cloneGraph(Node node) {

        // If the input node is null, return null
        if (node == null) {
            return node;
        }

        // If the node is already visited, return the cloned node from the visited map
        if (visited.containsKey(node)) {
            return visited.get(node);
        }

        // Create a clone for the given node
        // The cloned node has the same value but initially an empty list of neighbors
        Node cloneNode = new Node(node.val, new ArrayList<>());

        // Save this node in the visited map to avoid re-cloning it
        visited.put(node, cloneNode);

        // For every neighbor of the original node, recursively clone it and add it to the neighbors of the clone node
        for (Node neighbor : node.neighbors) {
            cloneNode.neighbors.add(cloneGraph(neighbor));
        }

        // Return the cloned node
        return cloneNode;
    }
}
```

### Step-by-Step Example:

Consider this graph:
```
   1
  / \
 2   4
 |   |
 3---4
```

- Node `1` has neighbors `2` and `4`.
- Node `2` has neighbors `1` and `3`.
- Node `3` has neighbors `2` and `4`.
- Node `4` has neighbors `1` and `3`.

### Execution:

- Start at **Node 1**:
  - Clone `1` and add it to the `visited` map.
  - For its neighbors (`2` and `4`):
    - Move to **Node 2**:
      - Clone `2`, add it to the `visited` map.
      - Move to its neighbors (`1` and `3`):
        - **Node 1** is already visited, so return the cloned version from the `visited` map.
        - Move to **Node 3**:
          - Clone `3`, add it to the `visited` map.
          - Move to its neighbors (`2` and `4`):
            - **Node 2** is already visited, so return the cloned version from the `visited` map.
            - Move to **Node 4**:
              - Clone `4`, add it to the `visited` map.
              - **Node 3** is already visited, so return the cloned version from the `visited` map.
    - Now clone **Node 4** similarly as above.

### Text Diagram of the Process:

1. **Original Graph**:

```
1 -- 2
|    |
4 -- 3
```

2. **Cloning Step-by-Step**:

- Start at Node `1` → clone `1`
- Go to Node `2` → clone `2`
- Go to Node `3` → clone `3`
- Go to Node `4` → clone `4`

The cloned structure is created recursively and connected by their neighbors just like the original graph.

### Why this works:

- The hashmap `visited` ensures that each node is cloned exactly once, preventing cycles from being revisited endlessly.
- The recursive approach ensures we visit all nodes connected to the original node, thus covering the entire graph.

### Time Complexity:

- **O(V + E)**, where `V` is the number of vertices (nodes) and `E` is the number of edges (connections between nodes). Each node and edge is visited once during the traversal.

### Space Complexity:

- **O(V)** for the recursion stack and hashmap, where `V` is the number of nodes. Each node is stored in the `visited` hashmap, and recursion requires stack space proportional to the depth of the graph.

### Summary:

This solution uses **Depth First Search (DFS)** to traverse and clone the graph. It keeps track of cloned nodes using a `HashMap`, and for each node, it recursively clones all its neighbors, ensuring a complete deep copy of the graph.

-------

### Time Complexity Explanation:

The time complexity of this solution is **O(V + E)**, where:
- `V` is the number of **vertices** (nodes) in the graph.
- `E` is the number of **edges** (connections between nodes).

#### Breakdown:
1. **O(V) for visiting each node (vertex)**:
   - The algorithm visits every node in the graph exactly once. Each node is cloned, and its neighbors are processed.
   - In the worst case, the graph is connected, meaning every node can reach every other node directly or indirectly. Therefore, the algorithm will visit all `V` nodes.

2. **O(E) for visiting each edge**:
   - For every node, we also visit all its neighbors. These connections between nodes are called **edges**.
   - In a graph, every edge is traversed once while processing the neighbors. Since we have to process all edges to correctly clone the graph, the time complexity accounts for this as well.

Thus, the total time complexity is **O(V + E)**, because the algorithm needs to process both the nodes and the edges of the graph.

### Space Complexity Explanation:

The space complexity is **O(V)**, where `V` is the number of vertices (nodes) in the graph. Let's break it down:

1. **Visited HashMap**:
   - We use a **HashMap** called `visited` to store each node and its clone.
   - Since we clone every node once and store it in the HashMap, this requires **O(V)** space.
   - For `V` nodes, the HashMap will store `V` keys (the original nodes) and `V` values (the cloned nodes).

2. **Recursive Call Stack**:
   - The recursion depth can be up to the number of nodes in the graph, as the graph can be connected, and the recursive call stack will hold at most `V` function calls at once (in the worst case).
   - In the worst case, the recursion will go through all nodes in a chain-like structure, requiring **O(V)** space for the stack.

3. **Neighbors List Storage**:
   - Every node has a list of neighbors, and these lists are recreated during the cloning process. This doesn’t increase the overall space complexity significantly beyond **O(V)**, as each node’s neighbors are also nodes from the graph.
   
4. **Auxiliary space for neighbor lists**:
   - The neighbor list for each node is stored as an ArrayList, and since each node is visited and its neighbors copied, this also adds up to space proportional to the number of nodes, i.e., **O(V)**.

### Summary:

- **Time Complexity**: **O(V + E)** where `V` is the number of nodes (vertices) and `E` is the number of connections (edges).
  - We visit every node once and process every connection once.
  
- **Space Complexity**: **O(V)**, which is primarily due to the `visited` HashMap and recursion stack.
  - The HashMap and recursive call stack both need to store information proportional to the number of nodes in the graph.
 
#### Why the time complexity in a graph is different from a tree and why the time complexity in a graph is O(V + E), while in a tree it's often considered O(V), even though both involve visiting nodes.

That's a great question! Let's clarify why the time complexity in a graph is different from a tree and why the time complexity in a graph is **O(V + E)**, while in a tree it's often considered **O(V)**, even though both involve visiting nodes.

### Key Differences Between Trees and Graphs:

1. **Tree Structure**:
   - A tree is a **connected, acyclic graph**, meaning it has no cycles and there is exactly one path between any two nodes.
   - In a tree with `V` nodes, there are always exactly `V - 1` edges (connections between nodes). The number of edges is proportional to the number of nodes.
   - When you traverse a tree (like in depth-first search or breadth-first search), you visit each node exactly once, and you follow each edge once to reach the child nodes.

   - **Time Complexity in Trees**:
     - When traversing a tree, the time complexity is usually stated as **O(V)**, where `V` is the number of nodes, because:
       - Each node is visited once.
       - The number of edges is always `V - 1`, which is also linear with respect to `V`.
       - The total work is proportional to the number of nodes, making the time complexity **O(V)**.

2. **Graph Structure**:
   - A graph is a more general structure than a tree. It can have **cycles** (nodes connected in loops) and **multiple paths** between nodes.
   - A graph can have **more edges** than a tree. In fact, a graph with `V` vertices can have up to `V * (V - 1) / 2` edges in a dense graph (in the case of a complete graph, where every node is connected to every other node).
   - A graph can also be **disconnected**, meaning it has separate components that are not reachable from one another.

   - **Time Complexity in Graphs**:
     - In a graph, the time complexity becomes **O(V + E)** because:
       - You need to visit every node once (just like in a tree), which contributes **O(V)** to the time complexity.
       - However, unlike a tree, a graph can have **more edges**. You have to process each edge (or connection between nodes) at least once to ensure you've explored the entire graph. This contributes an additional **O(E)**, where `E` is the number of edges.
     - Therefore, in a graph, the total time complexity is **O(V + E)**, because:
       - **O(V)**: Each node is visited once.
       - **O(E)**: Each edge is also processed once to explore its connected nodes.

### Example to Illustrate the Difference:

#### Tree Example:
Consider a simple binary tree with 5 nodes:
```
    1
   / \
  2   3
 / \
4   5
```
- **Nodes (V)**: 5
- **Edges (E)**: 4 (the edges are 1→2, 1→3, 2→4, and 2→5)

For a tree traversal (like DFS or BFS), we:
- Visit each node once.
- Traverse each edge once.

Time complexity = **O(V) = O(5)** (since there are 5 nodes).

#### Graph Example:
Consider a graph with 5 nodes and more edges:
```
   1 - 2
   | / |
   3 - 4
   |
   5
```
- **Nodes (V)**: 5
- **Edges (E)**: 6 (the edges are 1→2, 1→3, 2→3, 2→4, 3→4, and 3→5)

In this graph:
- We still visit each node once.
- However, there are more edges to process. Each edge connects nodes that need to be explored.

Time complexity = **O(V + E) = O(5 + 6) = O(11)**.

### Why Do We Count Edges Separately in Graphs?
- **In a tree**, the number of edges is always proportional to the number of nodes (`V - 1` edges), so we don't count edges separately.
- **In a graph**, the number of edges can vary dramatically. In a sparse graph, there might be only a few edges, but in a dense graph, there could be many more edges than nodes. To accurately represent the work needed to traverse a graph, we need to account for both nodes **and** edges, since both impact the overall time complexity.

### Conclusion:
- **Trees**: Since the number of edges is always proportional to the number of nodes, we only need to account for the number of nodes, giving **O(V)** time complexity.
- **Graphs**: Since graphs can have a varying number of edges (often more than nodes), we need to account for both the nodes and the edges, leading to **O(V + E)** time complexity.

This distinction is critical for understanding why graph algorithms have a different time complexity compared to tree algorithms, even though both structures involve nodes and edges.

-------

Let's break down your two questions:

### 1. Why does a graph with **V** vertices have up to **V * (V - 1) / 2** edges in a dense graph (complete graph)?

In a **complete graph**, every node (vertex) is connected to every other node. So, for **V** vertices, each vertex can have an edge to every other vertex except itself. Let's understand this with an example.

#### Example of a Complete Graph:

Suppose we have **4 vertices** labeled **A**, **B**, **C**, and **D**. In a complete graph, the connections between these vertices would look like this:

- **A** is connected to **B**, **C**, and **D**.
- **B** is connected to **A**, **C**, and **D**.
- **C** is connected to **A**, **B**, and **D**.
- **D** is connected to **A**, **B**, and **C**.

This graph has every possible edge between vertices. Let's count the edges:
- **A** has 3 edges: **A-B**, **A-C**, **A-D**.
- **B** has 3 edges: **B-A**, **B-C**, **B-D** (but we already counted **B-A** as **A-B**, so only **B-C** and **B-D** are new).
- **C** has 3 edges: **C-A**, **C-B**, **C-D** (but we already counted **C-A** and **C-B**, so only **C-D** is new).
- **D** has 3 edges: **D-A**, **D-B**, **D-C** (but all of these were already counted).

Thus, the total number of edges is 6: **A-B**, **A-C**, **A-D**, **B-C**, **B-D**, **C-D**.

Now, to generalize this for **V** vertices:
- Each vertex can connect to **V - 1** other vertices (since it can't connect to itself).
- But we need to avoid double-counting edges (e.g., **A-B** and **B-A** are the same edge). So, we divide the total number of connections by 2.

This gives us the formula for the maximum number of edges in a complete graph:
\[ \text{Maximum edges} = \frac{V \times (V - 1)}{2} \]

For **V = 4**:
\[
\frac{4 \times (4 - 1)}{2} = \frac{4 \times 3}{2} = 6
\]
Thus, the complete graph with 4 vertices has 6 edges.

In general, for **V = 5**:
\[
\frac{5 \times (5 - 1)}{2} = \frac{5 \times 4}{2} = 10
\]
For **V = 6**:
\[
\frac{6 \times (6 - 1)}{2} = \frac{6 \times 5}{2} = 15
\]

As the number of vertices increases, the number of edges grows quadratically, making the graph very dense.

---

### 2. Why do we not keep the complexity only **O(E)** if we process edges?

This is a great question, and it dives into the nature of graph traversal algorithms.

#### Graph Traversal with Both Vertices and Edges:
When processing a graph, we typically:
1. **Visit each vertex (node)**.
2. **Explore all edges connected to that vertex**.

Now, why isn't the time complexity **O(E)** only if edges are the main thing we're processing?

- **O(E)** counts only the edges, but what about the **vertices (nodes)**?
  - For example, when you visit a node, you need to find its neighbors (which are the edges). This takes some work per node. 
  - Each node could have multiple edges, so it's important to account for the work of processing both the nodes and the edges.
  
- In a sparse graph (few edges), most of the time is spent just visiting the nodes.
- In a dense graph (many edges), the work done on processing edges becomes more significant.

Therefore, the **total work** is a combination of:
- **O(V)** for visiting the nodes.
- **O(E)** for processing the edges.

#### Why not just **O(E)**?
In sparse graphs, where the number of edges is much smaller than the number of nodes, **O(E)** doesn't capture the full work being done. You still have to visit every node, even if there are few edges.

Let’s break it down with an example:

#### Sparse Graph Example:
- Suppose you have 5 nodes, but only 2 edges (a very sparse graph).
```
   A---B
   C   D   E
```
Here, **V = 5** and **E = 2**.

- If we only count the edges, **O(E)** would be **O(2)**, but we still have to visit the other nodes **C**, **D**, and **E** even though they don't have any edges. So, the complexity has to include **O(V)**, making it **O(V + E)**.

#### Dense Graph Example:
- Now, take a complete graph with 5 nodes:
```
   A---B---C---D---E
    \  / \  / \  /
     E---D---C---A
```
Here, **V = 5** and **E = 10**.

- In this case, **O(E)** is much larger than **O(V)**, but we still include **O(V)** because we are visiting each node. Therefore, the time complexity is **O(V + E)**, capturing both aspects of processing the graph.

#### Conclusion:
We don't just use **O(E)** because we need to account for both the **vertices (nodes)** and **edges**. In sparse graphs, **O(V)** can dominate, while in dense graphs, **O(E)** can dominate, so we combine them into **O(V + E)** to cover both cases.

This way, the complexity reflects both the time spent visiting the nodes and the time spent processing the edges, giving us a complete understanding of the work needed to traverse the graph.

-------

In the case of both **undirected** and **directed graphs**, the time complexity for traversing the graph is generally expressed as **O(V + E)**, not **O(V + E/2)**, even though an undirected graph might have fewer "unique" edges due to how edges are counted.

Let me explain why:

### 1. **Undirected Graphs:**
In an **undirected graph**, each edge connects two nodes, but you can think of it as being counted twice in some sense because each node refers to the edge once. For example, if there's an edge between **A** and **B**, it can be considered as:
- **A** having an edge to **B**.
- **B** having an edge to **A**.

Even though it's the same physical edge, it's counted once for each node, effectively making the edge "appear" twice.

#### Why not **O(V + E/2)** for undirected graphs?

- **E** refers to the total number of edges. For an undirected graph, while an edge between two nodes might be counted twice (once for each node), the algorithm will still process this edge twice in practice (once when you visit each node).
- The traversal algorithm, whether it’s **DFS** or **BFS**, will visit each vertex and explore all its edges. For undirected graphs, it doesn't "skip" processing the edge twice. Even though it's the same edge, it counts as two steps because the algorithm considers it once when visiting node **A** and once when visiting node **B**.

This is why we still express the complexity as **O(V + E)**, even though each edge might seem to be counted twice. **E** represents the total number of "references" to edges, not just the unique number of edges. The graph traversal algorithm still does work on each reference.

### Example:

In an undirected graph with 3 nodes and 2 edges:
```
  A -- B -- C
```

- There are 2 edges: **A-B** and **B-C**.
- When **A** is visited, it will explore the edge **A-B**.
- When **B** is visited, it will explore the edges **A-B** and **B-C**.
- When **C** is visited, it will explore the edge **B-C**.

Even though **A-B** and **B-C** are the same edges being visited from different directions, the algorithm still processes them individually, contributing to **E** operations.

Thus, even in undirected graphs, we count **E** rather than **E/2** because every edge is visited once for each endpoint.

### 2. **Directed Graphs:**
In a **directed graph**, each edge has a direction, meaning each edge only goes one way (e.g., from **A** to **B**, but not necessarily from **B** to **A**).

- In this case, the algorithm still processes **E** edges, but each edge is uniquely counted because it represents a one-way connection.

So, for directed graphs, the complexity remains **O(V + E)** since there's no double-counting; each edge has a specific direction and is visited once as part of the traversal.

### Summary:
The reason why the time complexity is **O(V + E)** and not **O(V + E/2)** (even in undirected graphs) is that:
1. In **undirected graphs**, even though edges may be "referred to" twice (once from each endpoint), they still contribute to the total work done by the algorithm.
2. In **directed graphs**, each edge is distinct and only counted once in the traversal.

Therefore, **O(V + E)** accurately captures the amount of work done by graph traversal algorithms, regardless of whether the graph is directed or undirected.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Question 2 - **[Course Schedule](https://leetcode.com/problems/course-schedule/)**

You are given:
1. **`numCourses`** - the number of courses, labeled from 0 to `numCourses - 1`.
2. **`prerequisites`** - a list of pairs of courses where each pair `[a, b]` means that course `a` depends on course `b` (i.e., you must complete course `b` before you can take course `a`).

**Goal**: Determine if you can complete all the courses given the prerequisite conditions.

This problem is asking us to detect whether there is a **cycle** in the course dependency graph. If there is a cycle, it means there's a circular dependency, and you cannot finish all the courses. If no cycle exists, it's possible to finish the courses.

### Graph Explanation
The courses and their prerequisites can be visualized as a **directed graph**:
- Each course is represented as a **node**.
- Each prerequisite pair `[a, b]` is a directed **edge** from node `b` to node `a`, meaning you must complete `b` before you can take `a`.

The problem is now equivalent to **detecting a cycle** in a directed graph.

### Solution Breakdown:

#### 1. **Building the Graph**:
We first create a graph using a **HashMap** where:
- Each **key** represents a course.
- The **value** (a list) represents the list of courses that depend on the key course.

The graph is built by going through the prerequisites and adding them to the map.

#### 2. **Detecting Cycles**:
To check whether it's possible to finish all courses, we need to detect whether there is a cycle in the graph. We do this using **Depth-First Search (DFS)**. If we revisit a course that is already being processed (i.e., it’s in the current recursion stack), it means we've found a cycle.

We use a **visited set** to keep track of the courses that are currently being processed. If we encounter a course that's already in the visited set, it means there's a cycle.

### Code Walkthrough (Step-by-Step):

```java
class Solution {
  public boolean canFinish(int numCourses, int[][] prerequisites) {

      // Step 1: Build the course graph using HashMap
      HashMap<Integer, List<Integer>> courseGraph = new HashMap<>();

      // For each prerequisite pair, we add the course as a dependency of another course
      for(int[] pre: prerequisites){
          // If course b already exists in the graph
          if(courseGraph.containsKey(pre[1])){
              courseGraph.get(pre[1]).add(pre[0]); // Add course a as dependent on b
          }
          else{
              // If it's a new course, create a new list for its dependencies
              List<Integer> nextCourses = new LinkedList<>();
              nextCourses.add(pre[0]);
              courseGraph.put(pre[1], nextCourses); // Add the dependency
          }
      }
      
      // Step 2: Use a HashSet to track the visited nodes during DFS
      HashSet<Integer> visited = new HashSet<>();
      
      // Step 3: Check each course to see if there's a cycle
      for(int currentCourse = 0; currentCourse < numCourses; currentCourse++){
          // If there is a cycle, return false
          if(courseSchedule(currentCourse, visited, courseGraph) == false){
              return false;
          }
      }
      // If no cycle is found, return true
      return true;
    }
    
    // Helper function to perform DFS and check for cycles
    public boolean courseSchedule(int course, HashSet<Integer> visited, 
                                  HashMap<Integer, List<Integer>> courseGraph) {
        // Step 4: If the course is already in the visited set, we've detected a cycle
        if(visited.contains(course)){
            return false;
        }
        
        // Step 5: If the course has no more dependencies, return true
        if(courseGraph.get(course) == null){
            return true;
        }
        
        // Step 6: Mark the course as visited
        visited.add(course);
        
        // Step 7: Check all the dependencies (prerequisites) of this course
        for(int pre: courseGraph.get(course)){
            if(courseSchedule(pre, visited, courseGraph) == false){
                return false;
            }
        }
        
        // Step 8: Remove the course from the visited set once done with this path
        visited.remove(course);
        
        // Step 9: Mark this course as "checked" by setting its value to null
        courseGraph.put(course, null);
        
        return true; // No cycle detected for this course
    }
}
```

### Text Diagram (Example):

Let’s assume we have 3 courses and their prerequisites are `[[1, 0], [2, 1]]`. This means:
- Course 1 depends on course 0.
- Course 2 depends on course 1.

We can visualize this as a graph:

```
0 → 1 → 2
```

- **Node 0** has an edge to **node 1** (because you must take course 0 before course 1).
- **Node 1** has an edge to **node 2** (because you must take course 1 before course 2).

This is a **linear dependency** without a cycle, so you can finish the courses.

If the prerequisites were `[[1, 0], [0, 1]]`, the graph would be:

```
0 → 1
1 → 0
```

This creates a **cycle** because course 0 depends on course 1, and course 1 depends on course 0. Therefore, it's impossible to finish the courses.

### Key Points for Beginners:
1. **Graph Representation**: We use a `HashMap<Integer, List<Integer>>` to represent the graph, where each course points to its dependent courses.
2. **Cycle Detection**: We use **Depth-First Search (DFS)** to detect cycles. If we revisit a node that's already being processed, we know there’s a cycle.
3. **Visited Set**: The `visited` set helps us track the courses we're currently visiting. If a cycle is found, we return `false`.
4. **No Cycles**: If no cycles are detected, we return `true`, meaning we can finish all the courses.

### Time Complexity:
- Building the graph takes **O(P)**, where `P` is the number of prerequisites.
- Checking for cycles takes **O(V + E)**, where:
  - **V** is the number of courses (nodes).
  - **E** is the number of dependencies (edges).

### Space Complexity:
- The space complexity is **O(V + E)**, as we store the graph and visited set.

### Dry Run Example (Including Cycle)

Let’s run through another **dry run** where we include a cycle in the course prerequisites to better understand how the code detects cycles and prevents completing the courses.

Suppose we have `4` courses and the following prerequisites:

```
numCourses = 4
prerequisites = [[1, 0], [2, 1], [0, 2], [3, 1]]
```

This means: 
- Course 1 depends on course 0. // Put right side as starting node and left side as ending node.
- Course 2 depends on course 1.
- **Course 0 depends on course 2 (cycle)**.
- Course 3 depends on course 1.

### Graph Representation:
The graph looks like this:

```
    0 → 1 → 2
     ↑  ↓   ↓ (Cycle detected)
     └──────┘
        3 
```

We have a cycle between courses 0, 1, and 2. Let’s walk through how the algorithm detects this cycle and avoids allowing the courses to be completed.

courseGraph = {0: [1], 1: [2, 3], 2: [0]} 
--------------
### Step-by-Step Dry Run:

#### 1. **Building the Graph**:

For each prerequisite pair `[a, b]`, we add `a` as a dependency of `b`:
- For `[1, 0]`: Course 1 depends on course 0 → `courseGraph = {0: [1]}`
- For `[2, 1]`: Course 2 depends on course 1 → `courseGraph = {0: [1], 1: [2]}`
- For `[0, 2]`: Course 0 depends on course 2 → `courseGraph = {0: [1], 1: [2], 2: [0]}`
- For `[3, 1]`: Course 3 depends on course 1 → `courseGraph = {0: [1], 1: [2, 3], 2: [0]}`

At this point, the graph is complete, and we will start the **DFS traversal** for each course.

#### 2. **DFS on Each Course**:

We now try to see if we can finish all courses by checking each course using DFS.

- **For course 0**:
  - Call `courseSchedule(0, visited, courseGraph)`:
    - `visited = {0}`
    - Course 0 has a prerequisite, so we check course 1 next → call `courseSchedule(1, visited, courseGraph)`.
    
- **For course 1**:
  - Call `courseSchedule(1, visited, courseGraph)`:
    - `visited = {0, 1}`
    - Course 1 has two prerequisites: course 2 and course 3. We check course 2 first → call `courseSchedule(2, visited, courseGraph)`.

- **For course 2**:
  - Call `courseSchedule(2, visited, courseGraph)`:
    - `visited = {0, 1, 2}`
    - Course 2 has a prerequisite on course 0 → call `courseSchedule(0, visited, courseGraph)`.

- **Cycle detected**:
  - Since `course 0` is already in the `visited` set, it means we have encountered a **cycle**. We return `false`, indicating that it's impossible to finish the courses because of the cycle.
  - Backtrack and return `false` all the way up the recursion stack. The final output will be `false`.

#### Dry Run Summary:
- The DFS starts at course 0, visits course 1, then visits course 2. When we attempt to visit course 0 again from course 2, we detect a cycle because course 0 is already in the `visited` set.
- As soon as a cycle is detected, the function returns `false`, indicating that it’s impossible to complete all courses.

### Code Breakdown with Relation to Example

Let's relate portions of the code to the dry run:

```java
// Step 1: Building the graph
for(int[] pre: prerequisites){
    if(courseGraph.containsKey(pre[1])){
        courseGraph.get(pre[1]).add(pre[0]); // Add dependencies
    }
    else{
        List<Integer> nextCourses = new LinkedList<>();
        nextCourses.add(pre[0]);
        courseGraph.put(pre[1], nextCourses); // Initialize new list of dependencies
    }
}
// After this, courseGraph is: {0: [1], 1: [2, 3], 2: [0]}
```

- **Why `LinkedList`**: Since we are only adding courses dynamically and not accessing elements by index, `LinkedList` works well.

Why are we using `List<Integer> nextCourses = new LinkedList<>();` and not a regular `List` implementation?

In Java, `LinkedList` and `ArrayList` both implement the `List` interface. The choice of using `LinkedList` vs. `ArrayList` depends on performance characteristics:

- **`LinkedList`** is good for operations where you frequently add or remove elements at the beginning or middle of the list. Since it stores elements as nodes, it allows for efficient insertions and deletions at any position. 
- **`ArrayList`**, on the other hand, is backed by an array, so inserting or removing elements in the middle or beginning requires shifting all other elements, which can be more expensive. However, it offers better access time if you need to frequently retrieve elements at specific indices (O(1) for access).

In this case, we use `LinkedList` because we are mostly adding elements (dependencies) dynamically, and it's faster to add in `LinkedList` compared to `ArrayList`. However, either would work because we're not accessing elements by index.

```java
// Step 2: Start DFS for each course
for(int currentCourse = 0; currentCourse<numCourses; currentCourse++ ){
    if(courseSchedule(currentCourse,visited,courseGraph) == false){
        return false; // Return false if any cycle is detected
    }
}
```

- **Why DFS**: DFS allows us to follow the entire dependency chain of a course before backtracking. In the case of cycles, if a course is revisited during the same DFS path, we can detect a cycle.

- **Backtracking**: DFS naturally supports backtracking, allowing us to explore one complete dependency chain at a time. If we detect a cycle, we can immediately stop and return `false`.
- **Cycle detection**: DFS is particularly well-suited for detecting cycles in a directed graph (which this problem essentially is). In DFS, if we revisit a node that is already in the `visited` set, it means there's a cycle.
- **Stack-like behavior**: DFS works well when the problem involves exploring deep dependencies (as in this problem) because it processes each dependency chain (stack-like behavior).

#### Why not BFS?
- **BFS (Breadth-First Search)** is better for finding the shortest path or exploring all neighbors at a given depth level, but it’s not well-suited for detecting cycles in this type of problem. It processes nodes level by level and doesn’t have an inherent mechanism for backtracking and cycle detection.

```java
// Step 3: The courseSchedule method (DFS logic)
public boolean courseSchedule (int course, HashSet<Integer> visited, 
                               HashMap<Integer, List<Integer>> courseGraph){
    if(visited.contains(course)){
        return false; // Cycle detected
    }
    
    if(courseGraph.get(course) == null){
        return true; // No more dependencies, safe to continue
    }
    
    visited.add(course); // Mark course as visited
    for(int pre: courseGraph.get(course)){ // Check all prerequisites
        if(courseSchedule(pre, visited, courseGraph ) == false){
            return false; // If cycle detected in any of the prerequisites
        }
    }
    visited.remove(course); // Backtrack
    courseGraph.put(course, null); // Mark course as fully processed
    return true;
}
```

- **Why `visited.remove(course)`**: We remove the course from `visited` after processing to allow it to be reprocessed correctly in another DFS path.

Why are we removing `visited.remove(course)` after adding?

This part of the code:

```java
visited.remove(course);
```

is critical for handling backtracking in the DFS algorithm. When performing Depth-First Search (DFS), the goal is to explore each node's dependencies and return to the previous state after exploring all paths. Here's the sequence:

1. **When we visit a node** (`course`), we add it to the `visited` set to track the nodes that are currently being processed.
2. **Once we finish processing that node's subtree** (i.e., all of its dependent courses), we remove it from the `visited` set so that it doesn’t interfere with further DFS calls.
3. **Why remove?** The `visited` set only tracks nodes being actively processed. Removing the node ensures that if we visit the same node again during a different DFS path, we can reprocess it correctly.
   
- **Why `courseGraph.put(course, null)`**: This is an optimization step. Once a course and all its dependencies are processed, we set its value to `null` to avoid rechecking it.

This is an **optimization**. After processing a course's dependencies, we set its value to `null` in the graph to indicate that there are no more dependencies to check for this course.

By setting it to `null`, we achieve two things:
1. **Save memory**: We clear the dependency list for courses we've fully processed, reducing memory usage.
2. **Avoid reprocessing**: It helps avoid redundant checks. If the course has already been processed (i.e., its value is `null`), we know it's already been handled, so we don’t need to traverse it again.

### Why DFS Over BFS?

- **DFS** is better suited for problems like cycle detection because it allows you to explore one dependency chain fully before backtracking. When a cycle is encountered, it’s easy to detect because the course will still be in the `visited` set.
- **BFS** is better for level-by-level exploration, such as finding the shortest path in a graph. It does not support the same kind of backtracking needed for cycle detection in a straightforward way.

### Time Complexity: O(V + E)

Let’s break down the complexity.

1. **Building the Graph**:
   - We iterate over all the prerequisites, which takes **O(P)** time, where `P` is the number of prerequisite pairs.
   
2. **DFS Traversal**:
   - For each course, we explore all its dependencies. In the worst case, we visit each course (vertex `V`) and explore every dependency (edge `E`), leading to **O(V + E)** time complexity.

### Space Complexity: O(V + E)

- **Graph storage**: We store the graph in a HashMap. Each course is a key (taking `V` space), and for each course, we store a list of its dependencies (which takes `E` space for all dependencies).
- **Visited set**: We store visited courses in a HashSet, which takes **O(V)** space.
- **Recursive stack**: In the worst case, the depth of the recursion could be equal to the number of courses (vertices `V`), leading to **O(V)** space for the recursion stack.

Thus, the overall space complexity is **O(V + E)**.

---

### Conclusion

- The algorithm uses **DFS** to detect cycles in the dependency graph.
- It builds the graph using a HashMap where each course points to its dependent courses.
- It checks for cycles using a `visited` set. If a cycle is detected, the function returns `false`.
- The complexity is **O(V + E)** because we visit each course and its dependencies once.

### Another Dry Run of the Code

Let’s run through a **dry run** with an example:

Suppose we have `4` courses and the following prerequisites:
```
numCourses = 4
prerequisites = [[1, 0], [2, 1], [3, 2]] 
```

This means:
- Course 1 depends on course 0.
- Course 2 depends on course 1.
- Course 3 depends on course 2.

### Graph Representation:
The graph looks like this:

```
0 → 1 → 2 → 3
```

#### Step-by-Step Execution:

1. **Building the Graph**:
   - For each prerequisite pair `[a, b]`, we add `a` as a dependency of `b`:
     - For `[1, 0]`, we add course 1 to the dependency list of course 0 → `courseGraph = {0: [1]}`
     - For `[2, 1]`, we add course 2 to the dependency list of course 1 → `courseGraph = {0: [1], 1: [2]}`
     - For `[3, 2]`, we add course 3 to the dependency list of course 2 → `courseGraph = {0: [1], 1: [2], 2: [3]}`
     
2. **DFS on Each Course**:
   We now try to see if we can finish all courses. We check each course using DFS.

   - **For course 0**:
     - `visited = {0}` → DFS calls course 1.
     - **For course 1**:
       - `visited = {0, 1}` → DFS calls course 2.
       - **For course 2**:
         - `visited = {0, 1, 2}` → DFS calls course 3.
         - **For course 3**:
           - `visited = {0, 1, 2, 3}`. Course 3 has no dependencies. It returns `true`.
         - Backtrack to course 2, remove course 3 from `visited`, return `true`.
       - Backtrack to course 1, remove course 2 from `visited`, return `true`.
     - Backtrack to course 0, remove course 1 from `visited`, return `true`.

   - **For course 1**: Already processed (set to `null`), returns `true`.
   - **For course 2**: Already processed (set to `null`), returns `true`.
   - **For course 3**: Already processed (set to `null`), returns `true`.

No cycles are found, and we can finish all the courses.

### 6. Time Complexity Explanation

- **Building the graph**: This takes **O(P)** time, where `P` is the number of prerequisite pairs.
- **DFS**: Each node (course) is visited once, and for each node, we explore all its neighbors (dependencies). So the time complexity is **O(V + E)**, where `V` is the number of vertices (courses), and `E` is the number of edges (prerequisites).

### 7. Space Complexity Explanation

- **Graph storage**: We use a HashMap to store the graph, which takes **O(V + E)** space.
- **Visited set**: We use a HashSet for tracking visited nodes during DFS, which takes **O(V)** space.
- **Recursive stack**: In the worst case, the depth of the recursion could be equal to the number of courses, leading to **O(V)** space for the recursion stack.

Thus, the overall space complexity is **O(V + E)**.

-------------------------------------------------------------------------------------------------------------------------------------------------------
### Another approach -

### Problem: Course Schedule (with Kahn's Algorithm and Topological Sort)

You are given `numCourses` and a list of `prerequisites` where each pair `[a, b]` means you must complete course `b` before taking course `a`. Your task is to determine if it is possible to finish all courses without violating the prerequisites.

This is a classic **graph cycle detection** problem, and it can be solved using **Topological Sort** through **Kahn's Algorithm**.

### Key Graph Concepts

1. **Nodes**: Each course is a node.
2. **Edges**: Each prerequisite (like `[a, b]`) is a directed edge from `b` to `a`, indicating that `b` must be completed before `a`.

#### Directed Acyclic Graph (DAG)
- If there is a **cycle** in the graph, it is impossible to complete the courses because some courses would be dependent on each other in a circular way.
- We solve this by performing **topological sort**, where we try to order the nodes (courses) such that all edges go from left to right, respecting dependencies.

### Topological Sort Using **Kahn's Algorithm**

Kahn's Algorithm is a **BFS**-based approach to topological sorting. We keep track of the **in-degree** (number of incoming edges) of each node. The main idea is:
- Start with nodes that have **no prerequisites** (in-degree 0).
- Remove these nodes from the graph and update the in-degree of their neighbors.
- Repeat this process until no nodes with in-degree 0 remain.

If we have processed all nodes, there is no cycle. If some nodes remain unprocessed, it indicates the presence of a cycle.

---

### Kahn's Algorithm Breakdown (with Text Diagram)

Consider an example with 4 courses and the following prerequisites:

```
numCourses = 4
prerequisites = [[1, 0], [2, 1], [3, 2]]
```

#### Graph Representation

- Course 1 depends on course 0: `0 → 1`
- Course 2 depends on course 1: `1 → 2`
- Course 3 depends on course 2: `2 → 3`

The graph looks like this:

```
0 → 1 → 2 → 3
```

#### Steps:

1. **Build the Graph**:
    - First, we create an **adjacency list** to represent the graph.
    - Then, we create an **in-degree array** to count the incoming edges for each course.
   
   After processing the prerequisites, we have:
   - **Adjacency List**: 
     ```
     0 → [1]
     1 → [2]
     2 → [3]
     ```
   - **In-degree array**: 
     ```
     [0, 1, 1, 1]
     ```

   This means course 0 has no prerequisites, course 1 has 1 prerequisite (course 0), and so on.

2. **Initialize the Queue**:
   - We start with courses that have an in-degree of 0, which means they have no prerequisites.
   - In this case, only course 0 has in-degree 0, so we add it to the queue:
     ```
     Queue = [0]
     ```

3. **Process the Queue**:
   - Dequeue course 0 (the first element of the queue) and process it.
   - After processing, we remove the edge `0 → 1`, so we decrement the in-degree of course 1 by 1:
     ```
     In-degree array: [0, 0, 1, 1]
     Queue = [1]
     ```

   - Now process course 1. Remove the edge `1 → 2` and decrement the in-degree of course 2:
     ```
     In-degree array: [0, 0, 0, 1]
     Queue = [2]
     ```

   - Next, process course 2. Remove the edge `2 → 3` and decrement the in-degree of course 3:
     ```
     In-degree array: [0, 0, 0, 0]
     Queue = [3]
     ```

   - Finally, process course 3. No more edges to remove, and the queue becomes empty:
     ```
     Queue = []
     ```

4. **Check Completion**:
   - If all courses have been processed (i.e., visited all nodes), the in-degree array will be all zeros, and we have successfully completed the topological sort.
   - In this case, all courses were processed, so it is possible to finish all courses. The result is `true`.

---

### Code for Kahn's Algorithm

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        // Step 1: Initialize the graph
        int[] indegree = new int[numCourses];
        List<List<Integer>> adj = new ArrayList<>(numCourses);

        // Create the adjacency list and in-degree array
        for (int i = 0; i < numCourses; i++) {
            adj.add(new ArrayList<>());
        }

        // Step 2: Build the graph by adding directed edges and updating in-degree
        for (int[] prerequisite : prerequisites) {
            adj.get(prerequisite[1]).add(prerequisite[0]);
            indegree[prerequisite[0]]++;
        }

        // Step 3: Initialize the queue with courses that have no prerequisites (in-degree 0)
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) {
            if (indegree[i] == 0) {
                queue.offer(i);
            }
        }

        // Step 4: BFS to process the graph
        int nodesVisited = 0;
        while (!queue.isEmpty()) {
            int node = queue.poll();
            nodesVisited++;

            for (int neighbor : adj.get(node)) {
                // Remove the edge "node -> neighbor"
                indegree[neighbor]--;
                if (indegree[neighbor] == 0) {
                    queue.offer(neighbor);
                }
            }
        }

        // Step 5: Return true if all courses have been visited, else false
        return nodesVisited == numCourses;
    }
}
```

### Complexity Analysis

- **Time Complexity**: `O(m + n)`
  - `m` is the number of edges (prerequisites), and `n` is the number of nodes (courses).
  - Building the adjacency list and in-degree array takes `O(m)` time.
  - Processing each node and edge also takes `O(m + n)` time.
  
- **Space Complexity**: `O(m + n)`
  - We need space for the adjacency list (`O(m)`), the in-degree array (`O(n)`), and the queue (`O(n)`).

### Explanation of Kahn's Algorithm

1. **Topological Sort**: Kahn's Algorithm uses the idea of topological sorting where we visit nodes in an order such that each node comes after its dependencies (its prerequisites in this case).
   
2. **In-degree Array**: The in-degree array tracks how many prerequisites each course has. We start with nodes that have no prerequisites and gradually remove dependencies as we process each course.

3. **Cycle Detection**: If there is a cycle, some courses will always have prerequisites left (in-degree > 0), and we won't be able to process them. Thus, if the number of visited courses is less than `numCourses`, we know a cycle exists.

---

### Why Not DFS (Depth-First Search)?

- DFS is another way to perform topological sort, but it works better for detecting cycles in *smaller, simpler graphs*.
- **BFS (Kahn's Algorithm)** is preferred for course scheduling because it processes nodes level by level, making it easier to handle dependencies without stack overflow in large graphs.

-------------------------------------------------------------------------------------------------------------------------------------------------------

## Question 2 -  Problem: Course Schedule II (Leetcode Link: [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/))

You are given `numCourses` courses labeled from `0` to `numCourses - 1`. You are also given an array of `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you must take course `bi` before you can take course `ai`.

Return the ordering of courses you should take to finish all courses. If it is impossible to finish all courses, return an empty array.

This is essentially a **graph problem** where the courses are vertices, and the prerequisites represent directed edges. The goal is to return a **topological sort** of the courses if no cycle exists, or an empty array if there is a cycle (indicating that it is not possible to complete all courses).

---

### Key Concepts:
1. **Graph Representation**:
   Each course is a node, and if a course `bi` is a prerequisite for another course `ai`, there is a directed edge from node `bi` to node `ai`.

2. **Cycle Detection**:
   If a cycle exists, it is not possible to complete all courses because some courses depend on each other in a circular fashion.

3. **Topological Sorting**:
   This problem can be solved using **DFS** (Depth-First Search) to generate a **topological sort**. Topological sorting is possible only if the graph is **acyclic** (i.e., it does not contain any cycles).

---

### Approach (DFS + Cycle Detection)

1. **Color Marking**:
   - `WHITE` (1): A vertex that has not been visited yet.
   - `GRAY` (2): A vertex that is currently being visited (part of the recursion stack).
   - `BLACK` (3): A vertex that has been fully visited.

  #### Why do we need to keep both `GRAY = 2` and `BLACK = 3`?

  In **DFS-based cycle detection**, having both `GRAY` and `BLACK` helps distinguish between nodes that are:

  - **GRAY (2)**: Nodes that are currently being visited (part of the current recursion stack). These nodes have been **discovered** but not yet fully processed.
  - **BLACK (3)**: Nodes that have been fully processed, meaning all their neighbors have been visited and processed as well. These nodes are **out of the recursion stack**.

  This distinction helps us detect **cycles**:
  - If during the DFS, we encounter a node that is marked as `GRAY`, it means we are visiting a node that is already in the current recursion path (i.e., we found a back edge), which indicates the presence of a cycle.
  - Once a node is fully processed, it is marked as `BLACK`. Visiting a `BLACK` node does not indicate a cycle because it has already been processed outside of the current recursion stack.

  In short:
  - **GRAY** is used to detect cycles.
  - **BLACK** is used to ensure that once a node is fully processed, we don’t revisit it unnecessarily.

2. **Graph Construction**:
   The graph is represented using an adjacency list (`adjList`). Each node (course) points to the list of courses that depend on it.

3. **DFS for Cycle Detection**:
   - We recursively visit each node.
   - If during a DFS traversal, we encounter a node marked `GRAY` (currently being visited), this indicates a cycle, and we can terminate early.

4. **Topological Sort**:
   If no cycle is found, we add nodes to the result in **reverse order** of their DFS finishing time (i.e., when the recursion unwinds).

### Why did we not prefer using BFS (Kahn's Algorithm) here?

**DFS-based Topological Sorting** is often preferred when:
1. **Cycle Detection is Important**: The DFS-based approach allows for easy detection of cycles by marking nodes as `GRAY` when visiting and detecting back edges. In contrast, Kahn’s Algorithm (BFS) does not inherently detect cycles—it just fails to provide a topological sort when a cycle is present.
   
2. **Post-order Processing**: DFS naturally allows post-order processing, which is key for topological sorting. This means we visit all dependencies (subtrees) of a node before marking the node itself.

3. **Edge Cases**: DFS handles graphs where not all nodes have dependencies, and the graph isn't fully connected, more gracefully.

**Kahn’s Algorithm (BFS)** is more efficient in some cases but:
- It requires maintaining an **in-degree array** to keep track of the number of incoming edges.
- It doesn't directly detect cycles but simply fails to produce a complete topological order when there's a cycle.

---

### Solution Breakdown:

```java
class Solution {
  // Define colors for marking the status of each node
  static int WHITE = 1;  // Unvisited
  static int GRAY = 2;   // Currently visiting (in stack)
  static int BLACK = 3;  // Fully visited (processed)

  boolean isPossible;  // To check if a valid topological sort is possible
  Map<Integer, Integer> color;  // Stores color of each node
  Map<Integer, List<Integer>> adjList;  // Graph adjacency list representation
  List<Integer> topologicalOrder;  // To store the topological order of courses

  // Initialization method to set up variables
  private void init(int numCourses) {
    this.isPossible = true;  // Assume we can complete all courses
    this.color = new HashMap<>();  // To keep track of visit status
    this.adjList = new HashMap<>();  // To store graph as adjacency list
    this.topologicalOrder = new ArrayList<>();  // To store the course order

    // Initially, mark all courses as WHITE (unvisited)
    for (int i = 0; i < numCourses; i++) {
      this.color.put(i, WHITE);
    }
  }

  // Depth-First Search (DFS) for cycle detection and topological sort
  private void dfs(int node) {
    // If a cycle is found, terminate early
    if (!this.isPossible) {
      return;
    }

    // Mark the node as currently being visited (GRAY)
    this.color.put(node, GRAY);

    // Visit all neighboring nodes (courses that depend on the current course)
    for (Integer neighbor : this.adjList.getOrDefault(node, new ArrayList<>())) {
      if (this.color.get(neighbor) == WHITE) {
        // If the neighbor is unvisited, perform DFS on it
        this.dfs(neighbor);
      } else if (this.color.get(neighbor) == GRAY) {
        // If the neighbor is currently being visited (GRAY), we found a cycle
        this.isPossible = false;
      }
    }

    // Mark the node as fully visited (BLACK)
    this.color.put(node, BLACK);
    // Add the node to the topological order
    this.topologicalOrder.add(node);
  }

  // Main function to find the course order
  public int[] findOrder(int numCourses, int[][] prerequisites) {
    // Initialize graph and color mapping
    this.init(numCourses);

    // Build the graph from prerequisites
    for (int i = 0; i < prerequisites.length; i++) {
      int dest = prerequisites[i][0];  // Course that depends on another
      int src = prerequisites[i][1];   // Course that must be taken first
      List<Integer> lst = adjList.getOrDefault(src, new ArrayList<>());
      lst.add(dest);
      adjList.put(src, lst);
    }

    // Perform DFS on all unvisited nodes
    for (int i = 0; i < numCourses; i++) {
      if (this.color.get(i) == WHITE) {
        this.dfs(i);
      }
    }

    // If a valid topological sort is possible, return the course order
    int[] order;
    if (this.isPossible) {
      order = new int[numCourses];
      for (int i = 0; i < numCourses; i++) {
        order[i] = this.topologicalOrder.get(numCourses - i - 1);  // Reverse the order
      }
    } else {
      order = new int[0];  // If a cycle is found, return an empty array
    }

    return order;
  }
}
```

---

### Detailed Breakdown:

1. **Initialization (`init()` method)**:
   - We initialize:
     - `isPossible` to `true`, assuming we can complete all courses.
     - `color` map to track the status (WHITE, GRAY, or BLACK) of each course.
     - `adjList` to represent the graph as an adjacency list.
     - `topologicalOrder` to store the final topological ordering.

   - Initially, all courses are marked as **unvisited (WHITE)**.

2. **Graph Construction**:
   - We loop through the `prerequisites` to build the graph.
   - For each prerequisite `[a, b]`, we add an edge from `b` to `a` in the adjacency list (`b -> a`).

3. **DFS Traversal (`dfs()` method)**:
   - **Cycle Detection**: 
     - If a node is marked **GRAY** during the DFS traversal (i.e., we encounter a node currently in the stack), it indicates a cycle.
     - If a cycle is found, `isPossible` is set to `false`, and we terminate the search.
   
   - **Topological Sorting**:
     - Once the DFS finishes processing a node, it is added to the `topologicalOrder` list in post-order (meaning, all its dependencies have been processed).

4. **Final Topological Sort**:
   - If no cycle is found, the topological order is constructed in reverse, as nodes are added in post-order traversal.
   - If a cycle is detected, we return an empty array, as it is impossible to complete all courses.

---

### Dry Run with Example (With Cycle):

Let's take an example of `numCourses = 4` and `prerequisites = [[1, 0], [2, 1], [3, 2], [1, 3]]`.

This forms a **cyclic graph** because course `1` depends on course `3`, but course `3` indirectly depends on course `1` through other courses.

```
   0 → 1
   ↑   ↓
   3 ← 2
```

### Initial Setup:

- `WHITE = 1`: Unvisited nodes.
- `GRAY = 2`: Nodes that are being visited.
- `BLACK = 3`: Nodes that are fully processed.

```
Initial:
color: { 0: WHITE, 1: WHITE, 2: WHITE, 3: WHITE }
adjList: { 0: [1], 1: [2], 2: [3], 3: [1] }
topologicalOrder: []
```

### Step-by-Step DFS:

1. **Visit Node 0**:
   - Color it `GRAY`.
   - It has one neighbor, `1`, so we move to `1`.

```
color: { 0: GRAY, 1: WHITE, 2: WHITE, 3: WHITE }
```

2. **Visit Node 1**:
   - Color it `GRAY`.
   - It has one neighbor, `2`, so we move to `2`.

```
color: { 0: GRAY, 1: GRAY, 2: WHITE, 3: WHITE }
```

3. **Visit Node 2**:
   - Color it `GRAY`.
   - It has one neighbor, `3`, so we move to `3`.

```
color: { 0: GRAY, 1: GRAY, 2: GRAY, 3: WHITE }
```

4. **Visit Node 3**:
   - Color it `GRAY`.
   - It has one neighbor, `1`. But `1` is **already marked as GRAY** (meaning we have found a back edge). This indicates a **cycle**.
   - We stop the DFS traversal and mark that it's **not possible** to complete the courses.

```
color: { 0: GRAY, 1: GRAY, 2: GRAY, 3: GRAY }
Cycle Detected! (1 → 3 → 2 → 1)
```

### Code Breakdown for Cycle Detection:

- **`init` Function**: 
  Initializes all nodes to `WHITE` and builds the adjacency list from the prerequisites.
  
- **`dfs` Function**:
  1. Colors the current node `GRAY`.
  2. For each neighbor, checks if it's `WHITE` (not visited) and continues the DFS. If it’s `GRAY` (currently visiting), a cycle is detected.
  3. Once the node and all its neighbors are processed, it is colored `BLACK` (fully visited) and added to the `topologicalOrder`.

- **Cycle Detection**: If at any point we encounter a `GRAY` node during the DFS, it means a cycle exists, and the function returns `false`.

### Dry Run Explanation for Key Code Portions:

1. **Graph Construction**:
   ```java
   for (int i = 0; i < prerequisites.length; i++) {
      int dest = prerequisites[i][0];
      int src = prerequisites[i][1];
      List<Integer> lst = adjList.getOrDefault(src, new ArrayList<>());
      lst.add(dest);
      adjList.put(src, lst);
    }
   ```
   - Constructs the adjacency list to represent the course dependencies.
   - For example, `prerequisites = [[1, 0], [2, 1], [3, 2], [1, 3]]` builds the following adjacency list:
     ```
     adjList: { 0: [1], 1: [2], 2: [3], 3: [1] }
     ```

2. **DFS Traversal**:
   - For each unvisited node (marked `WHITE`), we perform DFS. If we find a cycle (i.e., revisit a `GRAY` node), we terminate the search.
   
   Example code with cycle detection:
   ```java
   for (Integer neighbor : this.adjList.getOrDefault(node, new ArrayList<>())) {
      if (this.color.get(neighbor) == WHITE) {
         this.dfs(neighbor);
      } else if (this.color.get(neighbor) == GRAY) {
         // An edge to a GRAY vertex represents a cycle
         this.isPossible = false;
      }
   }
   ```

3. **Topological Sorting**:
   - Once a node and its dependencies are processed, it is marked `BLACK` and added to the `topologicalOrder`.

### Why DFS over BFS in this Case?

- **DFS** is more natural for **topological sorting** because it explores each path to its end before backtracking. This ensures that a node is added to the topological order only after all its dependencies have been processed.
- **Cycle Detection** is easy to implement using DFS by marking nodes as `GRAY` when they are in the current path and detecting back edges.
- **BFS (Kahn's Algorithm)** requires maintaining an in-degree array, and while it is more efficient in terms of iterations, it doesn't inherently support easy cycle detection. If we needed only the topological order (without concern for cycles), BFS could be used. However, DFS is better suited for detecting cycles while constructing the topological order.

---

### Time and Space Complexity:

- **Time Complexity**: O(V + E), where:
  - **V** is the number of courses (vertices).
  - **E** is the number of prerequisite pairs (edges).
  - We process each vertex and edge exactly once in the DFS traversal, so the overall complexity is linear with respect to the size of the graph.

- **Space Complexity**: O(V + E), where:
  - **V** is the space used for storing the color map, recursion stack (in DFS), and topological order.
  - **E** is the space used for the adjacency list representation of the graph.
---

### Example Dry Run (Without Cycle):

Let's run an example with `numCourses = 4` and `prerequisites = [[1, 0], [2, 1], [3, 2]]`.

- **Graph Representation**:
  ```
  0 → 1 → 2 → 3
  ```

- **Initialization**:
  All courses are initially marked **WHITE**.

- **DFS Process**:
  - Start with course `0`. It has a prerequisite to course `1`, so DFS proceeds to course `1`.
  - Course `1` has a prerequisite to course `2`, so DFS proceeds to course `2`.
  - Course `2` has a prerequisite to course `3`, so DFS proceeds to course `3`.
  - Course `3` has no prerequisites, so it's marked **BLACK** and added to the topological order.

- **Topological Order**:
  - After DFS finishes for course `3`, we backtrack and mark courses `2`, `1`, and `0` as **BLACK** and add them to the topological order.

- **Final Result**:
  The final topological order is `[0, 1, 2, 3]`, meaning you can complete the courses in this order.

---

### Time Complexity: O(V + E)
- **V**: The number of courses (vertices).
- **E**: The number of prerequisite pairs (edges).
- The DFS traversal visits each vertex once and explores each edge once, leading to a time complexity of **O(V + E)**.

### Space Complexity: O(V + E)
- The adjacency list stores each course's prerequisites (edges), which takes **O(E)** space.
- The `color` map and the `topologicalOrder` list both take **O(V)** space for the courses.
- Therefore, the space complexity is **O(V + E)**.

---

Certainly! Let's break down the provided solution step by step, making it easy to understand even if you're completely new to graph algorithms. We'll cover the problem statement, the approach used in the solution, a detailed example, reasoning behind choosing this approach, its time and space complexities, and some potential interview questions to test your understanding.

------------------------------------------------------------------------------------------------------------------------------------------------------

## Question 3 - Problem (Leetcode Link: [Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow))

**Pacific Atlantic Water Flow**

You are given an `m x n` grid of non-negative integers representing the height of each cell in a continent. The continent is surrounded by the Pacific Ocean on the **left** and **top** edges and the Atlantic Ocean on the **right** and **bottom** edges.

Water can flow from a cell to its **four** adjacent cells (up, down, left, or right) **if and only if** the adjacent cell's height is **equal to or lower** than the current cell's height.

```csharp
heights = [
  [1, 2, 2, 3, 4],
  [3, 2, 3, 4, 5],
  [2, 4, 5, 3, 1],
  [6, 7, 1, 4, 5],
  [5, 1, 1, 2, 4]
]
```

```csharp
P P P P P
P - - - A
P - - - A
P - - - A
P A A A A
```
**Task:** Find all cells where water can flow to **both** the Pacific and Atlantic Oceans.

**Return:** A list of coordinates `[i, j]` representing the cells where water can flow to both oceans.

---

## **Understanding the Solution**

The provided Java solution uses **Depth-First Search (DFS)** to determine which cells can reach both oceans. Here's a step-by-step explanation tailored for beginners.

### **1. Initial Checks and Setup**

```java
if (heights.length == 0 || heights[0].length == 0){
    return new ArrayList<>();
} 

int row = heights.length;
int col = heights[0].length;

boolean[][] pacificReachable = new boolean[row][col];
boolean[][] atlanticReachable = new boolean[row][col];
```

- **Check for Empty Grid:** If the `heights` grid is empty, return an empty list as there's nothing to process.
- **Grid Dimensions:** Determine the number of rows (`row`) and columns (`col`) in the grid.
- **Reachability Matrices:** Create two boolean matrices:
  - `pacificReachable`: Marks cells from which water can reach the Pacific Ocean.
  - `atlanticReachable`: Marks cells from which water can reach the Atlantic Ocean.

### **2. Performing DFS from Ocean Borders**

```java
for(int i = 0; i < row; i++){
    dfs(i, 0, pacificReachable, heights);       // Left edge (Pacific)
    dfs(i, col - 1, atlanticReachable, heights); // Right edge (Atlantic)
}

for(int i = 0; i < col; i++){
    dfs(0, i, pacificReachable, heights);       // Top edge (Pacific)
    dfs(row - 1, i, atlanticReachable, heights); // Bottom edge (Atlantic)
}
```

- **Pacific Ocean Borders:**
  - **Left Edge (First Column):** Iterate through each row and perform DFS starting from the first column.
  - **Top Edge (First Row):** Iterate through each column and perform DFS starting from the first row.
  
- **Atlantic Ocean Borders:**
  - **Right Edge (Last Column):** Iterate through each row and perform DFS starting from the last column.
  - **Bottom Edge (Last Row):** Iterate through each column and perform DFS starting from the last row.

**Why Start from the Oceans?**

Instead of checking for each cell whether it can reach both oceans (which can be inefficient), we reverse the problem:

- **For Pacific:** Find all cells that can be reached **from** the Pacific Ocean.
- **For Atlantic:** Find all cells that can be reached **from** the Atlantic Ocean.

The intersection of these two sets gives us the cells that can reach both oceans.

### **3. Collecting the Results**

```java
List<List<Integer>> result = new ArrayList<>();

for(int i = 0; i < row; i++){
    for(int j = 0; j < col; j++){
        if(pacificReachable[i][j] && atlanticReachable[i][j]){
            result.add(List.of(i, j));
        }
    }
}

return result;
```

- **Iterate Through All Cells:** For each cell in the grid, check if it's marked as reachable in both `pacificReachable` and `atlanticReachable`.
- **Add to Result:** If a cell can reach both oceans, add its coordinates to the `result` list.

### **4. Depth-First Search (DFS) Implementation**

```java
public void dfs(int row, int col, boolean[][] reachable, int[][] heights){
    
    int[][] directions = new int[][]{{0,1}, {1,0}, {-1,0}, {0,-1}};
    
    reachable[row][col] = true;
    
    for(int[] dir: directions){
        int newRow = row + dir[0];
        int newCol = col + dir[1];
        
        if(newRow < 0 || newRow >= heights.length || newCol < 0 || newCol >= heights[0].length){
            continue; // Out of bounds
        }
        
        if(reachable[newRow][newCol]){
            continue; // Already visited
        }
        
        if(heights[newRow][newCol] < heights[row][col]){
            continue; // Water cannot flow from higher to lower
        }
        
        dfs(newRow, newCol, reachable, heights);
    }
}
```

- **Directions Array:** Represents the four possible movements: right, down, up, and left.
- **Mark Current Cell as Reachable:** Set `reachable[row][col]` to `true` to indicate that this cell can reach the current ocean.
- **Explore Neighbors:**
  - **Boundary Check:** Ensure the new cell is within the grid.
  - **Visited Check:** If the new cell has already been marked as reachable, skip it to avoid redundant work.
  - **Height Check:** Only move to neighboring cells that are **equal or higher** in height, ensuring water can flow from higher to lower or same height.
  - **Recursive DFS Call:** If all checks pass, perform DFS on the neighboring cell.

---

## **Step-by-Step Dry Run with Example**

Let's walk through an example to see how the solution works.

### **Example Grid**

Consider the following `heights` grid:

```
[
  [1, 2, 2, 3, 5],
  [3, 2, 3, 4, 4],
  [2, 4, 5, 3, 1],
  [6, 7, 1, 4, 5],
  [5, 1, 1, 2, 4]
]
```

- **Dimensions:** `row = 5`, `col = 5`

### **Step 1: Initialize Reachability Matrices**

- `pacificReachable` and `atlanticReachable` are both 5x5 matrices initialized to `false`.

### **Step 2: Perform DFS from Pacific Borders**

- **Left Edge (First Column):** Cells `(0,0)`, `(1,0)`, `(2,0)`, `(3,0)`, `(4,0)`
  - Start DFS from each of these cells, marking reachable cells in `pacificReachable`.
  
- **Top Edge (First Row):** Cells `(0,0)`, `(0,1)`, `(0,2)`, `(0,3)`, `(0,4)`
  - Start DFS from each of these cells, marking reachable cells in `pacificReachable`.

### **Step 3: Perform DFS from Atlantic Borders**

- **Right Edge (Last Column):** Cells `(0,4)`, `(1,4)`, `(2,4)`, `(3,4)`, `(4,4)`
  - Start DFS from each of these cells, marking reachable cells in `atlanticReachable`.
  
- **Bottom Edge (Last Row):** Cells `(4,0)`, `(4,1)`, `(4,2)`, `(4,3)`, `(4,4)`
  - Start DFS from each of these cells, marking reachable cells in `atlanticReachable`.

### **Step 4: Collect Cells Reachable by Both Oceans**

After performing DFS from all border cells, some cells will be marked as reachable in both `pacificReachable` and `atlanticReachable`. For this example, the cells that satisfy this condition are:

```
[
  [0,4],
  [1,3],
  [1,4],
  [2,2],
  [3,0],
  [3,1],
  [4,0]
]
```

These are the cells from which water can flow to both the Pacific and Atlantic Oceans.

---

## **Reasoning Behind Choosing DFS Approach**

### **Why DFS?**

- **Exploration Efficiency:** DFS is effective for exploring all possible paths from a starting point. In this problem, we're interested in all cells that can be reached from the ocean borders.
  
- **Avoiding Redundant Work:** By marking cells as reachable during DFS, we avoid revisiting the same cells multiple times, making the process efficient.

### **Why Start from Oceans Instead of Each Cell?**

- **Optimized Traversal:** Starting DFS from the oceans and marking reachable cells reduces the number of DFS calls. If we were to start DFS from each cell to check if it can reach both oceans, it would result in significantly more computations, especially for large grids.

- **Simplified Intersection:** After performing DFS from both oceans, finding the intersection of reachable cells is straightforward.

---

## **Time and Space Complexity**

### **Time Complexity: O(m * n)**

- **m:** Number of rows in the grid.
- **n:** Number of columns in the grid.
  
- **Explanation:**
  - Each cell is visited at most twice: once from the Pacific side and once from the Atlantic side.
  - The DFS explores each cell's neighbors, but since each cell is processed a limited number of times, the overall time complexity is linear relative to the number of cells.

### **Space Complexity: O(m * n)**

- **Explanation:**
  - Two boolean matrices (`pacificReachable` and `atlanticReachable`) each require O(m * n) space.
  - The recursion stack for DFS can go up to O(m * n) in the worst case.

---

## **Interview Questions and Answers**

To ensure you've grasped the concepts, here are some potential interview questions along with their answers.

### **1. What is the main idea behind the solution?**

**Answer:** The solution uses Depth-First Search (DFS) starting from the ocean borders to mark cells that can reach each ocean. By performing DFS from both the Pacific and Atlantic borders, we identify cells that are reachable by both oceans by finding the intersection of these two sets of cells.

### **2. Why do we perform DFS from the ocean borders instead of from each cell?**

**Answer:** Performing DFS from the ocean borders is more efficient because it reduces the number of DFS calls. Starting from each cell would lead to redundant computations and higher time complexity, especially for large grids. By reversing the problem, we efficiently identify all cells that can reach each ocean.

### **3. How does the DFS ensure that water can flow from a cell to the ocean?**

**Answer:** During DFS, we only move to neighboring cells that are **equal or higher** in height. This ensures that water can flow from the higher or same height cell to the lower or same height cell, effectively simulating water flow towards the ocean.

### **4. What are the time and space complexities of this solution?**

**Answer:** Both time and space complexities are O(m * n), where m is the number of rows and n is the number of columns in the grid. Each cell is visited at most twice (once for each ocean), and we use additional space for the reachability matrices and the recursion stack.

### **5. Can this solution be optimized further?**

**Answer:** The current solution is already optimized for time and space for this problem. However, iterative approaches using Breadth-First Search (BFS) could be used instead of DFS to potentially improve performance in practice, especially to avoid deep recursion which can lead to stack overflow in large grids.


   - What are the advantages of using BFS over DFS in this problem?
      - Answer: BFS avoids the risk of stack overflow associated with deep recursion in DFS, especially in large grids. It provides a controlled iterative approach using queues, which can be more predictable in terms of memory usage. Additionally, BFS can be more efficient in scenarios where the solution requires exploring cells layer by layer.

   - Could this BFS approach be further optimized?
      - Answer: The BFS approach is already efficient with O(m * n) time and space complexities. However, minor optimizations could include:

      - Using Bitmasking: Instead of two separate boolean matrices, use a single matrix with bitmask flags to indicate reachability.
      - Early Termination: If certain conditions allow, terminate BFS early when no further cells can be marked as reachable.
      - Iterative BFS Optimization: Reuse data structures or minimize object creation within loops for better performance.

### **6. What would happen if the grid is empty? How does the solution handle it?**

**Answer:** If the grid is empty (i.e., `heights.length == 0` or `heights[0].length == 0`), the solution immediately returns an empty list, as there are no cells to process.

### **7. How does the solution determine which cells can reach both oceans?**

**Answer:** After performing DFS from both the Pacific and Atlantic borders, the solution iterates through all cells and selects those that are marked as reachable in both `pacificReachable` and `atlanticReachable` matrices. These cells can flow water to both oceans.

---

## **Conclusion**

This solution efficiently solves the Pacific Atlantic Water Flow problem by leveraging DFS from ocean borders and identifying cells reachable by both oceans. Understanding the reasoning behind the approach, along with its implementation details and complexities, is crucial for mastering similar graph-related problems.

### Conclusion:


- This solution uses **DFS** to detect cycles and construct a topological order for the courses.
- If a cycle is detected, we return an empty array, as it's impossible to complete all courses.
- If no cycle is found, we return the valid course order in topological sort.

-------------------------------------------------------------------------------------------------------------------------------------------------------


## Question 4 - Problem (Leetcode Link: [Number of Islands](https://leetcode.com/problems/number-of-islands/))

## **Problem Statement**

### **Number of Islands**

You are given a 2D grid map consisting of `'1'`s (land) and `'0'`s (water). An **island** is surrounded by water and is formed by connecting adjacent lands **horizontally or vertically**. You may assume all four edges of the grid are surrounded by water.

**Task:** Write an algorithm to count the number of islands in the given grid.

**Example:**

```
Input:
11110
11010
11000
00000

Output: 1
```

```
Input:
11000
11000
00100
00011

Output: 3
```

---

## **Understanding the Solution**

The provided Java solution utilizes **Depth-First Search (DFS)** to traverse the grid and count the number of distinct islands. Here's a step-by-step explanation tailored for beginners.

### **1. Overview of the Approach**

1. **Traverse the Grid:** Iterate through each cell in the grid.
2. **Identify Land Cells:** When a land cell (`'1'`) is found, it's part of an island.
3. **Mark Visited Cells:** Use DFS to mark all connected land cells of the current island to avoid recounting them.
4. **Count Islands:** Increment the island count each time a new unvisited land cell is found.

### **2. Why Use DFS?**

- **Exploration:** DFS is effective for exploring all connected cells in a component (island) before moving to the next.
- **Simplicity:** The recursive nature of DFS makes it straightforward to implement for this problem.
- **Efficiency:** Each cell is visited only once, ensuring optimal performance.

---

## **Detailed Code Explanation with Comments**

Let's annotate the provided code with detailed comments to understand each part.

```java
class Solution {
    // Main method to count the number of islands
    public int numIslands(char[][] grid) {
        // Check if the grid is empty
        if (grid == null || grid.length == 0) {
            return 0;
        }
        
        int row = grid.length;        // Number of rows in the grid
        int col = grid[0].length;     // Number of columns in the grid
        
        int island = 0; // Counter for the number of islands
        
        // Iterate through each cell in the grid
        for(int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                // If the current cell is land ('1'), it's part of an island
                if(grid[i][j] == '1') {
                    island++; // Increment island count
                    dfs(i, j, grid); // Perform DFS to mark all connected land cells
                }
            }
        }
        
        return island; // Return the total number of islands found
    }
    
    // DFS method to traverse and mark connected land cells
    public void dfs(int row, int col, char[][] grid) {
        int newRow = grid.length;      // Total rows
        int newCol = grid[0].length;   // Total columns
        
        // Directions to move: right, down, left, up
        int [][] directions = new int[][]{{0,1}, {1,0},{0,-1},{-1,0}};
        
        // Base cases to stop recursion:
        // 1. If the current cell is out of bounds
        // 2. If the current cell is water ('0')
        if(row < 0 || col < 0 || row >= newRow || col >= newCol || grid[row][col] == '0') {
            return;
        }
        
        // Mark the current cell as visited by setting it to '0' (water)
        grid[row][col] = '0';
        
        // Explore all four directions from the current cell
        for(int[] dir: directions){
            dfs(row + dir[0], col + dir[1], grid);
        }
    }
}
```

---

## **Step-by-Step Dry Run with Text Diagrams**

Let's walk through a sample grid to see how the algorithm works.

### **Example Grid**

Consider the following grid:

```
1 1 1 1 0
1 1 0 1 0
1 1 0 0 0
0 0 0 0 0
```

**Visual Representation:**

```
Row\Col 0 1 2 3 4
   0    1 1 1 1 0
   1    1 1 0 1 0
   2    1 1 0 0 0
   3    0 0 0 0 0
```

### **Initial Setup**

- **Grid Dimensions:** `row = 4`, `col = 5`
- **Island Count:** `island = 0`

### **Traversal Process**

We'll traverse the grid cell by cell. When we find a `'1'`, we'll perform DFS to mark all connected `'1'`s as `'0'` to indicate they've been visited.

#### **Step 1: Cell (0,0)**
- **Value:** `'1'`
- **Action:** Increment `island` to `1`. Perform DFS from `(0,0)`.

**DFS from (0,0):**
1. Mark `(0,0)` as `'0'`.
2. Explore right to `(0,1)`.
   - **Value:** `'1'`
   - **Action:** Mark `(0,1)` as `'0'`. Continue DFS.
3. From `(0,1)`, explore right to `(0,2)`.
   - **Value:** `'1'`
   - **Action:** Mark `(0,2)` as `'0'`. Continue DFS.
4. From `(0,2)`, explore right to `(0,3)`.
   - **Value:** `'1'`
   - **Action:** Mark `(0,3)` as `'0'`. Continue DFS.
5. From `(0,3)`, explore right to `(0,4)`.
   - **Value:** `'0'` (water). Stop.
6. From `(0,3)`, explore down to `(1,3)`.
   - **Value:** `'1'`
   - **Action:** Mark `(1,3)` as `'0'`. Continue DFS.
7. From `(1,3)`, explore right to `(1,4)`.
   - **Value:** `'0'` (water). Stop.
8. From `(1,3)`, explore down to `(2,3)`.
   - **Value:** `'0'` (water). Stop.
9. From `(1,3)`, explore left to `(1,2)`.
   - **Value:** `'0'` (water). Stop.
10. From `(1,3)`, explore up to `(0,3)`.
    - **Value:** `'0'` (already marked). Stop.
11. Continue backtracking. All connected `'1'`s are marked.

**Grid After DFS:**

```
0 0 0 0 0
0 0 0 0 0
1 1 0 0 0
0 0 0 0 0
```

#### **Step 2: Cells (0,1), (0,2), (0,3), (0,4)**
- **Value:** `'0'` (already marked). **Action:** Skip.

#### **Step 3: Cell (1,0)**
- **Value:** `'1'`
- **Action:** Increment `island` to `2`. Perform DFS from `(1,0)`.

**DFS from (1,0):**
1. Mark `(1,0)` as `'0'`.
2. Explore right to `(1,1)`.
   - **Value:** `'1'`
   - **Action:** Mark `(1,1)` as `'0'`. Continue DFS.
3. From `(1,1)`, explore right to `(1,2)`.
   - **Value:** `'0'` (water). Stop.
4. From `(1,1)`, explore down to `(2,1)`.
   - **Value:** `'1'`
   - **Action:** Mark `(2,1)` as `'0'`. Continue DFS.
5. From `(2,1)`, explore right to `(2,2)`.
   - **Value:** `'0'` (water). Stop.
6. From `(2,1)`, explore down to `(3,1)`.
   - **Value:** `'0'` (water). Stop.
7. From `(2,1)`, explore left to `(2,0)`.
   - **Value:** `'1'`
   - **Action:** Mark `(2,0)` as `'0'`. Continue DFS.
8. From `(2,0)`, explore right to `(2,1)`.
   - **Value:** `'0'` (already marked). Stop.
9. From `(2,0)`, explore down to `(3,0)`.
   - **Value:** `'0'` (water). Stop.
10. From `(2,0)`, explore left to `(2,-1)` (out of bounds). Stop.
11. From `(2,0)`, explore up to `(1,0)`.
    - **Value:** `'0'` (already marked). Stop.
12. Continue backtracking. All connected `'1'`s are marked.

**Grid After DFS:**

```
0 0 0 0 0
0 0 0 0 0
0 0 0 0 0
0 0 0 0 0
```

#### **Step 4: Remaining Cells**
- All remaining cells are `'0'` (water). **Action:** Skip.

### **Final Count**

- **Total Islands:** `2`

**Output:** `2`

---

## **Textual Diagrams**

To further aid understanding, here's a textual diagram illustrating the process.

### **Initial Grid:**

```
1 1 1 1 0
1 1 0 1 0
1 1 0 0 0
0 0 0 0 0
```

### **After First DFS (Island 1):**

```
0 0 0 0 0
0 0 0 0 0
1 1 0 0 0
0 0 0 0 0
```

### **After Second DFS (Island 2):**

```
0 0 0 0 0
0 0 0 0 0
0 0 0 0 0
0 0 0 0 0
```

---

## **Reasoning Behind Choosing DFS Approach**

### **Why Depth-First Search (DFS)?**

1. **Connected Components Identification:** DFS is excellent for identifying and traversing all connected components (islands) in a grid.
2. **Efficiency:** Each cell is visited exactly once, leading to an optimal time complexity.
3. **Simplicity:** The recursive nature of DFS makes it straightforward to implement for this problem.

### **Alternative Approaches:**

- **Breadth-First Search (BFS):** Similar to DFS in terms of performance but uses a queue instead of recursion. BFS can also be used effectively for this problem.
- **Union-Find (Disjoint Set):** Can be used to group connected lands but is generally more complex to implement for beginners.

Given the problem's nature, DFS strikes a balance between efficiency and simplicity, making it an ideal choice.

---

## **Time and Space Complexity**

### **Time Complexity: O(m * n)**

- **m:** Number of rows in the grid.
- **n:** Number of columns in the grid.

**Explanation:**

- **Traversal:** Each cell is visited once.
- **DFS Calls:** In the worst case, where the grid is filled with land (`'1'`), DFS will visit all cells, resulting in O(m * n) time.

### **Space Complexity: O(m * n)**

**Explanation:**

- **Recursive Call Stack:** In the worst case (all land), the recursion stack can go up to O(m * n).
- **Grid Modification:** The grid is modified in place, so no additional space is used for marking visited cells.
  
**Note:** If the grid isn't modified in place, additional space (like a visited matrix) would also require O(m * n) space.

---

## **Complete Annotated Code with Detailed Comments**

Here's the complete Java solution with added comments for clarity.

```java
class Solution {
    /**
     * Counts the number of islands in the given grid.
     *
     * @param grid 2D character array representing the map where '1' is land and '0' is water.
     * @return The number of islands.
     */
    public int numIslands(char[][] grid) {
        // Edge case: If grid is null or empty, return 0
        if (grid == null || grid.length == 0) {
            return 0;
        }
        
        int row = grid.length;        // Number of rows
        int col = grid[0].length;     // Number of columns
        
        int island = 0; // Initialize island counter
        
        // Iterate through each cell in the grid
        for(int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                // If the cell is land ('1'), it's part of an island
                if(grid[i][j] == '1') {
                    island++; // Increment island count
                    dfs(i, j, grid); // Perform DFS to mark all connected land cells
                }
            }
        }
        
        return island; // Return the total number of islands found
    }
    
    /**
     * Performs Depth-First Search to mark all connected land cells as visited.
     *
     * @param row Current row index.
     * @param col Current column index.
     * @param grid 2D character array representing the map.
     */
    public void dfs(int row, int col, char[][] grid) {
        int newRow = grid.length;      // Total number of rows
        int newCol = grid[0].length;   // Total number of columns
        
        // Define directions: right, down, left, up
        int [][] directions = new int[][]{{0,1}, {1,0},{0,-1},{-1,0}};
        
        // Base case checks:
        // 1. If the current cell is out of bounds
        // 2. If the current cell is water ('0')
        if(row < 0 || col < 0 || row >= newRow || col >= newCol || grid[row][col] == '0') {
            return; // Stop recursion
        }
        
        // Mark the current cell as visited by setting it to '0'
        grid[row][col] = '0';
        
        // Recursively explore all four directions
        for(int[] dir: directions){
            int newR = row + dir[0]; // Calculate new row index
            int newC = col + dir[1]; // Calculate new column index
            dfs(newR, newC, grid);   // Recursive call for the neighbor cell
        }
    }
}
```

---

## **Additional Example for Clarity**

Let's consider another example to reinforce understanding.

### **Example Grid:**

```
1 1 0 0 0
1 1 0 0 0
0 0 1 0 0
0 0 0 1 1
```

**Visual Representation:**

```
Row\Col 0 1 2 3 4
   0    1 1 0 0 0
   1    1 1 0 0 0
   2    0 0 1 0 0
   3    0 0 0 1 1
```

### **Traversal Process:**

1. **Cell (0,0):** `'1'` → **Island Count:** `1` → Perform DFS.
   - Mark `(0,0)` as `'0'`.
   - Explore right `(0,1)`: `'1'` → Mark `'0'`.
     - Explore right `(0,2)`: `'0'` → Stop.
     - Explore down `(1,1)`: `'1'` → Mark `'0'`.
       - Explore right `(1,2)`: `'0'` → Stop.
       - Explore down `(2,1)`: `'0'` → Stop.
       - Explore left `(1,0)`: `'1'` → Mark `'0'`.
         - Explore left `(1,-1)`: Out of bounds → Stop.
         - Explore down `(2,0)`: `'0'` → Stop.
         - Explore up `(0,0)`: `'0'` → Stop.
   - All connected `'1'`s for Island 1 are marked.

2. **Cells (0,1), (1,0), (1,1):** `'0'` (already marked). **Action:** Skip.

3. **Cell (2,2):** `'1'` → **Island Count:** `2` → Perform DFS.
   - Mark `(2,2)` as `'0'`.
   - Explore all directions: All neighbors are `'0'` → Stop.

4. **Cell (3,3):** `'1'` → **Island Count:** `3` → Perform DFS.
   - Mark `(3,3)` as `'0'`.
   - Explore right `(3,4)`: `'1'` → Mark `'0'`.
     - Explore right `(3,5)`: Out of bounds → Stop.
     - Explore down `(4,4)`: Out of bounds → Stop.
     - Explore left `(3,3)`: `'0'` → Stop.
     - Explore up `(2,4)`: `'0'` → Stop.
   - All connected `'1'`s for Island 3 are marked.

**Final Count:** `3`

**Output:** `3`

---

## **Interview Questions and Answers**

To assess your understanding of the **Number of Islands** problem and the DFS-based solution, here are some potential interview questions along with their answers.

### **1. What is the main objective of the Number of Islands problem?**

**Answer:** The main objective is to determine the number of distinct islands in a 2D grid, where an island is formed by connecting adjacent land cells (`'1'`) horizontally or vertically. Diagonal connections do not count.

### **2. Why is Depth-First Search (DFS) an appropriate choice for solving this problem?**

**Answer:** DFS is suitable because it efficiently explores all connected land cells of an island before moving to the next. It ensures that all parts of an island are visited and marked, preventing recounting, and operates with optimal time complexity.

### **3. Can you explain how the DFS function works in this solution?**

**Answer:** The DFS function starts from a land cell (`'1'`), marks it as visited by setting it to `'0'`, and recursively explores all four adjacent directions (right, down, left, up). This process continues until all connected land cells of the current island are visited and marked, ensuring they aren't counted again.

### **4. What is the time complexity of your solution, and why?**

**Answer:** The time complexity is O(m * n), where m is the number of rows and n is the number of columns in the grid. This is because each cell is visited at most once during the traversal.

### **5. What is the space complexity of your solution?**

**Answer:** The space complexity is O(m * n) in the worst case due to the recursion stack in DFS. If the grid is filled with land, the recursion can go as deep as m * n.

### **6. How does your solution handle grids with no land cells?**

**Answer:** If the grid contains no land cells (`'1'`), the algorithm will iterate through all cells without finding any `'1'`. Consequently, the island count remains `0`, which is correctly returned.

### **7. How would your solution change if diagonal connections also counted as part of an island?**

**Answer:** To account for diagonal connections, we would need to include additional directions in the DFS function. Specifically, we would add the four diagonal directions: top-left, top-right, bottom-left, and bottom-right, making a total of eight possible directions to explore from each cell.

**Modified Directions:**
```java
int [][] directions = new int[][]{
    {0,1}, {1,0}, {0,-1}, {-1,0},  // Right, Down, Left, Up
    {-1,-1}, {-1,1}, {1,-1}, {1,1} // Diagonals
};
```

### **8. How does your solution ensure that each island is counted only once?**

**Answer:** Once a land cell is identified as part of an island, DFS marks it and all connected land cells as visited by setting them to `'0'`. This prevents the algorithm from recounting any of these cells as part of another island.

### **9. Could your solution be implemented iteratively instead of recursively? If so, how?**

**Answer:** Yes, the solution can be implemented iteratively using a stack (for DFS) or a queue (for BFS). In an iterative DFS, we use a stack to keep track of cells to visit. Here's a brief outline using a stack:

```java
public void iterativeDFS(int row, int col, char[][] grid) {
    Stack<int[]> stack = new Stack<>();
    stack.push(new int[]{row, col});
    
    while(!stack.isEmpty()) {
        int[] cell = stack.pop();
        int r = cell[0];
        int c = cell[1];
        
        // Boundary and visited checks
        if(r < 0 || c < 0 || r >= grid.length || c >= grid[0].length || grid[r][c] == '0') {
            continue;
        }
        
        grid[r][c] = '0'; // Mark as visited
        
        // Push all adjacent cells to the stack
        stack.push(new int[]{r+1, c});
        stack.push(new int[]{r-1, c});
        stack.push(new int[]{r, c+1});
        stack.push(new int[]{r, c-1});
    }
}
```

### **10. How would you modify your solution to return the size of the largest island?**

**Answer:** To find the size of the largest island, we can introduce a counter within the DFS function to keep track of the number of cells visited during each island traversal. We then compare and store the maximum size found.

**Modified Code Snippet:**
```java
public int maxAreaOfIsland(char[][] grid) {
    if (grid == null || grid.length == 0) {
        return 0;
    }
    
    int row = grid.length;
    int col = grid[0].length;
    
    int maxArea = 0;
    
    for(int i = 0; i < row; i++) {
        for (int j = 0; j < col; j++) {
            if(grid[i][j] == '1') {
                int area = dfsCount(i, j, grid);
                maxArea = Math.max(maxArea, area);
            }
        }
    }
    
    return maxArea;
}

public int dfsCount(int row, int col, char[][] grid) {
    int newRow = grid.length;
    int newCol = grid[0].length;
    
    int [][] directions = new int[][]{{0,1}, {1,0},{0,-1},{-1,0}};
    
    if(row < 0 || col < 0 || row >= newRow || col >= newCol || grid[row][col] == '0') {
        return 0;
    }
    
    grid[row][col] = '0'; // Mark as visited
    int count = 1; // Current cell
    
    for(int[] dir: directions){
        count += dfsCount(row + dir[0], col + dir[1], grid);
    }
    
    return count;
}
```

---

## **Conclusion**

The provided Java solution efficiently counts the number of islands in a 2D grid using Depth-First Search (DFS). By iterating through each cell and performing DFS on unvisited land cells, the algorithm ensures that each island is counted exactly once. Understanding this approach equips you with a fundamental graph traversal technique applicable to various similar problems.

---

## **Understanding Space Complexity in DFS Solution**

### **What is Space Complexity?**

**Space Complexity** refers to the amount of memory an algorithm uses in terms of the size of the input data. It accounts for both the space needed for the input itself and any additional space used by the algorithm during its execution.

### **Space Complexity of the DFS Solution**

In the **DFS-based solution** for the Number of Islands problem, the space complexity primarily arises from the **recursion stack**. Here's why:

- **Recursion Stack in DFS:**
  - DFS explores as far as possible along each branch before backtracking. This means that in the worst-case scenario, where the grid is entirely land (`'1'`), the recursion can go as deep as the total number of cells in the grid.
  - For a grid with `m` rows and `n` columns, there are `m * n` cells. Therefore, the maximum depth of recursion can be `m * n`, leading to a space complexity of **O(m * n)**.

- **Grid Modification:**
  - The grid is modified in place to mark visited cells by changing `'1'` to `'0'`. This does not require additional space but alters the input grid.

- **Additional Variables:**
  - Other variables used in the algorithm (like loop counters and temporary variables) consume **constant space**, which is negligible compared to the recursion stack.

**Final Space Complexity:** **O(m * n)** in the worst case.

**Illustration:**

Consider a grid where all cells are land:
```
1 1 1
1 1 1
1 1 1
```
- DFS will start at `(0,0)` and recursively visit `(0,1)`, `(0,2)`, `(1,2)`, `(2,2)`, `(2,1)`, `(2,0)`, `(1,0)`, and `(1,1)`.
- The recursion stack grows with each call until all cells are visited.
- Total recursive calls: `m * n = 3 * 3 = 9`.

---

## **Breadth-First Search (BFS) Solution**

While DFS is effective, **BFS** offers an alternative approach that can be more efficient in practice, especially for large grids, as it avoids deep recursion and potential stack overflow issues.

### **1. Overview of the BFS Approach**

**BFS** explores the grid level by level using a queue. For the Number of Islands problem, BFS can systematically traverse all adjacent land cells connected to a starting land cell.

**Steps:**

1. **Traverse the Grid:** Iterate through each cell in the grid.
2. **Identify Land Cells:** When a land cell (`'1'`) is found, it's part of an island.
3. **Mark Visited Cells:** Use BFS to mark all connected land cells of the current island to avoid recounting them.
4. **Count Islands:** Increment the island count each time a new unvisited land cell is found.

### **2. BFS-Based Solution in Java with Detailed Comments**

```java
import java.util.LinkedList;
import java.util.Queue;

class Solution {
    /**
     * Counts the number of islands in the given grid using BFS.
     *
     * @param grid 2D character array representing the map where '1' is land and '0' is water.
     * @return The number of islands.
     */
    public int numIslands(char[][] grid) {
        // Edge case: If grid is null or empty, return 0
        if (grid == null || grid.length == 0) {
            return 0;
        }

        int row = grid.length;        // Number of rows
        int col = grid[0].length;     // Number of columns

        int island = 0; // Initialize island counter

        // Iterate through each cell in the grid
        for(int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                // If the cell is land ('1'), it's part of an island
                if(grid[i][j] == '1') {
                    island++; // Increment island count
                    bfs(i, j, grid); // Perform BFS to mark all connected land cells
                }
            }
        }

        return island; // Return the total number of islands found
    }

    /**
     * Performs Breadth-First Search to traverse and mark connected land cells as visited.
     *
     * @param startRow Starting row index.
     * @param startCol Starting column index.
     * @param grid     2D character array representing the map.
     */
    public void bfs(int startRow, int startCol, char[][] grid) {
        int row = grid.length;
        int col = grid[0].length;

        // Define directions: right, down, left, up
        int[][] directions = new int[][]{{0,1}, {1,0}, {0,-1}, {-1,0}};

        Queue<int[]> queue = new LinkedList<>();
        queue.offer(new int[]{startRow, startCol});
        grid[startRow][startCol] = '0'; // Mark as visited

        while(!queue.isEmpty()) {
            int[] current = queue.poll();
            int currentRow = current[0];
            int currentCol = current[1];

            // Explore all four directions from the current cell
            for(int[] dir : directions) {
                int newRow = currentRow + dir[0];
                int newCol = currentCol + dir[1];

                // Check if the new cell is within bounds and is land
                if(newRow >= 0 && newRow < row && newCol >= 0 && newCol < col && grid[newRow][newCol] == '1') {
                    queue.offer(new int[]{newRow, newCol}); // Add to queue for further exploration
                    grid[newRow][newCol] = '0'; // Mark as visited
                }
            }
        }
    }
}
```

## **Comparing DFS and BFS Solutions**

Understanding both DFS and BFS approaches provides flexibility in solving graph-related problems. Here's a concise comparison table highlighting their differences in the context of the Number of Islands problem.

### **Detailed Comparison**

| **Feature**              | **Depth-First Search (DFS)**                                                                     | **Breadth-First Search (BFS)**                                                                    |
|--------------------------|-------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------|
| **Traversal Order**      | Goes as deep as possible along each branch before backtracking.                                 | Explores all neighbors at the current depth before moving to nodes at the next depth level.       |
| **Data Structure Used**  | Recursion stack (implicit) or an explicit stack.                                                | Queue (FIFO) to manage the order of exploration.                                                  |
| **Space Complexity**     | O(m * n) in the worst case due to recursion stack.                                              | O(m * n) in the worst case; typically O(min(m, n)) for balanced grids.                           |
| **Time Complexity**      | O(m * n) as each cell is visited once.                                                          | O(m * n) as each cell is visited once.                                                             |
| **Ease of Implementation** | Easy to implement recursively; however, deep recursion can be a drawback.                     | Requires iterative implementation with queue management; slightly more complex.                   |
| **Memory Usage**         | Potentially higher due to recursion stack, especially for large grids.                         | Generally more controlled as it uses a queue, avoiding deep recursion.                             |
| **Risk Factors**         | Stack overflow for very large grids due to deep recursion.                                      | Higher memory usage with large queues but avoids stack overflow.                                   |
| **Practical Considerations** | DFS can be faster in practice due to lower constant factors and better cache performance.     | BFS can be more predictable in terms of memory usage and avoids deep recursive calls.              |
| **Use Cases Beyond Islands** | Suitable for puzzles, mazes, and scenarios requiring exhaustive exploration.                 | Ideal for shortest path problems, level-order traversals, and scenarios needing breadth-wise exploration. |

-------------------------------------------------------------------------------------------------------------------------------------------------------


## Question 5 - Problem (Leetcode Link: [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence))

### Problem Statement
The task is to find the **length of the longest consecutive sequence** in an array of integers. A consecutive sequence is a set of numbers that follow one another, for example, in the sequence `[1, 2, 3, 4]`, the length is 4.

Example 1:

Input: nums = [100,4,200,1,3,2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
Example 2:

Input: nums = [0,3,7,2,5,8,4,6,0,1]
Output: 9

### Approach Overview
To solve this problem efficiently, we use the following approach:
1. Use a **HashSet** to store the elements of the array.
2. Iterate over the array, and whenever we find a number that could be the **start** of a new sequence, we begin counting the length of that sequence.
3. We keep track of the **longest sequence** we find during this process.

### Step-by-Step Explanation

Here's the Java solution:

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        // Utilizing HashSet to efficiently handle non-duplicate elements
        HashSet<Integer> numSet = new HashSet<>();
        for (int num : nums) numSet.add(num);

        // Track the longest consecutive streak found so far
        int longestStreak = 0;

        // Iterate through the set of numbers
        for (int num : numSet) {
            // Check if the current number could be the start of a new consecutive sequence
            if (!numSet.contains(num - 1)) {
                int currNum = num;
                int currStreak = 1;

                // Count the length of the current consecutive streak
                while (numSet.contains(currNum + 1)) {
                    currNum++;
                    currStreak++;
                }

                // Update the longest streak if necessary
                longestStreak = Math.max(longestStreak, currStreak);
            }
        }

        return longestStreak;
    }
}
```

Let’s break this down step by step.

#### 1. HashSet Initialization
```java
HashSet<Integer> numSet = new HashSet<>();
for (int num : nums) numSet.add(num);
```
- **Why Use a HashSet?**: A HashSet helps us to quickly check if a number exists in our collection. The operations to add and check existence in a HashSet are both **O(1)** on average.
- We iterate through the input array `nums` and add each element to the HashSet `numSet`. This ensures that:
  - **Duplicates** are eliminated.
  - We can efficiently check for existence later.

#### 2. Initialize Longest Streak
```java
int longestStreak = 0;
```
- This variable keeps track of the **length of the longest consecutive sequence** found so far.

#### 3. Iterate Over HashSet
```java
for (int num : numSet) {
    if (!numSet.contains(num - 1)) {
        int currNum = num;
        int currStreak = 1;

        while (numSet.contains(currNum + 1)) {
            currNum++;
            currStreak++;
        }

        longestStreak = Math.max(longestStreak, currStreak);
    }
}
```
- We iterate over each number in `numSet`. The key idea is that we want to **start a new sequence** only if the current number is the **start** of that sequence. 
  - To determine if a number is the start of a sequence, we check:
    ```java
    if (!numSet.contains(num - 1))
    ```
    This means that we only start counting from a number if there is **no previous number** (`num - 1`) in the set. For example, if `num` is `1`, and there is no `0` in the set, we know that `1` must be the start of the sequence.
  
- If the current number (`num`) is the start of a sequence, we begin to count its length:
  - We initialize `currNum` to `num` and `currStreak` to `1`.
  - Then, we continue increasing `currNum` as long as `numSet` contains the **next number** (`currNum + 1`):
    ```java
    while (numSet.contains(currNum + 1)) {
        currNum++;
        currStreak++;
    }
    ```
  - This loop will keep running until we have reached the end of the current sequence.
  
- After calculating the length of the current sequence (`currStreak`), we update `longestStreak` if this sequence is longer than any sequence we have seen so far:
  ```java
  longestStreak = Math.max(longestStreak, currStreak);
  ```

#### Example Dry Run
Consider the array `[100, 4, 200, 1, 3, 2]`:
1. After adding all elements to the `HashSet`, we have `{100, 4, 200, 1, 3, 2}`.
2. Iterate over each element in the set:
   - For `100`, `num - 1` (`99`) is **not** in the set, so `100` is the start of a sequence. The sequence only contains `100`, so its length is `1`.
   - For `4`, `num - 1` (`3`) **is** in the set, so `4` is **not** the start of a new sequence.
   - For `200`, `num - 1` (`199`) is **not** in the set, so `200` is the start of a sequence. The sequence only contains `200`, so its length is `1`.
   - For `1`, `num - 1` (`0`) is **not** in the set, so `1` is the start of a new sequence:
     - `1` -> `2` -> `3` -> `4` forms a sequence of length `4`.
3. The **longest sequence** is `[1, 2, 3, 4]`, and the length is `4`.

### Complexity Analysis
- **Time Complexity**: **O(n)** where `n` is the number of elements in the array.
  - We add all elements to the `HashSet` in **O(n)** time.
  - We then iterate over each element, and each element is processed only once in the while loop.
- **Space Complexity**: **O(n)** for storing all the elements in the `HashSet`.

### Key Takeaways for Interviews
- The key to this solution is the **use of a HashSet** to efficiently store and lookup elements.
- We **only start counting** when we find a **number that could be the start** of a sequence. This reduces unnecessary work and helps us maintain an **O(n)** time complexity.
- **Why HashSet?** HashSet provides average **O(1)** lookup time, making it ideal for this problem.

By explaining the reasoning behind each step, including a dry run, and providing a complexity analysis, even a beginner should be able to grasp the solution approach.

-------------------------------------------------------------------------------------------------------------------------------------------------------


## Question 5 - Problem (Leetcode Link: [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence))
