
### INDEX
### Arrays

*   [[#1. Array vs. Linked List (Differences, Benefits/Drawbacks, When to Prefer)]]
*   [[#6. Why Can't You Access a Linked List's 5th Index Directly, While You Can in an Array?]]
*   [[#7. Searching in Array vs. Linked List]]
*   [[#20. Find Missing Number in Sorted/Unsorted Array]]
*   [[#26. Flatten Array: Multi-dimensional to Single-dimensional]]
*   [[#30. Find 2nd Highest Element of Array]]
*   [[#32. Convert Array to BST]]

### Linked Lists

*   [[#1. Array vs. Linked List (Differences, Benefits/Drawbacks, When to Prefer)]]
*   [[#2. Reverse Linked List (Iterative, Recursive, In-Place)]]
*   [[#3. Find Middle Node of Linked List]]
*   [[#5. Insertion in the Middle of a Linked List vs. Array. Any benefit over array?]]
*   [[#15. Linked List (Singly) Delete a Node: (Given access to the node itself, but not the head)]]
*   [[#21. Linked List: Detect and Remove Cycle]]
*   [[#23. Linked List: Delete the Last Occurrence of an Element]]
*   [[#24. Linked List: Reverse Linked List in k Chunks]]
*   [[#31. Linked List: Reverse Linked List Using Recursion]]

### Trees & Graphs

*   [[#4. BST (Binary Search Tree)]]
*   [[#9. Tree vs. Graph]]
*   [[#12. BFS (Breadth-First Search)]]
*   [[#13. DFS (Depth-First Search)]]
*   [[#16. Binary Tree Traversal (Inorder, Preorder, Postorder)]]
*   [[#17. Mirror the BST]]
*   [[#22. Find Distance Between Two Nodes in BST]]
*   [[#25. Given a binary tree every node of tree should have a property of siblings which will point to its next child]]

### Stacks & Queues

*   [[#8. Stack vs. Queue]]
*   [[#14. Implement Queue using Stack]]
*   [[#27. Stack: Real-Life Example]]
*   [[#28. Heap vs Stack]]
*   [[#29. Heap Memory Allocation vs Stack Memory Allocation]]

### Sorting Algorithms

*   [[#10. Sorting Algorithms (General)]]
*   [[#18. Sorting Algorithms: Quick Sort]]
*   [[#19. Sorting Algorithms: Merge Sort]]

### General Concepts

*   [[#11. Time Complexity]]
## 1. Array vs. Linked List (Differences, Benefits/Drawbacks, When to Prefer):

**Array vs. Linked List (Differences, Benefits/Drawbacks, When to Prefer):**

**Definitions:**

*   **Array:** An array is a contiguous block of memory used to store elements of the **same data type**. Elements are accessed using an index (starting from 0). The size is usually fixed at the time of creation.

*   **Linked List:** A linked list is a linear data structure where elements are stored in **nodes**, which are not necessarily stored contiguously in memory. Each node contains the data and a **pointer (or link)** to the next node in the sequence. The list is accessed starting from the **head** node. The size of the linked list can change dynamically.

**Differences:**

*   **Memory Allocation:**
    *   **Array:** Arrays use contiguous memory blocks. The size of an array needs to be determined at the time of its creation.
    *   **Linked List:** Linked Lists use a collection of nodes that are not stored contiguously. Memory is allocated for each node as needed.

*   **Structure:**
    *   **Array:** Array is a fixed-size data structure.
    *   **Linked List:** Linked List is a dynamic-size data structure.

*   **Accessing Elements:**
    *   **Array:** Direct (random) access using an index (e.g., `arr[5]` gets the 6th element immediately).
    *   **Linked List:** Sequential access. You must traverse the list from the head to reach a specific element.

*   **Insertion/Deletion:**
    *   **Array:** Insertion and deletion can be slow, especially in the middle of the array, because elements might need to be shifted to make space or fill the gap.
    *   **Linked List:** Insertion and deletion are generally faster, especially if you already have a pointer to the node before the insertion/deletion point.  You just need to adjust pointers.

*   **Memory Usage:**
    *   **Array:** Can waste memory if the array is larger than needed. Requires contiguous memory allocation, which may be difficult to find for very large arrays.
    *   **Linked List:** Uses extra memory for the pointers. Memory usage is more flexible as you only allocate memory for the nodes you need.

**Benefits/Drawbacks:**

| Feature        | Array                               | Linked List                         |
|----------------|--------------------------------------|-------------------------------------|
| Access         | Fast (O(1))                          | Slow (O(n))                       |
| Insertion/Deletion | Slow (O(n) in worst/average case)   | Fast (O(1) if node is known)        |
| Memory         | Can be wasteful, fixed size.       | More flexible, overhead for pointers |
| Contiguity     | Contiguous memory blocks            | Non-contiguous memory blocks        |
| Size           | Fixed (needs pre-allocation)        | Dynamic (can grow/shrink)          |
| Cache Friendliness| High (due to contiguity)            | Low (due to non-contiguity)        |

**When to Prefer Which:**

*   **Array:**
    *   When you need frequent random access to elements.
    *   When the size of the data structure is known in advance.
    *   When memory efficiency is critical and the size can be tightly controlled.
    *   When cache performance is important. (Arrays are more cache-friendly due to data contiguity.)

*   **Linked List:**
    *   When you need frequent insertions and deletions, especially in the middle of the list.
    *   When the size of the data structure is not known in advance.
    *   When you need to represent a sequence of elements where the order is important and changes frequently.
    *   When you are concerned about memory fragmentation (linked lists allocate memory in chunks, which can be helpful if contiguous memory blocks are scarce).
    *   Implementing stacks or queues is also very common use case for linked lists.

---

## 2. Reverse Linked List (Iterative, Recursive, In-Place):

**Reverse Linked List (Iterative, Recursive, In-Place):**

  **Iterative Approach:**


```python
    class ListNode:  # Definition of a node for a linked list
        def __init__(self, val=0, next=None):
            self.val = val
            self.next = next

    def reverseList_iterative(head):
        """Reverses a singly linked list iteratively."""
        prev = None
        curr = head
        while curr:
            next_node = curr.next  # Store the next node
            curr.next = prev       # Reverse the pointer
            prev = curr            # Move prev to the current node
            curr = next_node       # Move curr to the next node
        return prev  # prev is the new head
    ```

*   **Recursive Approach:**

    ```python
    def reverseList_recursive(head):
        """Reverses a singly linked list recursively."""
        if not head or not head.next:  # Base case: empty list or single node
            return head

        new_head = reverseList_recursive(head.next) # Recursively reverse the rest of the list

        head.next.next = head  # Make the next node point back to the current node
        head.next = None       # Set the current node's next to None (new tail)

        return new_head
    ```

*   **In-Place:** Both the iterative and recursive solutions can be implemented in-place.

---

## 3. Find Middle Node of Linked List:

**Find Middle Node of Linked List:**

```python
def findMiddleNode(head):
    """Finds the middle node of a singly linked list."""
    slow = head
    fast = head

    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next

    return slow
```

---

## 4. BST (Binary Search Tree):

**BST (Binary Search Tree):**

A Binary Search Tree (BST) is a tree data structure where:

*   Each node has a `key` (or value).
*   The `key` of each node is **greater than** all keys in its left subtree.
*   The `key` of each node is **less than** all keys in its right subtree.
*   The left and right subtrees are also BSTs.

**Example Code (Python - simple insertion):**

```python
class TreeNode:
    def __init__(self, key):
        self.key = key
        self.left = None
        self.right = None

def insert(root, key):
    """Inserts a new node with the given key into the BST."""
    if root is None:
        return TreeNode(key)

    if key < root.key:
        root.left = insert(root.left, key)
    elif key > root.key:
        root.right = insert(root.right, key)

    return root
```

---

## 5. Insertion in the Middle of a Linked List vs. Array. Any benefit over array?

**Insertion in the Middle of a Linked List vs. Array. Any benefit over array?**

*   **Linked List:** The time complexity of insertion *after* finding the correct insertion point is O(1). The cost of finding the middle point in the linked list is O(n).
*   **Array:** The time complexity is O(n) because, on average, you need to shift half of the elements.

Yes, *if you already have a pointer to the insertion point*.  The key is that the actual pointer manipulation is O(1).

---

## 6. Why Can't You Access a Linked List's 5th Index Directly, While You Can in an Array?

**Why Can't You Access a Linked List's 5th Index Directly, While You Can in an Array?**

Arrays store elements in contiguous memory locations, allowing for direct calculation of memory addresses using the index (O(1) access). Linked Lists store elements in nodes that are *not* necessarily contiguous.  To access the 5th element, you must traverse the list from the head (O(n) access).

---

## 7. Searching in Array vs. Linked List

**Searching in Array vs. Linked List**

| Data Structure | Search Algorithm | Time Complexity | Notes                                                                   |
|-----------------|-------------------|------------------|--------------------------------------------------------------------------|
| Unsorted Array  | Linear Search     | O(n)            |                                                                          |
| Sorted Array    | Binary Search     | O(log n)         | Requires the array to be sorted first.                                   |
| Linked List     | Linear Search     | O(n)            | Even if sorted, binary search is not efficient due to sequential access. |

For **unsorted** data, both arrays and linked lists require O(n) time for searching.
For **sorted** data, arrays (with binary search) provide significantly faster searching (O(log n)) compared to linked lists (O(n)).

---

## 8. Stack vs. Queue:

**Stack vs. Queue:**

*   **Differences:**

    *   **Data Access Principle:** This is the core difference.
        *   **Stack:** Last-In, First-Out (LIFO). The last element added to the stack is the first one removed. Think of a stack of plates.
        *   **Queue:** First-In, First-Out (FIFO). The first element added to the queue is the first one removed. Think of a line at a store.

    *   **Operations:**
        *   **Stack:** `push` (add to the top), `pop` (remove from the top), `peek` (view the top element), `isEmpty`.
        *   **Queue:** `enqueue` (add to the rear), `dequeue` (remove from the front), `peek` (view the front element), `isEmpty`.

*   **Implementations:**

    *   **Stack:**
        *   **Array-based:** Simple and efficient if the maximum stack size is known.  Potential for stack overflow if the size is exceeded.
        *   **Linked List-based:** More flexible, dynamic size.
    *   **Queue:**
        *   **Array-based:**  Requires careful handling of the front and rear indices. Can use a circular array to avoid shifting elements.
        *   **Linked List-based:**  Easy to implement, dynamic size.  Nodes are added to the rear and removed from the front.

*   **Real-Life Examples:**

    *   **Stack:**
        *   **Function call stack:**  When a function calls another function, the return address and local variables are pushed onto the stack.
        *   **Undo/Redo functionality:** The "undo" operation typically uses a stack to store previous states.
        *   **Expression evaluation:**  Stacks are used in evaluating arithmetic expressions (e.g., converting infix to postfix).
        *   **Browser History:** The back button functionality in a browser relies on a stack.
    *   **Queue:**
        *   **Print queue:** Jobs are added to the queue and printed in the order they were received.
        *   **Task scheduling:** Operating systems use queues to manage tasks waiting to be executed.
        *   **Message queues:**  Used for asynchronous communication between applications.
        *   **Breadth-First Search (BFS):**  Uses a queue to explore nodes level by level.
        *   **Call centers:** Phone calls are often placed in a queue to be handled by the next available agent.

---

## 9. Tree vs. Graph:

**Tree vs. Graph:**

*   **Differences:**

    *   **Definition:**
        *   **Tree:** A hierarchical data structure consisting of nodes connected by edges, with a root node and no cycles. A tree is a *special type of graph*.
        *   **Graph:** A more general data structure consisting of nodes (vertices) connected by edges. Graphs can have cycles, can be directed or undirected, and may not have a root.

    *   **Cycles:**
        *   **Tree:** No cycles are allowed.
        *   **Graph:** Cycles are allowed.

    *   **Connectivity:**
        *   **Tree:** Must be connected (there is a path from the root to every other node).
        *   **Graph:** Can be connected or disconnected.

    *   **Root Node:**
        *   **Tree:** Has a designated root node.
        *   **Graph:** May or may not have a designated root node.

    *   **Relationships:**
        *   **Tree:** Parent-child relationships (hierarchical).
        *   **Graph:** More general relationships between nodes.

*   **When to Use Each:**

    *   **Tree:**
        *   Representing hierarchical relationships (e.g., file system, organizational chart, DOM structure in a web page).
        *   Efficient searching and sorting (e.g., Binary Search Trees).
        *   Decision-making processes (e.g., decision trees).
    *   **Graph:**
        *   Representing networks (e.g., social networks, transportation networks, communication networks).
        *   Modeling relationships between entities where cycles are possible (e.g., dependencies between tasks in a project).
        *   Pathfinding algorithms (e.g., finding the shortest route between two cities).
        *   Representing maps or mazes.

---

## 10. Sorting Algorithms (General):

**Sorting Algorithms (General):**

*   **Time Complexities:**

    | Algorithm        | Best Case | Average Case | Worst Case | Space Complexity |
    |------------------|-----------|--------------|------------|------------------|
    | Bubble Sort      | O(n)      | O(n<sup>2</sup>)   | O(n<sup>2</sup>) | O(1)              |
    | Selection Sort   | O(n<sup>2</sup>) | O(n<sup>2</sup>)   | O(n<sup>2</sup>) | O(1)              |
    | Insertion Sort   | O(n)      | O(n<sup>2</sup>)   | O(n<sup>2</sup>) | O(1)              |
    | Merge Sort       | O(n log n) | O(n log n)   | O(n log n) | O(n)              |
    | Quick Sort       | O(n log n) | O(n log n)   | O(n<sup>2</sup>) | O(log n) (average), O(n) (worst) |
    | Heap Sort        | O(n log n) | O(n log n)   | O(n log n) | O(1)              |

*   **Which Algorithms are Best Under Different Conditions:**

    *   **Small Datasets:** Insertion sort can be quite efficient for small datasets because its overhead is low.

    *   **Mostly Sorted Data:** Insertion sort also performs well if the data is already mostly sorted.  Bubble sort can be efficient for already sorted lists.

    *   **Large Datasets:**
        *   **Merge Sort:** Guarantees O(n log n) time complexity.  Stable (maintains the relative order of equal elements). Requires extra space.
        *   **Quick Sort:** Typically the fastest in practice (on average), but has a worst-case time complexity of O(n<sup>2</sup>). Can be implemented in-place (with O(log n) space for the call stack).
        *   **Heap Sort:** Guarantees O(n log n) time complexity and is in-place.  Generally a bit slower than Quick Sort in practice.

    *   **Memory Constraints:** If memory is very limited, in-place algorithms like Heap Sort and Quick Sort are preferred.

    *   **Stability:** If maintaining the relative order of equal elements is important, use Merge Sort or Insertion Sort.

---

## 11. Time Complexity:

**Time Complexity:**

*   **General Understanding:** Time complexity is a measure of how the execution time of an algorithm grows as the input size increases. It is typically expressed using Big O notation (e.g., O(n), O(log n), O(n<sup>2</sup>)).

*   **How it Affects Execution:**

    *   **O(1) (Constant Time):** The execution time is independent of the input size.  Very efficient.
    *   **O(log n) (Logarithmic Time):** The execution time grows logarithmically with the input size.  Efficient for large datasets (e.g., binary search).
    *   **O(n) (Linear Time):** The execution time grows linearly with the input size.  For example, searching an unsorted array.
    *   **O(n log n) (Linearithmic Time):**  The execution time grows linearly with the input size, multiplied by a logarithmic factor.  Efficient sorting algorithms (e.g., merge sort, quick sort).
    *   **O(n<sup>2</sup>) (Quadratic Time):** The execution time grows quadratically with the input size.  Less efficient for large datasets (e.g., bubble sort, selection sort).
    *   **O(2<sup>n</sup>) (Exponential Time):**  The execution time grows exponentially with the input size.  Very inefficient for even moderately sized datasets.
    *   **O(n!) (Factorial Time):** The execution time grows extremely rapidly with the input size.  Usually impractical for all but the smallest datasets.

    Choosing algorithms with lower time complexity is crucial for efficient execution, especially when dealing with large amounts of data.

---

## 12. BFS (Breadth-First Search):

**BFS (Breadth-First Search):**

*   **Algorithm:**  BFS explores a graph level by level, starting from a source node. It uses a queue to keep track of the nodes to visit.

*   **Steps:**
    1.  Enqueue the starting node.
    2.  While the queue is not empty:
        *   Dequeue a node from the queue.
        *   Mark the node as visited.
        *   Enqueue all unvisited neighbors of the node.

*   **Use Cases:**
    *   Finding the shortest path in an unweighted graph.
    *   Web crawling.
    *   Social network analysis.
    *   Finding all reachable nodes from a given source.

---

## 13. DFS (Depth-First Search):

**DFS (Depth-First Search):**

*   **Algorithm:** DFS explores a graph by going as deep as possible along each branch before backtracking. It can be implemented recursively or iteratively (using a stack).

*   **Steps (Recursive):**
    1.  Mark the current node as visited.
    2.  For each unvisited neighbor of the current node:
        *   Recursively call DFS on the neighbor.

*   **Steps (Iterative):**
    1.  Push the starting node onto the stack.
    2.  While the stack is not empty:
        *   Pop a node from the stack.
        *   If the node is not visited:
            *   Mark the node as visited.
            *   Push all unvisited neighbors of the node onto the stack.

*   **Use Cases:**
    *   Finding a path between two nodes.
    *   Detecting cycles in a graph.
    *   Topological sorting.
    *   Solving mazes.

---

## 14. Implement Queue using Stack:

**Implement Queue using Stack:**

This can be done using two stacks: `enqueueStack` and `dequeueStack`.

*   **Enqueue (add to the rear):** Simply push the new element onto `enqueueStack`.

*   **Dequeue (remove from the front):**
    1.  If `dequeueStack` is empty, transfer all elements from `enqueueStack` to `dequeueStack` (reversing their order).  This effectively puts the "oldest" element at the top of `dequeueStack`.
    2.  Pop the top element from `dequeueStack`.

*   **Time Complexity:**
    *   Enqueue: O(1)
    *   Dequeue: O(1) amortized (on average).  The transfer from `enqueueStack` to `dequeueStack` takes O(n) time, but it only happens when `dequeueStack` is empty, so the cost is amortized over multiple dequeue operations.

---

## 15. Linked List (Singly) Delete a Node: (Given access to the node itself, but not the head)

**Linked List (Singly) Delete a Node: (Given access to the node itself, but not the head)**

This is a tricky problem that highlights the limitations of singly linked lists. The standard deletion approach won't work because you can't get to the *previous* node to update its `next` pointer.

*   **The Solution:**
    1.  **Copy the data from the next node into the current node.**
    2.  **Delete the next node.**

    ```python
    class ListNode:  # Assuming this is how your ListNode is defined
        def __init__(self, val=0, next=None):
            self.val = val
            self.next = next

    def deleteNode(node):
        """Deletes a node in a singly linked list given access to the node itself (not the head)."""
        if node is None or node.next is None:  # Cannot delete the last node.
            raise ValueError("Cannot delete the last node or a null node.")

        node.val = node.next.val  # Copy the value from the next node
        node.next = node.next.next  # Skip the next node (effectively deleting it)
    ```

*   **Important Limitation:** This solution *does not work* if the node to be deleted is the *last* node in the linked list because there is no "next" node to copy from. You could set the node value to null, but the node itself remains in memory and the next pointer will point to nothing. The only way to truly delete that last node is with access to the head and traversing through the list. The problem statement assumes that this is not the case

---

## 16. Binary Tree Traversal (Inorder, Preorder, Postorder):

**Binary Tree Traversal (Inorder, Preorder, Postorder):**

These are ways to visit all the nodes in a binary tree.

*   **Inorder Traversal:** (Left, Root, Right)
    1.  Traverse the left subtree.
    2.  Visit the root node.
    3.  Traverse the right subtree.
    *   Result: Nodes are visited in sorted order (for a Binary Search Tree).

*   **Preorder Traversal:** (Root, Left, Right)
    1.  Visit the root node.
    2.  Traverse the left subtree.
    3.  Traverse the right subtree.
    *   Result: Useful for creating a copy of the tree.

*   **Postorder Traversal:** (Left, Right, Root)
    1.  Traverse the left subtree.
    2.  Traverse the right subtree.
    3.  Visit the root node.
    *   Result: Useful for deleting a tree.
*   **Example Code:**

    ```python
    class TreeNode: #Defining a Node
      def __init__(self, val=0, left=None, right=None):
          self.val = val
          self.left = left
          self.right = right

    def inorderTraversal(root):
      result = []
      if root:
        result = inorderTraversal(root.left)
        result.append(root.val)
        result = result + inorderTraversal(root.right)
      return result

    def preorderTraversal(root):
        result = []
        if root:
            result.append(root.val)
            result = result + preorderTraversal(root.left)
            result = result + preorderTraversal(root.right)
        return result

    def postorderTraversal(root):
        result = []
        if root:
            result =  postorderTraversal(root.left)
            result = result + postorderTraversal(root.right)
            result.append(root.val)
        return result
    ```

    **Explanation**
    The basic is to recursively call the function again using the .left or .right based on the pattern

---

## 17. Mirror the BST

**Mirror the BST**

Mirroring a BST involves swapping the left and right children of each node in the tree. This effectively creates a mirror image of the original tree.

*   **Algorithm:**

    1.  If the current node is `None`, return.
    2.  Recursively mirror the left subtree.
    3.  Recursively mirror the right subtree.
    4.  Swap the left and right children of the current node.

*   **Python Code:**

    ```python
    class TreeNode:
        def __init__(self, val=0, left=None, right=None):
            self.val = val
            self.left = left
            self.right = right

    def mirrorBST(root):
        """Mirrors a Binary Search Tree."""
        if root is None:
            return

        mirrorBST(root.left)  # Recursively mirror the left subtree
        mirrorBST(root.right) # Recursively mirror the right subtree

        # Swap the left and right children
        root.left, root.right = root.right, root.left
    ```

**Explanation**

The code first checks if the root is `None`, if it is that means the node doesn't exist and there nothing to do.

After that it recursively calls `mirrorBST` on the `root.left` and `root.right`. Calling those functions allows the function to recursively keep going until it reaches the last node.

When all the recursive calls are over, the last line switches the left and right nodes.

---

## 18. Sorting Algorithms: Quick Sort:

**Sorting Algorithms: Quick Sort:**

*   **How it Works:** Quick Sort is a divide-and-conquer sorting algorithm.

    1.  **Partitioning:** Select a "pivot" element from the array. Reorder the array so that all elements less than the pivot are placed before it, and all elements greater than the pivot are placed after it.  The pivot is now in its final sorted position.
    2.  **Recursion:** Recursively apply the partitioning step to the sub-arrays to the left and right of the pivot.
    3.  **Base Case:** The recursion stops when the sub-array has only one element (or is empty), as a single-element array is already sorted.

*   **Pivot Selection:**  The choice of pivot is crucial. Common strategies include:
    *   Choosing the first element.
    *   Choosing the last element.
    *   Choosing a random element.
    *   Using the "median of three" approach (selecting the median of the first, middle, and last elements).

*   **Time Complexity:**
    *   **Best Case:** O(n log n) - Occurs when the pivot consistently divides the array into roughly equal halves.
    *   **Average Case:** O(n log n) - In practice, with a good pivot selection strategy, the average case is quite close to the best case.
    *   **Worst Case:** O(n<sup>2</sup>) - Occurs when the pivot consistently results in highly unbalanced partitions (e.g., when the pivot is always the smallest or largest element).

*   **Space Complexity:** O(log n) on average (due to the recursive call stack). In the worst case, it can be O(n).

---

## 19. Sorting Algorithms: Merge Sort:

**Sorting Algorithms: Merge Sort:**

*   **How it Works:** Merge Sort is another divide-and-conquer sorting algorithm.

    1.  **Divide:** Divide the unsorted list into n sublists, each containing one element (a list of one element is considered sorted).
    2.  **Conquer:** Repeatedly merge sublists to produce new sorted sublists until there is only one sublist remaining. This will be the sorted list.

*   **Merging:** The merging step involves taking two sorted sublists and creating a new sorted list by comparing elements from both sublists and placing them in the correct order.

*   **Time Complexity:** O(n log n) in all cases (best, average, and worst).

*   **Space Complexity:** O(n) (requires extra space for the merging step).

*   **Stability:** Merge Sort is a stable sorting algorithm, meaning that it preserves the relative order of equal elements.

---

## 20. Find Missing Number in Sorted/Unsorted Array:

**Find Missing Number in Sorted/Unsorted Array:**

*   **Sorted Array (with numbers 1 to n, one missing):**

    *   **Summation Method:** Calculate the expected sum of numbers from 1 to n (using the formula n*(n+1)/2). Calculate the actual sum of the elements in the array. The difference between the expected sum and the actual sum is the missing number. Time Complexity: O(n).
    *   **Binary Search:** If the array is sorted and you know the range of numbers (e.g., 1 to n), you can use a modified binary search. Check if `arr[mid] == mid + starting_number`.  If it's not, the missing number is in the left half. Otherwise, it's in the right half.  Time complexity: O(log n).
    *   **XOR Method:** Take XOR of all elements of array with index+1 and store result in a variable, print the variable.

*   **Unsorted Array (with numbers 1 to n, one missing):**

    *   **Hash Set:** Add all elements of the array to a hash set. Iterate from 1 to n. If a number is not found in the hash set, it is the missing number. Time Complexity: O(n), Space Complexity: O(n).
    *   **Summation Method:** Same as the sorted array approach.  Time Complexity: O(n).
    *   **XOR Method:** Works for unsorted arrays as well. Time Complexity: O(n).

---

## 21. Linked List: Detect and Remove Cycle:

**Linked List: Detect and Remove Cycle:**

*   **Floyd's Cycle-Finding Algorithm (Tortoise and Hare):** This is the standard approach.

    1.  **Detection:** Use two pointers, a slow pointer (tortoise) and a fast pointer (hare). The slow pointer moves one node at a time, and the fast pointer moves two nodes at a time. If there is a cycle, the two pointers will eventually meet.
    2.  **Removal:** Once the pointers meet, move the slow pointer back to the head of the list. Keep the fast pointer at the meeting point. Move both pointers one node at a time. The point where they meet again is the start of the cycle.  To remove the cycle, set the `next` pointer of the node before the start of the cycle to `None`.

    ```python
    class ListNode:  # Assuming you have this class
        def __init__(self, val=0, next=None):
            self.val = val
            self.next = next

    def detectAndRemoveCycle(head):
        slow = head
        fast = head

        # Detect Cycle
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                break  # Cycle detected

        if fast is None or fast.next is None: # No cycle exists
          return

        # Find the start of the cycle
        slow = head
        while slow != fast:
            slow = slow.next
            fast = fast.next

        # Remove Cycle
        temp = slow
        while temp.next!= slow:
          temp= temp.next

        temp.next = None
        return
    ```

---

## 22. Find Distance Between Two Nodes in BST:

**Find Distance Between Two Nodes in BST:**

1.  **Find the Lowest Common Ancestor (LCA):**  First, find the LCA of the two nodes. The LCA is the deepest node that is an ancestor of both nodes.
2.  **Calculate Distances from LCA:** Calculate the distance from the LCA to each of the two nodes.
3.  **Sum the Distances:** The distance between the two nodes is the sum of the distances from the LCA to each node.

```python
class TreeNode: #Assuming you have this class
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def findLCA(root, n1, n2):
    """Finds the lowest common ancestor of two nodes in a BST."""
    if root is None:
        return None

    if root.val > n1 and root.val > n2:
        return findLCA(root.left, n1, n2)

    if root.val < n1 and root.val < n2:
        return findLCA(root.right, n1, n2)

    return root  # Root is the LCA

def findLevel(root, a, level):
  if root is None:
    return 0
  if root.val == a:
    return level
  downlevel = findLevel(root.left, a, level+1)
  if downlevel!=0:
    return downlevel
  downlevel = findLevel(root.right, a, level+1)
  return downlevel

def findDist(root, a, b):
  lca = findLCA(root,a,b)
  d1 = findLevel(lca,a,0)
  d2 = findLevel(lca,b,0)
  return d1+d2
```

---

## 23. Linked List: Delete the Last Occurrence of an Element:

**Linked List: Delete the Last Occurrence of an Element:**

```python
class ListNode: #Assuming you have this class
    def __init__(self, val=0, next=None):
        
Okay, continuing from the previous response, formatted for Obsidian:

```markdown
self.val = val
        self.next = next

def deleteLastOccurrence(head, key):
    """Deletes the last occurrence of a key in a singly linked list."""
    lastOccurrence = None  # Stores the last occurrence of the key
    prevOfLast = None  # Stores the node *before* the last occurrence
    curr = head
    prev = None

    # Find the last occurrence and the node before it
    while curr:
        if curr.val == key:
            lastOccurrence = curr
            prevOfLast = prev
        prev = curr
        curr = curr.next

    # If the key was not found, do nothing
    if lastOccurrence is None:
        return head

    # If the last occurrence is the head node
    if lastOccurrence == head:
        head = head.next
    else:
        prevOfLast.next = lastOccurrence.next  # Remove the last occurrence

    return head
```

---

## 24. Linked List: Reverse Linked List in k Chunks:

**Linked List: Reverse Linked List in k Chunks:**

```python
class ListNode: #Assuming you have this class
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def reverseKGroup(head, k):
    """Reverses a linked list in chunks of k nodes."""
    curr = head
    count = 0

    # Check if there are at least k nodes remaining
    while curr and count < k:
        curr = curr.next
        count += 1

    if count < k:  # Less than k nodes remaining, don't reverse
        return head

    curr = head
    prev = None
    next_node = None
    count = 0

    # Reverse the current k-node group
    while curr and count < k:
        next_node = curr.next
        curr.next = prev
        prev = curr
        curr = next_node
        count += 1

    # Recursively reverse the remaining list
    if next_node:
        head.next = reverseKGroup(next_node, k)

    return prev  # prev is the new head of the reversed k-node group
```

---

## 25. Given a binary tree every node of tree should have a property of siblings which will point to its next child:

**Given a binary tree every node of tree should have a property of siblings which will point to its next child:**

This effectively wants a transformation of a tree into a level-order linked list. You can accomplish this using a Breadth-First Search (BFS) approach. The "sibling" pointer refers to the next node on the same level.

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None, next=None):
        self.val = val
        self.left = left
        self.right = right
        self.next = next  # Sibling pointer

def connectSiblings(root):
    """Connects siblings in a binary tree (level-order)."""
    if not root:
        return root

    queue = [root]

    while queue:
        level_size = len(queue)
        prev = None  # Keep track of the previous node on the current level

        for _ in range(level_size):
            node = queue.pop(0)

            if prev:
                prev.next = node  # Set the sibling pointer

            prev = node  # Update prev to the current node

            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
    return root
```

---

## 26. Flatten Array: Multi-dimensional to Single-dimensional:

**Flatten Array: Multi-dimensional to Single-dimensional:**

```python
def flattenArray(arr):
    """Flattens a multi-dimensional array into a single-dimensional array."""
    result = []
    for item in arr:
        if isinstance(item, list):
            result.extend(flattenArray(item))  # Recursive call for sublists
        else:
            result.append(item)
    return result
```

---

## 27. Stack: Real-Life Example:

**Stack: Real-Life Example:**

*   **Function Call Stack:** When a function calls another function, the return address and local variables are pushed onto the stack. This enables the program to return to the correct location after the called function completes.

---

## 28. Heap vs Stack:

**Heap vs Stack:**

| Feature           | Heap                                | Stack                                    |
|--------------------|--------------------------------------|------------------------------------------|
| Memory Allocation | Dynamic (during runtime)           | Static (compile-time)                      |
| Size              | Can grow dynamically               | Fixed size (determined at compile time)   |
| Access            | Slower                               | Faster                                     |
| Management        | Explicit (using `new`/`malloc`, and `delete`/`free` in languages like C++) or implicit via garbage collection (in languages like Java, Python)    | Automatic (managed by the compiler/runtime) |
| Lifespan          | Variables can persist longer than the function call that created them. | Variables are automatically deallocated when the function returns. |
| Usage             | Storing objects, dynamically sized data structures.   | Storing local variables, function call information (return addresses, parameters).|
| Fragmentation     | More prone to fragmentation          | Less prone to fragmentation               |

---

## 29. Heap Memory Allocation vs Stack Memory Allocation:

**Heap Memory Allocation vs Stack Memory Allocation:**

See table in Q28

---

## 30. Find 2nd Highest Element of Array:

**Find 2nd Highest Element of Array:**

```python
def findSecondHighest(arr):
    """Finds the second highest element in an array."""
    if len(arr) < 2:
        return None  # Not enough elements

    highest = float('-inf')
    second_highest = float('-inf')

    for num in arr:
        if num > highest:
            second_highest = highest
            highest = num
        elif num > second_highest and num != highest:
            second_highest = num

    if second_highest == float('-inf'):
        return None  # All elements are the same

    return second_highest
```

---

## 31. Linked List: Reverse Linked List Using Recursion:

**Linked List: Reverse Linked List Using Recursion:**
(This is a repeat - the solution is in a previous answer. See Question 2)

---

## 32. Convert Array to BST:

**Convert Array to BST:**

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def sortedArrayToBST(nums):
    """Converts a sorted array to a balanced BST."""
    if not nums:
        return None

    mid = len(nums) // 2
    root = TreeNode(nums[mid])  # Middle element becomes the root
    root.left = sortedArrayToBST(nums[:mid])  # Left subtree from the left half
    root.right = sortedArrayToBST(nums[mid + 1:])  # Right subtree from the right half

    return root
```
```
