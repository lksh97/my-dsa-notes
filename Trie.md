### Improved Notes for Trie Data Structure (Insertion and Search)

### What is a Trie?
A **Trie** (pronounced "try") is a specialized tree-like data structure used to store a dynamic set of strings. It is designed to provide efficient searching, especially for prefix-matching operations. The search time in a Trie is proportional to the length of the string being searched, making it very fast for retrievals.

**Key Characteristics:**
1. **Nodes for Characters:** Each node in a Trie represents a single character of a string.
2. **Common Prefixes:** Strings sharing common prefixes use the same path in the Trie, making it space-efficient.
3. **End of Word Marker:** Each node contains a boolean `isEndOfWord` to mark if the path from the root to that node forms a complete word.

**Example:** For the words "and," "ant," "dad," and "do," the Trie structure is visualized as:
- The root node is empty.
- Each branch represents a unique path of characters.
- Nodes that signify the end of a word are marked as `isEndOfWord = true`.

**Text Diagram:**
```
Root
 ├── a
 │   ├── n
 │   │   ├── d (end of "and")
 │   │   └── t (end of "ant")
 └── d
     ├── a
     │   └── d (end of "dad")
     └── o (end of "do")
```
The above diagram shows how the words "and," "ant," "dad," and "do" are stored in a Trie, sharing common prefixes like 'a' and 'd'.

### Structure of a Trie Node
A typical Trie node contains:
- An array of child nodes (pointers) representing each character (e.g., for the 26 lowercase English letters).
- A boolean `isEndOfWord` to indicate if the path up to this node forms a valid word.

### Insertion in a Trie
**Steps to Insert a Word:**
1. Start from the root.
2. For each character in the word, check if the current node has a child node representing the character.
   - If not, create a new node.
   - If yes, move to that node.
3. Mark the last node of the word with `isEndOfWord = true`.

**Example Insertion:**
- Insert "and": Create nodes for 'a', 'n', 'd', and mark the last node as the end of the word.
- Insert "ant": Reuse nodes for 'a' and 'n', then create a node for 't' and mark it as the end of the word.

### Search Operation in a Trie
**Steps to Search for a Word:**
1. Start from the root.
2. For each character in the word, check if the current node has a child node representing the character.
   - If not, the word doesn't exist in the Trie.
   - If yes, move to the next node.
3. If you reach the end of the word and the last node is marked with `isEndOfWord = true`, the word exists in the Trie.

**Example Search:**
- To search for "dad," traverse through nodes 'd' -> 'a' -> 'd'. Check `isEndOfWord` in the last node; if true, the word exists.

### Code Implementation in Java
```java
public class Trie {
    
    static final int ALPHABET_SIZE = 26;
    
    // Trie node
    static class TrieNode {
        TrieNode[] children = new TrieNode[ALPHABET_SIZE];
        boolean isEndOfWord;
        
        TrieNode() {
            isEndOfWord = false;
            for (int i = 0; i < ALPHABET_SIZE; i++)
                children[i] = null;
        }
    };
     
    static TrieNode root; 
    
    // Insert a key into the Trie
    static void insert(String key) {
        int length = key.length();
        TrieNode pCrawl = root;
     
        for (int level = 0; level < length; level++) {
            int index = key.charAt(level) - 'a';
            if (pCrawl.children[index] == null)
                pCrawl.children[index] = new TrieNode();
            pCrawl = pCrawl.children[index];
        }
     
        // Mark the last node as the end of a word
        pCrawl.isEndOfWord = true;
    }
     
    // Search for a key in the Trie
    static boolean search(String key) {
        int length = key.length();
        TrieNode pCrawl = root;
     
        for (int level = 0; level < length; level++) {
            int index = key.charAt(level) - 'a';
            if (pCrawl.children[index] == null)
                return false;
            pCrawl = pCrawl.children[index];
        }
     
        return (pCrawl.isEndOfWord);
    }
     
    // Driver
    public static void main(String args[]) {
        String keys[] = {"the", "a", "there", "answer", "any", "by", "bye", "their"};
        String output[] = {"Not present in trie", "Present in trie"};
     
        root = new TrieNode();
     
        // Construct the Trie
        for (String key : keys)
            insert(key);
     
        // Search for different keys
        System.out.println("the --- " + (search("the") ? output[1] : output[0]));
        System.out.println("these --- " + (search("these") ? output[1] : output[0]));
        System.out.println("their --- " + (search("their") ? output[1] : output[0]));
        System.out.println("thaw --- " + (search("thaw") ? output[1] : output[0]));
    }
}
```

