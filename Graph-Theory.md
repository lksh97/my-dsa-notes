# 1. Topological Sorting (Kahn's BFS Based Algorithm)

**Topological sorting** for a Directed Acyclic Graph (DAG) is a linear ordering of its vertices such that for every directed edge \( uv \), vertex \( u \) comes before vertex \( v \) in the ordering. Topological Sorting is not possible for graphs that contain cycles.

#### Why Topological Sorting?
- It helps in scheduling tasks in a specific order where some tasks depend on the completion of others.
- It is mainly used in scenarios like task scheduling, course prerequisite management, and resolving dependencies in build systems.

### Example of Topological Sorting
Below is an example of a graph and its possible topological sorts:

<img width="283" alt="graph" src="https://github.com/user-attachments/assets/00a2e6e3-ce78-428c-914d-a8a3fad96685">

![Untitled-Diagram-337](https://github.com/user-attachments/assets/5fdee85b-5b5e-4957-99e1-e937e9009dc7)

**Input Graph**: As shown above, we have a Directed Acyclic Graph (DAG) with vertices 0, 1, 2, 3, 4, and 5.

![Untitled-Diagram-256](https://github.com/user-attachments/assets/927fc8d4-ea47-4876-9367-6ac3cd70ddf0)



**Topological Sort Output**:
- One possible topological sorting: `5 4 2 3 1 0`
- Another possible sorting: `4 5 2 0 3 1`

The order depends on which vertices with an in-degree of 0 are selected first.

#### Steps Involved in Topological Sorting
1. **Calculate In-Degree for Each Node**:
   The **in-degree** is the number of incoming edges to a vertex. We calculate this for all vertices.
2. **Initialize a Queue**:
   Add all vertices with an in-degree of 0 into a queue. These are the starting points, as they have no dependencies.
3. **Process the Queue**:
   - Remove a vertex from the queue.
   - Add it to the topological ordering.
   - Decrease the in-degree of all its neighboring nodes.
   - If the in-degree of a neighboring node becomes 0, add it to the queue.
4. **Repeat Until Queue is Empty**:
   The final list of nodes is the topological ordering.

#### Detailed Example Walkthrough with Kahn's Algorithm
Consider the graph below:

<img width="283" alt="graph" src="https://github.com/user-attachments/assets/b4ddc416-187f-4dab-8461-88081edcc6a9">


1. **Step 1: Calculate In-Degree**
   - Node 5: In-degree = 0
   - Node 4: In-degree = 0
   - Node 2: In-degree = 1 (from node 5)
   - Node 0: In-degree = 2 (from nodes 5 and 4)
   - Node 3: In-degree = 1 (from node 2)
   - Node 1: In-degree = 2 (from nodes 3 and 4)

2. **Step 2: Initialize a Queue**
   - Start with nodes with in-degree 0: [5, 4]

3. **Step 3: Process the Queue**
   - **Dequeue 5**:
     - Topological order so far: [5]
     - Reduce in-degree of node 2 (now 0), add to queue.
     - Reduce in-degree of node 0 (now 1).
   - **Dequeue 4**:
     - Topological order so far: [5, 4]
     - Reduce in-degree of node 0 (now 0), add to queue.
     - Reduce in-degree of node 1 (now 1).
   - **Dequeue 2**:
     - Topological order so far: [5, 4, 2]
     - Reduce in-degree of node 3 (now 0), add to queue.
   - **Dequeue 0**:
     - Topological order so far: [5, 4, 2, 0]
   - **Dequeue 3**:
     - Topological order so far: [5, 4, 2, 0, 3]
     - Reduce in-degree of node 1 (now 0), add to queue.
   - **Dequeue 1**:
     - Topological order so far: [5, 4, 2, 0, 3, 1]

#### Algorithm: Kahn's Algorithm for Topological Sorting
The algorithm is implemented as follows:

```java
import java.util.*;

// Class to represent a directed acyclic graph
class Graph {
    private int numVertices;  // Number of vertices
    private List<Integer>[] adjacencyList;  // Adjacency list representation of the graph

    // Constructor to initialize the graph with given vertices
    public Graph(int numVertices) {
        this.numVertices = numVertices;
        adjacencyList = new ArrayList[numVertices];
        for (int i = 0; i < numVertices; i++) {
            adjacencyList[i] = new ArrayList<>(); // Initialize each adjacency list
        }
    }

    // Method to add a directed edge from vertex 'u' to vertex 'v'
    public void addEdge(int u, int v) {
        adjacencyList[u].add(v); // Add 'v' to adjacency list of 'u'
    }

    // Method to perform Topological Sort using Kahn's Algorithm (BFS approach)
    public void topologicalSort() {
        int[] inDegree = new int[numVertices];  // Array to keep track of in-degrees of vertices

        // Step 1: Calculate in-degrees of all vertices
        for (int i = 0; i < numVertices; i++) {
            for (int node : adjacencyList[i]) {
                inDegree[node]++; // Increment in-degree for each outgoing edge
            }
        }

        // Step 2: Add all vertices with in-degree 0 to the queue
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < numVertices; i++) {
            if (inDegree[i] == 0) {
                queue.add(i); // Add vertices with no incoming edges to the queue
            }
        }

        // Step 3: Initialize count of visited vertices and list to store topological order
        int visitedVerticesCount = 0;
        List<Integer> topologicalOrder = new ArrayList<>();

        // Process the queue until it is empty
        while (!queue.isEmpty()) {
            // Step 4: Extract the front of the queue (dequeue) and add it to topological order
            int currentVertex = queue.poll();
            topologicalOrder.add(currentVertex);

            // Step 5: Iterate through all neighboring nodes of dequeued vertex
            // and decrease their in-degree by 1
            for (int neighbor : adjacencyList[currentVertex]) {
                inDegree[neighbor]--; // Reduce in-degree by 1
                if (inDegree[neighbor] == 0) {
                    // If in-degree becomes zero, add the neighbor to the queue
                    queue.add(neighbor);
                }
            }
            visitedVerticesCount++; // Increment count of visited vertices
        }

        // Step 6: Check if there was a cycle in the graph
        if (visitedVerticesCount != numVertices) {
            System.out.println("There exists a cycle in the graph, so topological sorting is not possible.");
            return; // If all nodes were not visited, the graph contains a cycle
        }

        // Step 7: Print the topological order
        System.out.println("Topological Sort:");
        for (int vertex : topologicalOrder) {
            System.out.print(vertex + " ");
        }
    }
}

// Main class to test the functionality of the Graph class
public class Main {
    public static void main(String args[]) {
        // Create a graph with 6 vertices (0 to 5)
        Graph graph = new Graph(6);
        // Adding edges to the graph
        graph.addEdge(5, 2);
        graph.addEdge(5, 0);
        graph.addEdge(4, 0);
        graph.addEdge(4, 1);
        graph.addEdge(2, 3);
        graph.addEdge(3, 1);

        // Perform Topological Sort and display the result
        graph.topologicalSort();
    }
}
```

**Output**:  
`Following is a Topological Sort: 5 4 2 0 3 1`

#### Complexity Analysis
- **Time Complexity**: O(V + E), where V is the number of vertices and E is the number of edges.
  - Calculating in-degrees takes O(V + E) time.
  - Processing each vertex and edge takes O(V + E) time. (This is because each vertex is added to the queue once, and all edges are processed once)
- **Auxiliary Space**: O(V) for storing the in-degrees and the queue.

#### Key Points for Beginners:
1. **Directed Acyclic Graph (DAG)**:
   - A graph with directed edges and no cycles.
   - Topological sorting is only possible for DAGs.
  
2. **In-Degree and Out-Degree**:
   - **In-Degree**: Number of incoming edges to a vertex.
   - **Out-Degree**: Number of outgoing edges from a vertex.

3. **Order of Execution**:
   - Always start with nodes with no dependencies (`in-degree = 0`).
   - After processing a node, update the in-degree of its neighboring nodes.

4. **Multiple Topological Sorts**:
   - A graph can have more than one valid topological order.
   - The final order may depend on which nodes with in-degree 0 are processed first.

### How Calculation of In-Degrees Works:
1. **Initialize In-Degree Array**:
   - Create an array (`inDegree[]`) of size equal to the number of vertices (`V`) in the graph.
   - Initially set all values in `inDegree[]` to 0, indicating no incoming edges have been counted yet.
   
   This step involves looping through each vertex to initialize its in-degree value to 0.
   
   - **Time Complexity for Initialization**: \( O(V) \)

2. **Traverse Adjacency List**:
   - For each vertex, traverse its **adjacency list** to find its **outgoing edges**.
   - Whenever you encounter an outgoing edge pointing to another vertex (`u -> v`), increment the in-degree count of the destination vertex (`v`).
   
   The traversal involves iterating through:
   - **All vertices (`V`)**: You need to visit each vertex to examine its adjacency list.
   - **All edges (`E`)**: For every edge, you increment the in-degree of the destination vertex.
   
   Therefore, this step requires visiting each vertex and iterating over all of its edges.

### Time Complexity Breakdown:
- **Initialization Step**:
  - You need to loop through all `V` vertices to initialize the in-degree array.
  - **Time Complexity**: \( O(V) \)

- **Traversal of Adjacency List**:
  - For each vertex, you traverse its adjacency list, which contains its outgoing edges. You do this for all vertices in the graph.
  - The time complexity of traversing all the adjacency lists is equivalent to visiting each edge exactly once.
  - Thus, the time complexity for this step is **\( O(E) \)**, where `E` is the total number of edges.

### Overall Time Complexity:
- Since we need to **initialize the in-degree array (`O(V)`)** and then **traverse all adjacency lists (`O(E)`)**, the **total time complexity** for calculating in-degrees for all vertices is:
  
  \[
  O(V) + O(E) = O(V + E)
  \]

### Summary:
- The **initialization** of the in-degree array takes \( O(V) \) time.
- **Traversing the adjacency lists** to increment the in-degree values takes \( O(E) \) time.
- Therefore, calculating in-degrees for all vertices in the graph is an \( O(V + E) \) operation.
  
This time complexity is efficient and optimal for graphs, as you need to visit all vertices and all edges at least once to determine the in-degrees of each vertex.

### Visual Representation of the Graph and Topological Sorting
The graph images you've provided represent examples of DAGs used in topological sorting:

1. **Graph Representation**:
   <img width="283" alt="graph" src="https://github.com/user-attachments/assets/fdbe38e8-2216-4727-b50d-5cdf9a33111b">

2. **Topological Sort Example (0, 1, 2, 3, 4)**:
   ![Untitled-Diagram-256](https://github.com/user-attachments/assets/21fbdf39-2c35-4510-a027-427601542fa2)

3. **Topological Sort Example (5, 0, 1, 2, 3, 4)**:


### Conclusion
Topological sorting using Kahn's Algorithm is a simple yet powerful approach for scheduling and ordering tasks where dependencies exist. For beginners, the focus should be on:
- Understanding in-degree and out-degree.
- Visualizing the graph and the topological ordering process.
- Practicing with examples and variations of the graph to understand how different orders can exist.

Calculating in-degrees for all vertices in a graph takes \( O(V + E) \) time complexity because of the following reasons:

### Key Concepts:
- **Vertices (V)**: The nodes in the graph.
- **Edges (E)**: The connections between the vertices in the graph.

In a Directed Acyclic Graph (DAG), the **in-degree** of a vertex is defined as the number of incoming edges pointing to that vertex.


--
### Topological Sorting (Kahn's BFS-Based Algorithm)

**Topological Sorting** is a linear ordering of vertices in a Directed Acyclic Graph (DAG) such that for every directed edge \(uv\), vertex \(u\) comes before vertex \(v\). Topological sorting is not possible if the graph contains a cycle.

#### Why Topological Sorting?
- **Order of Execution**: Helps in scheduling tasks that have dependencies.
- **Real-Life Use Cases**: 
  - **Course Prerequisites**: A course can be taken only after its prerequisites are completed.
  - **Project Planning**: Certain tasks in a project depend on the completion of others.

#### Key Concepts:
- **Directed Acyclic Graph (DAG)**: A graph with directed edges and no cycles.
- **In-Degree**: The number of incoming edges to a vertex.
- **Out-Degree**: The number of outgoing edges from a vertex.

### Example Graph Representation and Topological Sorting
Here are text diagrams that illustrate the concept of topological sorting.

#### Example Graph
```
  5 → 2
  5 → 0
  4 → 0
  4 → 1
  2 → 3
  3 → 1
```
This text diagram represents the edges between the nodes (vertices). For example, "5 → 2" means there is a directed edge from node 5 to node 2.

#### Topological Sorting Examples:
**Example 1: Topological Sort Output**  
```
Order: 5, 4, 2, 0, 3, 1
Explanation:
1. Start with nodes that have no incoming edges: 5 and 4.
2. Process 5 → Add 5 to the order → Nodes affected: 2, 0.
3. Process 4 → Add 4 to the order → Nodes affected: 1, 0.
4. Process 2 → Add 2 to the order → Nodes affected: 3.
5. Process 0 → Add 0 to the order.
6. Process 3 → Add 3 to the order → Nodes affected: 1.
7. Process 1 → Add 1 to the order.

Final Order: 5, 4, 2, 0, 3, 1
```

**Example 2: Another Possible Topological Sort**
```
Order: 4, 5, 2, 0, 3, 1
Explanation:
1. Start with nodes that have no incoming edges: 4 and 5.
2. Process 4 → Add 4 to the order → Nodes affected: 0, 1.
3. Process 5 → Add 5 to the order → Nodes affected: 2, 0.
4. Process 2 → Add 2 to the order → Nodes affected: 3.
5. Process 0 → Add 0 to the order.
6. Process 3 → Add 3 to the order → Nodes affected: 1.
7. Process 1 → Add 1 to the order.

Final Order: 4, 5, 2, 0, 3, 1
```

### Step-by-Step Algorithm for Topological Sorting

**Steps:**
1. **Calculate In-Degree** for each vertex.
2. **Initialize a Queue** with all vertices having in-degree 0.
3. **Process the Queue**:
   - Remove a vertex.
   - Add it to the topological order.
   - Decrease the in-degree of its neighbors by 1.
   - If any neighbor’s in-degree becomes 0, add it to the queue.
4. **Repeat Until the Queue is Empty**.

#### Text Diagram Illustrating Kahn’s Algorithm
```
Step 1: Calculate In-Degree
Vertex    | In-Degree
---------------------
5         | 0
4         | 0
2         | 1
0         | 2
3         | 1
1         | 2

Step 2: Initialize Queue with In-Degree = 0 → [5, 4]

Step 3: Process Queue
Queue     | Current Vertex | Updated In-Degree of Neighbors
----------------------------------------------------------
[5, 4]    | 5              | 2 → 0, 0 → 1
[4, 2]    | 4              | 0 → 0, 1 → 1
[2, 0]    | 2              | 3 → 0
[0, 3]    | 0              | None
[3]       | 3              | 1 → 0
[1]       | 1              | None
```
The **Final Order** from the above steps is `5, 4, 2, 0, 3, 1`.

### Explanation for Beginners:
- **In-Degree**: Counts incoming arrows to each node.
- **Out-Degree**: Counts outgoing arrows.
- **Queue**: Maintains nodes with no dependencies.
- **Topological Order**: Represents the sequence in which tasks can be executed without breaking dependency rules.

### Diagrams Corresponding to Graphs

1. **Initial Graph**:
   ```
     5      4
    ↘ ↘    ↘ ↘
    2 → 3  0 → 1
   ```
   This text diagram represents the original graph with nodes and directed edges.

2. **Topological Sorting Walkthrough** (using numbers to represent order):
   ```
   Step 1: Start with Nodes with In-Degree = 0
   → Nodes: 5, 4

   Step 2: Add 5 → Order: [5]
   → Process nodes from 5 (affecting 2, 0)

   Step 3: Add 4 → Order: [5, 4]
   → Process nodes from 4 (affecting 1, 0)

   Step 4: Continue in this way until all nodes are added.
   ```

### Conclusion
Topological Sorting using Kahn's Algorithm helps understand how tasks dependent on one another can be efficiently ordered. Remember:
- The algorithm starts with nodes that have no dependencies (in-degree 0).
- Multiple topological sorts may exist for the same graph.
- Text diagrams and simple step-by-step walkthroughs help visualize the process.

The additional diagrams and detailed walkthrough provide a clearer path for understanding each step of Kahn’s algorithm in sorting the graph topologically.


# 2. 