### Output
```
the --- Present in trie
these --- Not present in trie
their --- Present in trie
thaw --- Not present in trie
```

### Complexity Analysis
- **Insertion:** O(n), where n is the length of the word. Each character in the word is processed one at a time.
- **Searching:** O(n), similar to insertion, where n is the length of the word.
- **Space Complexity:** O(ALPHABET_SIZE * key_length * N), where ALPHABET_SIZE is 26 (for lowercase English), `key_length` is the maximum length of a word, and N is the total number of words in the Trie.

### Advantages of Tries
1. **Fast Search:** Trie allows for quick searches by breaking down words into characters, enabling prefix searches in O(n) time.
2. **Memory Efficiency:** Words with common prefixes share nodes, reducing the space used.
3. **Prefix Matching:** Useful in applications like auto-complete or spell-check.

### Disadvantages
1. **Space-Intensive:** For large sets of strings or alphabets, Tries can consume significant memory.
2. **Complex Implementation:** Unlike hash tables, Tries are not available in standard libraries and require custom implementation.

### Additional Example
For the input strings: `"apple", "app", "apt", "bat", "ball"`:
- Insert "apple": Create nodes for 'a' -> 'p' -> 'p' -> 'l' -> 'e'.
- Insert "app": Reuse nodes for 'a' -> 'p' -> 'p' and mark the last node as the end.
- Insert "apt": Reuse 'a' -> 'p', create 't'.

The structure of the Trie after these insertions will share common prefixes efficiently.

### Conclusion
Tries provide an efficient way to handle prefix-based searches. They are commonly used in applications like dictionaries, search engines, and spell-checkers. While they offer optimal search times, they may require substantial memory for large datasets.

---------------------------------------------------------------------------------------------------------------------------------------------------

## **Improved Notes on Deletion in a Trie with Detailed Explanation, Code, and Dry Run**

### **Trie Recap**
A Trie is a tree-like data structure used to efficiently store and search strings. Each node represents a character of the string, and strings that share common prefixes share the same path in the Trie. The `isEndOfWord` flag in each node helps to identify the end of a valid word.

### **Deletion in a Trie**
Deletion in a Trie is more complex than insertion and searching because we need to handle multiple cases, such as:
1. **Case 1:** The key may not be present in the Trie. In this case, we simply return without making any changes.
2. **Case 2:** The key is a unique string in the Trie (no prefix overlap with other keys). In this case, we delete all nodes representing the key.
3. **Case 3:** The key is a prefix of another word in the Trie. Here, we only need to unmark the `isEndOfWord` flag for the last character.
4. **Case 4:** The key is present in the Trie and other words share the same prefix. In this case, we remove nodes from the end of the key until we reach the first node that is shared by other words.

### **Steps to Implement Deletion in a Trie**
1. **Traverse the Trie:** Navigate through the characters of the key in the Trie.
2. **Unmark the Leaf Node:** If you reach the last character of the key, unmark its `isEndOfWord` flag.
3. **Remove Unnecessary Nodes:** Recursively check if the current node has any children. If not, remove it.

### **Code Implementation in Java**
Here is the complete code with improvements and comments for a beginner's understanding:

```java
public class Trie {
    static final int ALPHABET_SIZE = 26;
    
    // Trie Node class
    static class TrieNode {
        TrieNode[] children = new TrieNode[ALPHABET_SIZE];
        boolean isEndOfWord;
        
        // Constructor to initialize the TrieNode
        public TrieNode() {
            isEndOfWord = false;
            for (int i = 0; i < ALPHABET_SIZE; i++) {
                children[i] = null;
            }
        }
    }
    
    static TrieNode root;
    
    // Insert a key into the Trie
    static void insert(String key) {
        int length = key.length();
        TrieNode pCrawl = root;
        for (int level = 0; level < length; level++) {
            int index = key.charAt(level) - 'a';
            if (pCrawl.children[index] == null) {
                pCrawl.children[index] = new TrieNode();
            }
            pCrawl = pCrawl.children[index];
        }
        // Mark the last node as end of a word
        pCrawl.isEndOfWord = true;
    }
    
    // Search for a key in the Trie
    static boolean search(String key) {
        int length = key.length();
        TrieNode pCrawl = root;
        for (int level = 0; level < length; level++) {
            int index = key.charAt(level) - 'a';
            if (pCrawl.children[index] == null) {
                return false;
            }
            pCrawl = pCrawl.children[index];
        }
        // Return true if the end of the word is reached
        return (pCrawl != null && pCrawl.isEndOfWord);
    }
    
    // Check if a node has no children
    static boolean hasNoChild(TrieNode currentNode) {
        for (int i = 0; i < currentNode.children.length; i++) {
            if (currentNode.children[i] != null) {
                return false;
            }
        }
        return true;
    }
    
    // Utility function to delete a key from the Trie
    static boolean removeUtil(TrieNode currentNode, String key, int level, int length) {
        // Base case: if Trie is empty
        if (currentNode == null) {
            return false;
        }
        
        // If last character of key is being processed
        if (level == length) {
            // Unmark the current node as end of word
            if (currentNode.isEndOfWord) {
                currentNode.isEndOfWord = false;
                // If the node has no children, it can be deleted
                return hasNoChild(currentNode);
            }
            return false;
        }
        
        // Recur for the child node
        int index = key.charAt(level) - 'a';
        boolean shouldDeleteCurrentNode = removeUtil(currentNode.children[index], key, level + 1, length);
        
        // If true is returned, delete the child node
        if (shouldDeleteCurrentNode) {
            currentNode.children[index] = null;
            // Check if the current node has any children or is marked as the end of another word
            return !currentNode.isEndOfWord && hasNoChild(currentNode);
        }
        return false;
    }
    
    // Wrapper function to delete a key
    static void remove(String key) {
        int length = key.length();
        if (length > 0) {
            removeUtil(root, key, 0, length);
        }
    }

    // Driver Code    
    public static void main(String[] args) {
        root = new TrieNode();
    
        String keys[] = {"the", "a", "there", "answer", "any", "by", "bye", "their", "hero", "heroplane"};
        
        // Construct Trie
        for (String key : keys) {
            insert(key);
        }
    
        // Search for different keys
        System.out.println(search("the") ? "Yes" : "No"); // Output: Yes
        System.out.println(search("these") ? "Yes" : "No"); // Output: No
    
        // Remove a key
        remove("heroplane");
        System.out.println(search("hero") ? "Yes" : "No"); // Output: Yes
    }
}
```

### **Output Explanation**
```
Yes
No
Yes
```
- The first output "Yes" indicates that "the" is present in the Trie.
- The second output "No" shows that "these" is not present in the Trie.
- After removing "heroplane," the output "Yes" shows that "hero" is still present in the Trie.

### **Dry Run of the Code**
**Let's dry-run the deletion of the word "heroplane" in the Trie:**

1. **Initial Trie Structure:**
   - Inserted keys: "hero", "heroplane".
   - Trie nodes created for "h" -> "e" -> "r" -> "o" -> "p" -> "l" -> "a" -> "n" -> "e".

2. **Delete "heroplane":**
   - Start at the root.
   - Navigate to "h" -> "e" -> "r" -> "o" -> "p" -> "l" -> "a" -> "n" -> "e".
   - Unmark the node for "e" as the end of the word.
   - Check if this node has no children. Since it has no children, delete it.
   - Move up to the node for "n," delete it (no children).
   - Continue this process up to "p".
   - Stop at "o" because it is part of another word ("hero").
   
3. **Result:** "heroplane" is removed, but "hero" still exists.

### **Time Complexity:**
- **Insertion:** O(n), where n is the length of the key.
- **Deletion:** O(n), where n is the length of the key. The deletion process traverses the length of the key and removes nodes if necessary.
- **Space Complexity:** O(ALPHABET_SIZE * key_length * N), where `ALPHABET_SIZE` is 26 (for lowercase English letters), `key_length` is the maximum length of a word, and `N` is the total number of words.

### **Key Takeaways for Beginners:**
1. **Tries** are efficient for storing and searching strings, particularly when dealing with large datasets or performing prefix-based searches.
2. **Deletion in a Trie** involves careful consideration of whether nodes are part of other words or prefixes.
3. The **recursive approach** allows us to navigate through nodes and remove only the necessary parts of the Trie.

### **Additional Example**
For the Trie containing "bat," "ball," and "batman":
- Deleting "bat" will only unmark the end of "bat" without affecting "batman."
- Deleting "batman" will remove the nodes for 'm', 'a', and 'n', leaving "bat" and "ball" intact.

### **Conclusion**
Understanding Tries and their operations, especially deletion, is crucial for handling large sets of strings efficiently. This guide provides a detailed explanation of how to implement and handle different cases of deletion in a Trie.

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


## **Implementing Auto-Complete Feature using Trie**

### **What is the Auto-Complete Feature?**
The auto-complete feature is used to suggest possible completions of a word or phrase as the user types. For example, if the dictionary contains the words: `"abc"`, `"abcd"`, `"aa"`, `"abbbaba"`, and the user types `"ab"`, they should receive suggestions like `"abc"`, `"abcd"`, `"abbbaba"`.

### **How to Implement Auto-Complete using a Trie?**
A **Trie** (pronounced "try") is a tree-like data structure used to store a large collection of strings and efficiently perform operations such as insertion, search, and prefix matching. For the auto-complete feature:
1. Insert all the words into a Trie.
2. Search for the prefix typed by the user in the Trie.
3. Recursively fetch all words in the Trie that match the prefix.

### **Detailed Algorithm:**
1. **Insertion:**
   - Insert all the given words into the Trie.
   - Each node in the Trie represents a character. Mark the end of a word using a boolean flag (`isWordEnd`).
   
2. **Search for Prefix:**
   - Traverse the Trie using the characters of the input prefix.
   - If the prefix does not exist, return `0` to indicate no matches.

3. **Auto-Complete Suggestions:**
   - Once the prefix is found, recursively explore all paths (children nodes) from the last node of the prefix to find all matching words.

### **Key Points in the Code:**
- **TrieNode:** Each node has 26 children (for each alphabet letter) and a flag to mark the end of a word.
- **Insertion:** The `insert` function inserts words into the Trie.
- **Search for Suggestions:** The `printAutoSuggestions` function searches for all words with the given prefix.

### **Code Implementation:**
Here's the improved code with detailed comments and explanations:

```java
import java.util.*;

class AutoCompleteTrie {
    
    // Alphabet size (# of symbols)
    public static final int ALPHABET_SIZE = 26;

    // Trie node
    static class TrieNode {
        TrieNode[] children = new TrieNode[ALPHABET_SIZE];
        boolean isWordEnd;
    };
    
    // Returns a new Trie node
    static TrieNode getNode() {
        TrieNode node = new TrieNode();
        node.isWordEnd = false;
        for (int i = 0; i < ALPHABET_SIZE; i++) {
            node.children[i] = null;
        }
        return node;
    }
    
    // Insert a word into the Trie
    static void insert(TrieNode root, String key) {
        TrieNode currentNode = root;
        for (int level = 0; level < key.length(); level++) {
            int index = key.charAt(level) - 'a'; // Calculate the index for the character
            if (currentNode.children[index] == null) {
                currentNode.children[index] = getNode(); // Create a new node if it doesn't exist
            }
            currentNode = currentNode.children[index]; // Move to the next level
        }
        // Mark the end of the word
        currentNode.isWordEnd = true;
    }

    // Check if a node has no children
    static boolean isLastNode(TrieNode root) {
        for (int i = 0; i < ALPHABET_SIZE; i++) {
            if (root.children[i] != null) {
                return false;
            }
        }
        return true;
    }
    
    // Recursive function to print all words under the given node
    static void suggestionsRec(TrieNode root, String prefix) {
        if (root.isWordEnd) {
            System.out.println(prefix); // Print the current word
        }
        if (isLastNode(root)) {
            return; // If there are no more children, return
        }
        for (int i = 0; i < ALPHABET_SIZE; i++) {
            if (root.children[i] != null) {
                // Append the current character to the prefix and recursively find more words
                suggestionsRec(root.children[i], prefix + (char)(i + 'a'));
            }
        }
    }
    
    // Function to print suggestions for the given query prefix
    static int printAutoSuggestions(TrieNode root, String query) {
        TrieNode currentNode = root;
        // Traverse the Trie to find the end node of the prefix
        for (int level = 0; level < query.length(); level++) {
            int index = query.charAt(level) - 'a';
            if (currentNode.children[index] == null) {
                return 0; // Prefix not found
            }
            currentNode = currentNode.children[index];
        }

        // If the prefix is also a word
        boolean isWord = currentNode.isWordEnd;
        boolean isLast = isLastNode(currentNode);

        if (isWord && isLast) {
            System.out.println(query);
            return -1; // Indicates there are no further suggestions
        }

        if (!isLast) {
            suggestionsRec(currentNode, query); // Recursively find and print suggestions
            return 1; // Indicates suggestions found
        }
        
        return 0; // No suggestions found
    }
    
    // Driver code
    public static void main(String[] args) {
        TrieNode root = getNode();
        String[] words = {"hello", "dog", "hell", "cat", "a", "hel", "help", "helps", "helping"};
        
        // Insert words into Trie
        for (String word : words) {
            insert(root, word);
        }

        // Test auto-complete feature
        int result = printAutoSuggestions(root, "hel");
        
        if (result == -1) {
            System.out.println("No other strings found with this prefix.");
        } else if (result == 0) {
            System.out.println("No string found with this prefix.");
        }
    }
}
```

### **Output**
```
hel
hell
hello
help
helps
helping
```

### **Dry Run of the Code:**
Let's walk through the code with the query `"hel"`:
1. **Insert Words:** The words `"hello"`, `"dog"`, `"hell"`, `"cat"`, `"a"`, `"hel"`, `"help"`, `"helps"`, and `"helping"` are inserted into the Trie.
2. **Search for Prefix "hel":** 
   - The code traverses through the characters `'h' -> 'e' -> 'l'` in the Trie.
   - Reaches the end of the prefix `"hel"`.
3. **Check if it's a Word:** `"hel"` is marked as the end of a word.
4. **Find Suggestions:** 
   - Since `"hel"` is not a last node, recursively fetch all words starting with `"hel"`:
     - `"hel"` → `"hell"` → `"hello"` → `"help"` → `"helps"` → `"helping"`.
5. **Output:** The suggestions `"hel"`, `"hell"`, `"hello"`, `"help"`, `"helps"`, `"helping"` are printed.

### **Explanation of Key Concepts for Beginners:**
- **TrieNode:** Represents each character in a word. Each node has 26 children (one for each lowercase English letter) and an `isWordEnd` flag to indicate the end of a word.
- **Prefix Matching:** The code finds the node corresponding to the last character of the prefix. Then, it explores all possible paths from this node to find words that share the same prefix.
- **Recursive Exploration:** The `suggestionsRec` function is a recursive function that explores all child nodes to find words with the given prefix.

### **Time Complexity:**
- **Insertion:** O(L), where L is the length of the word being inserted. Each character of the word is inserted one by one.
- **Auto-Completion:** O(P + N), where P is the length of the prefix and N is the number of words that share the prefix. This includes the time to find the prefix node and recursively explore the sub-tree for suggestions.

### **Space Complexity:**
- The space complexity of the Trie is O(N * L), where N is the number of words and L is the average length of the words. The auxiliary space for storing the Trie nodes is proportional to the number of nodes.

### **Additional Example:**
If we insert the words `"ant"`, `"anteater"`, `"antelope"`, `"apple"` into the Trie and search for `"ant"`, the suggestions would be:
```
ant
anteater
antelope
```
This is because all these words start with the prefix `"ant"`.

### **Conclusion:**
Implementing the auto-complete feature using a Trie allows for efficient prefix-based search. This approach is faster than searching through all the words for matching prefixes. The recursive function `suggestionsRec` explores the Trie to find and print all words that share the given prefix.
