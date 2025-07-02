# 206. Reverse Linked List - Explanation

[Problem Link](https://neetcode.io/problems/reverse-a-linked-list/)

## Description

Given the beginning of a singly linked list `head`, reverse the list, and return the new beginning of the list.

**Example 1:**

```java
Input: head = [0,1,2,3]

Output: [3,2,1,0]
```

Copy

**Example 2:**

```java
Input: head = []

Output: []
```

Copy

**Constraints:**

- `0 <= The length of the list <= 1000`.
- `-1000 <= Node.val <= 1000`

  
  
Recommended Time & Space Complexity

  
Hint 1

  
Hint 2

  
Hint 3

  
Hint 4

  

  

## 1. Recursion

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if not head:
            return None

        newHead = head
        if head.next:
            newHead = self.reverseList(head.next)
            head.next.next = head
        head.next = None
        
        return newHead
```

Copy

### Time & Space Complexity

- Time complexity: O(n)O(n)
- Space complexity: O(n)O(n)

---

## 2. Iteration



```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        prev, curr = None, head

        while curr:
            temp = curr.next
            curr.next = prev
            prev = curr
            curr = temp
        return prev
```

Copy

### Time & Space Complexity

- Time complexity: O(n)O(n)
- Space complexity: O(1)O(1)

# 242. Valid Anagram - Explanation

[Problem Link](https://neetcode.io/problems/is-anagram/)

## Description

Given two strings `s` and `t`, return `true` if the two strings are anagrams of each other, otherwise return `false`.

An **anagram** is a string that contains the exact same characters as another string, but the order of the characters can be different.

**Example 1:**

```java
Input: s = "racecar", t = "carrace"

Output: true
```

Copy

**Example 2:**

```java
Input: s = "jar", t = "jam"

Output: false
```

Copy

**Constraints:**

- `s` and `t` consist of lowercase English letters.

  
  
Recommended Time & Space Complexity

  
Hint 1

  
Hint 2

  
Hint 3

  

  

## 1. Sorting



```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False
            
        return sorted(s) == sorted(t)
```

Copy

### Time & Space Complexity

- Time complexity: O(nlog⁡n+mlog⁡m)O(nlogn+mlogm)
- Space complexity: O(1)O(1) or O(n+m)O(n+m) depending on the sorting algorithm.

> Where nn is the length of string ss and mm is the length of string tt.

---

## 2. Hash Map



```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False

        countS, countT = {}, {}

        for i in range(len(s)):
            countS[s[i]] = 1 + countS.get(s[i], 0)
            countT[t[i]] = 1 + countT.get(t[i], 0)
        return countS == countT
```

Copy

### Time & Space Complexity

- Time complexity: O(n+m)O(n+m)
- Space complexity: O(1)O(1) since we have at most 2626 different characters.

> Where nn is the length of string ss and mm is the length of string tt.
# 215. Kth Largest Element In An Array - Explanation

[Problem Link](https://neetcode.io/problems/kth-largest-element-in-an-array/)

## Description

Given an unsorted array of integers `nums` and an integer `k`, return the `kth` largest element in the array.

By `kth` largest element, we mean the `kth` largest element in the sorted order, not the `kth` distinct element.

Follow-up: Can you solve it without sorting?

**Example 1:**

```java
Input: nums = [2,3,1,5,4], k = 2

Output: 4
```

Copy

**Example 2:**

```java
Input: nums = [2,3,1,1,5,5,4], k = 3

Output: 4
```

Copy

**Constraints:**

- `1 <= k <= nums.length <= 10000`
- `-1000 <= nums[i] <= 1000`

## 1. Sorting



```python
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        nums.sort()
        return nums[len(nums) - k]
```

Copy

### Time & Space Complexity

- Time complexity: O(nlog⁡n)O(nlogn)
- Space complexity: O(1)O(1) or O(n)O(n) depending on the sorting algorithm.

---
## 3. Quick Select



```python
class Solution:

    def findKthLargest(self, nums: List[int], k: int) -> int:
        k = len(nums) - k
        
        def quickSelect(l, r):
            pivot, p = nums[r], l
            for i in range(l, r):
                if nums[i] <= pivot:
                    nums[p], nums[i] = nums[i], nums[p]
                    p += 1
            nums[p], nums[r] = nums[r], nums[p]

            if p > k: 
                return quickSelect(l, p - 1)
            elif p < k:
                return quickSelect(p + 1, r)
            else:
                return nums[p]

        return quickSelect(0, len(nums) - 1)
```

Copy

### Time & Space Complexity

- Time complexity: O(n)O(n) in average case, O(n2)O(n2) in worst case.
- Space complexity: O(n)O(n)
# 876. Middle of the Linked List - Explanation

[Problem Link](https://leetcode.com/problems/middle-of-the-linked-list/)
## 3. Fast & Slow Pointers

- Python
- Java
- C++
- JavaScript

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def middleNode(self, head: Optional[ListNode]) -> Optional[ListNode]:
        slow, fast = head, head

        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        return slow
```

### Time & Space Complexity

- Time complexity: O(n)O(n)
- Space complexity: O(1)O(1) extra space.

# 2095. Delete the Middle Node of a Linked List

Medium[Linked List](https://algo.monster/problems/linked_list_cycle)[Two Pointers](https://algo.monster/problems/two_pointers_intro)

[Leetcode Link](https://leetcode.com/problems/delete-the-middle-node-of-a-linked-list)

## Problem Description

You have a singly [linked list](https://algo.monster/problems/linked_list_cycle). Your task is to remove the node that is in the middle of this list and return the head of the updated list. The middle node is defined as the `&lfloor;n / 2&rfloor;<sup>th</sup>` node from the beginning of the list, where `n` is the total number of nodes in the list and `&lfloor;x&rfloor;` signifies the greatest integer less than or equal to `x`. This means that you're not counting from 1, but from 0. So, in a linked list with:

- 1 node, the middle is the 0th node.
- 2 nodes, the middle is the 1st node.
- 3 nodes, the middle is the 1st node.
- 4 nodes, the middle is the 2nd node.
- 5 nodes, the middle is the 2nd node.

Your goal is to efficiently find and remove this middle node and ensure the integrity of the [linked list](https://algo.monster/problems/linked_list_cycle) is maintained after the removal.

## Intuition

To solve this problem, the two-pointer technique is a perfect fit. The idea is to have [two pointers](https://algo.monster/problems/two_pointers_intro), `slow` and `fast`. The `slow` pointer moves one step at a time, while the `fast` pointer moves two steps at a time. By the time the `fast` pointer reaches the end of the list, the `slow` pointer will be at the middle node.

Here's the step-by-step intuition:

1. Initialize a `dummy` node that points to the head. This dummy node will help us easily handle edge cases like when there's only one node in the list.
2. Start both `slow` and `fast` pointers. The `slow` pointer will start from the dummy node, while `fast` will start from the head node of the list.
3. Move `slow` one step and `fast` two steps until `fast` reaches the end of the list or has no next node (this is for cases when the number of nodes is even).
4. When the `fast` pointer reaches the end of the list, the `slow` pointer will be on the node just before the middle node (since it started from `dummy`, which is before the `head`).
5. Adjust the `next` pointer of the `slow` node so that it skips over the middle node, effectively removing it from the list.
6. Return the new head of the list, which is pointed to by `dummy.next`, since the `dummy` node was added before the original head.

By utilizing this approach, we can identify and remove the middle node in a singly [linked list](https://algo.monster/problems/linked_list_cycle) in a single pass and O(n) time complexity, where n is the number of nodes in the list.

**Learn more about [Linked List](https://algo.monster/problems/linked_list_cycle) and [Two Pointers](https://algo.monster/problems/two_pointers_intro) patterns.**

## Solution Implementation

- Python
- Java
- C++
- TypeScript

```python
1# Definition for singly-linked list.
2class ListNode:
3    def __init__(self, val=0, next=None):
4        self.val = val
5        self.next = next
6
7class Solution:
8    def deleteMiddle(self, head: Optional[ListNode]) -> Optional[ListNode]:
9        # Create a dummy node that points to the head of the list, to handle edge cases smoothly
10        dummy_node = ListNode(next=head)
11      
12        # Initialize two pointers, slow will move one step at a time, fast will move two steps at a time
13        slow_pointer, fast_pointer = dummy_node, head
14      
15        # Iterate through the list to find the middle
16        while fast_pointer and fast_pointer.next:
17            slow_pointer = slow_pointer.next  # Move slow pointer one step
18            fast_pointer = fast_pointer.next.next  # Move fast pointer two steps
19      
20        # Now, slow_pointer is at the node just before the middle one. Delete the middle node
21        slow_pointer.next = slow_pointer.next.next
22      
23        # Return the head of the updated list, by skipping over the dummy node
24        return dummy_node.next
25
```
# 1929. Concatenation of Array - Explanation

[Problem Link](https://neetcode.io/problems/concatenation-of-array/)

## Description

You are given an integer array `nums` of length `n`. Create an array `ans` of length `2n` where `ans[i] == nums[i]` and `ans[i + n] == nums[i]` for `0 <= i < n` **(0-indexed)**.

Specifically, `ans` is the concatenation of two `nums` arrays.

Return the array `ans`.

**Example 1:**

```java
Input: nums = [1,4,1,2]

Output: [1,4,1,2,1,4,1,2]
```

Copy

**Example 2:**

```java
Input: nums = [22,21,20,1]

Output: [22,21,20,1,22,21,20,1]
```

Copy

**Constraints:**

- `1 <= nums.length <= 1000`.
- `1 <= nums[i] <= 1000`

  
## 1. Iteration (Two Pass)


```python
class Solution:
    def getConcatenation(self, nums: List[int]) -> List[int]:
        ans = []
        for i in range(2):
            for num in nums:
                ans.append(num)
        return ans
```

Copy

### Time & Space Complexity

- Time complexity: O(n)O(n)
- Space complexity: O(n)O(n) for the output array.


# 217. Contains Duplicate - Explanation

[Problem Link](https://neetcode.io/problems/duplicate-integer/)

## Description

Given an integer array `nums`, return `true` if any value appears **more than once** in the array, otherwise return `false`.

**Example 1:**

```java
Input: nums = [1, 2, 3, 3]

Output: true
```

Copy

  

**Example 2:**

```java
Input: nums = [1, 2, 3, 4]

Output: false
```

## 3. Hash Set

- Python
- Java
- C++
- JavaScript
- C#
- Go
- Kotlin
- Swift

```python
class Solution:
    def hasDuplicate(self, nums: List[int]) -> bool:
        seen = set()
        for num in nums:
            if num in seen:
                return True
            seen.add(num)
        return False
```

Copy

### Time & Space Complexity

- Time complexity: O(n)O(n)
- Space complexity: O(n)O(n)
  
# 242. Valid Anagram - Explanation

[Problem Link](https://neetcode.io/problems/is-anagram/)

## Description

Given two strings `s` and `t`, return `true` if the two strings are anagrams of each other, otherwise return `false`.

An **anagram** is a string that contains the exact same characters as another string, but the order of the characters can be different.

**Example 1:**

```java
Input: s = "racecar", t = "carrace"

Output: true
```

Copy

**Example 2:**

```java
Input: s = "jar", t = "jam"

Output: false
```

Copy

**Constraints:**

- `s` and `t` consist of lowercase English letters.

## 2. Hash Map

- Python
- Java
- C++
- JavaScript
- C#
- Go
- Kotlin
- Swift

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False

        countS, countT = {}, {}

        for i in range(len(s)):
            countS[s[i]] = 1 + countS.get(s[i], 0)
            countT[t[i]] = 1 + countT.get(t[i], 0)
        return countS == countT
```

Copy

### Time & Space Complexity

- Time complexity: O(n+m)O(n+m)
- Space complexity: O(1)O(1) since we have at most 2626 different characters.
# 1. Two Sum - Explanation

[Problem Link](https://neetcode.io/problems/two-integer-sum/)

## Description

Given an array of integers `nums` and an integer `target`, return the indices `i` and `j` such that `nums[i] + nums[j] == target` and `i != j`.

You may assume that _every_ input has exactly one pair of indices `i` and `j` that satisfy the condition.

Return the answer with the smaller index first.

**Example 1:**

```java
Input: 
nums = [3,4,5,6], target = 7

Output: [0,1]
```

Copy

Explanation: `nums[0] + nums[1] == 7`, so we return `[0, 1]`.

**Example 2:**

```java
Input: nums = [4,5,6], target = 10

Output: [0,2]
```

Copy

**Example 3:**

```java
Input: nums = [5,5], target = 10

Output: [0,1]
```

Copy

**Constraints:**

- `2 <= nums.length <= 1000`
- `-10,000,000 <= nums[i] <= 10,000,000`
- `-10,000,000 <= target <= 10,000,000`

## 4. Hash Map (One Pass)

- Python
- Java
- C++
- JavaScript
- C#
- Go
- Kotlin
- Swift

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        prevMap = {}  # val -> index

        for i, n in enumerate(nums):
            diff = target - n
            if diff in prevMap:
                return [prevMap[diff], i]
            prevMap[n] = i
```

Copy

### Time & Space Complexity

- Time complexity: O(n)O(n)
- Space complexity: O(n)O(n)

# 27. Remove Element - Explanation

[Problem Link](https://neetcode.io/problems/remove-element/)

## Description

You are given an integer array `nums` and an integer `val`. Your task is to remove **all occurrences** of `val` from `nums` **in-place**.

After removing all occurrences of `val`, return the number of remaining elements, say `k`, such that the **first `k` elements** of `nums` do **not contain `val`**.

Note:

- The order of the elements which are not equal to `val` **does not matter**.
- It is not necessary to consider elements beyond the first `k` positions of the array.
- To be accepted, the first `k` elements of `nums` must contain only elements **not equal** to `val`.

Return `k` as the final result.

**Example 1:**

```java
Input: nums = [1,1,2,3,4], val = 1

Output: [2,3,4]
```

Copy

Explanation: You should return `k = 3` as we have `3` elements which are not equal to `val = 1`.

**Example 2:**

```java
Input: nums = [0,1,2,2,3,0,4,2], val = 2

Output: [0,1,3,0,4]
```

Copy

Explanation: You should return `k = 5` as we have `5` elements which are not equal to `val = 2`.

**Constraints:**

- `0 <= nums.length <= 100`
- `0 <= nums[i] <= 50`
- `0 <= val <= 100`

## 2. Two Pointers - I

- Python
- Java
- C++
- JavaScript
- C#

```python
class Solution:
    def removeElement(self, nums: list[int], val: int) -> int:
        k = 0
        for i in range(len(nums)):
            if nums[i] != val:
                nums[k] = nums[i]
                k += 1
        return k
```

Copy

### Time & Space Complexity

- Time complexity: O(n)O(n)
- Space complexity: O(1)O(1)

# 169. Majority Element - Explanation

[Problem Link](https://neetcode.io/problems/majority-element/)

## Description

Given an array `nums` of size `n`, return the **majority element**.

The majority element is the element that appears more than `⌊n / 2⌋` times in the array. You may assume that the majority element always exists in the array.

**Example 1:**

```java
Input: nums = [5,5,1,1,1,5,5]

Output: 5
```

Copy

**Example 2:**

```java
Input: nums = [2,2,2]

Output: 2
```

Copy

**Constraints:**

- `1 <= nums.length <= 50,000`
- `-1,000,000,000 <= nums[i] <= 1,000,000,000`

**Follow-up:** Could you solve the problem in linear time and in `O(1)` space?
## 5. Boyer-Moore Voting Algorithm

- Python
- Java
- C++
- JavaScript
- C#

```python
class Solution:
    def majorityElement(self, nums):
        res = count = 0

        for num in nums:
            if count == 0:
                res = num
            count += (1 if num == res else -1)
        return res
```

Copy

### Time & Space Complexity

- Time complexity: O(n)O(n)
- Space complexity: O(1)O(1)

# 912. Sort an Array - Explanation

[Problem Link](https://neetcode.io/problems/sort-an-array/)

## Description

You are given an array of integers `nums`, sort the array in ascending order and return it.

You must solve the problem **without using any built-in functions** in `O(nlog(n))` time complexity and with the smallest space complexity possible.

**Example 1:**

```java
Input: nums = [10,9,1,1,1,2,3,1]

Output: [1,1,1,1,2,3,9,10]
```

Copy

**Example 2:**

```java
Input: nums = [5,10,2,1,3]

Output: [1,2,3,5,10]
```

Copy

**Constraints:**

- `1 <= nums.length <= 50,000`.
- `-50,000 <= nums[i] <= 50,000`

## 2. Merge Sort

- Python
- Java
- C++
- JavaScript
- C#

```python
class Solution:
    def sortArray(self, nums: List[int]) -> List[int]:
        def merge(arr, L, M, R):
            left, right = arr[L : M + 1], arr[M + 1 : R + 1]
            i, j, k = L, 0, 0

            while j < len(left) and k < len(right):
                if left[j] <= right[k]:
                    arr[i] = left[j]
                    j += 1
                else:
                    arr[i] = right[k]
                    k += 1
                i += 1
            while j < len(left):
                nums[i] = left[j]
                j += 1
                i += 1
            while k < len(right):
                nums[i] = right[k]
                k += 1
                i += 1
        
        def mergeSort(arr, l, r):
            if l == r:
                return

            m = (l + r) // 2
            mergeSort(arr, l, m)
            mergeSort(arr, m + 1, r)
            merge(arr, l, m, r)
            return
        
        mergeSort(nums, 0, len(nums))
        return nums
```

### Time & Space Complexity

- Time complexity: O(nlog⁡n)O(nlogn)
- Space complexity: O(n)O(n)
# 238. Product of Array Except Self - Explanation

[Problem Link](https://neetcode.io/problems/products-of-array-discluding-self/)

## Description

Given an integer array `nums`, return an array `output` where `output[i]` is the product of all the elements of `nums` except `nums[i]`.

Each product is **guaranteed** to fit in a **32-bit** integer.

Follow-up: Could you solve it in O(n)O(n) time without using the division operation?

**Example 1:**

```java
Input: nums = [1,2,4,6]

Output: [48,24,12,8]
```

Copy

**Example 2:**

```java
Input: nums = [-1,0,1,2,3]

Output: [0,-6,0,0,0]
```

Copy

**Constraints:**

- `2 <= nums.length <= 1000`
- `-20 <= nums[i] <= 20`

## 4. Prefix & Suffix (Optimal)

- Python
- Java
- C++
- JavaScript
- C#
- Go
- Kotlin
- Swift

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        res = [1] * (len(nums))

        prefix = 1
        for i in range(len(nums)):
            res[i] = prefix
            prefix *= nums[i]
        postfix = 1
        for i in range(len(nums) - 1, -1, -1):
            res[i] *= postfix
            postfix *= nums[i]
        return res
```

Copy

### Time & Space Complexity

- Time complexity: O(n)O(n)
- Space complexity:
    - O(1)O(1) extra space.
    - O(n)O(n) space for the output array.

# 344. Reverse String - Explanation

[Problem Link](https://neetcode.io/problems/reverse-string/)

## Description

You are given an array of characters which represents a string `s`. Write a function which reverses a string.

You must do this by modifying the input array in-place with `O(1)` extra memory.

**Example 1:**

```java
Input: s = ["n","e","e","t"]

Output: ["t","e","e","n"]
```

Copy

**Example 2:**

```java
Input: s = ["r","a","c","e","c","a","r"]

Output: ["r","a","c","e","c","a","r"]
```

Copy

**Constraints:**

- `0 <= s.length < 100,000`
- `s[i]` is a [printable ascii character](https://en.wikipedia.org/wiki/ASCII#Printable_characters).

## 5. Two Pointers

- Python
- Java
- C++
- JavaScript
- C#

```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        l, r = 0, len(s) - 1
        while l < r:
            s[l], s[r] = s[r], s[l]
            l += 1
            r -= 1
```

Copy

### Time & Space Complexity

- Time complexity: O(n)O(n)
- Space complexity: O(1)O(1)
# 125. Valid Palindrome - Explanation

[Problem Link](https://neetcode.io/problems/is-palindrome/)

## Description

Given a string `s`, return `true` if it is a **palindrome**, otherwise return `false`.

A **palindrome** is a string that reads the same forward and backward. It is also case-insensitive and ignores all non-alphanumeric characters.

**Note:** Alphanumeric characters consist of letters `(A-Z, a-z)` and numbers `(0-9)`.

**Example 1:**

```java
Input: s = "Was it a car or a cat I saw?"

Output: true
```

Copy

Explanation: After considering only alphanumerical characters we have "wasitacaroracatisaw", which is a palindrome.

**Example 2:**

```java
Input: s = "tab a cat"

Output: false
```

Copy

Explanation: "tabacat" is not a palindrome.

**Constraints:**

- `1 <= s.length <= 1000`
- `s` is made up of only printable ASCII characters.

  
  
Recommended Time & Space Complexity

  
Hint 1

  
Hint 2

  
Hint 3

  

  

## 1. Reverse String

- Python
- Java
- C++
- JavaScript
- C#
- Go
- Kotlin
- Swift

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        newStr = ''
        for c in s:
            if c.isalnum():
                newStr += c.lower()
        return newStr == newStr[::-1]
```

Copy

### Time & Space Complexity

- Time complexity: O(n)O(n)
- Space complexity: O(n)O(n)

---

## 2. Two Pointers

- Python
- Java
- C++
- JavaScript
- C#
- Go
- Kotlin
- Swift

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        l, r = 0, len(s) - 1

        while l < r:
            while l < r and not self.alphaNum(s[l]):
                l += 1
            while r > l and not self.alphaNum(s[r]):
                r -= 1
            if s[l].lower() != s[r].lower():
                return False
            l, r = l + 1, r - 1
        return True
    
    def alphaNum(self, c):
        return (ord('A') <= ord(c) <= ord('Z') or 
                ord('a') <= ord(c) <= ord('z') or 
                ord('0') <= ord(c) <= ord('9'))
```

Copy

### Time & Space Complexity

- Time complexity: O(n)O(n)
- Space complexity: O(1)O(1)
# 167. Two Sum II Input Array Is Sorted - Explanation

[Problem Link](https://neetcode.io/problems/two-integer-sum-ii/)

## Description

Given an array of integers `numbers` that is sorted in **non-decreasing order**.

Return the indices (**1-indexed**) of two numbers, `[index1, index2]`, such that they add up to a given target number `target` and `index1 < index2`. Note that `index1` and `index2` cannot be equal, therefore you may not use the same element twice.

There will always be **exactly one valid solution**.

Your solution must use O(1)O(1) additional space.

**Example 1:**

```java
Input: numbers = [1,2,3,4], target = 3

Output: [1,2]
```

Copy

Explanation:  
The sum of 1 and 2 is 3. Since we are assuming a 1-indexed array, `index1` = 1, `index2` = 2. We return `[1, 2]`.

**Constraints:**

- `2 <= numbers.length <= 1000`
- `-1000 <= numbers[i] <= 1000`
- `-1000 <= target <= 1000`

## 4. Two Pointers

- Python
- Java
- C++
- JavaScript
- C#
- Go
- Kotlin
- Swift

```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        l, r = 0, len(numbers) - 1

        while l < r:
            curSum = numbers[l] + numbers[r]

            if curSum > target:
                r -= 1
            elif curSum < target:
                l += 1
            else:
                return [l + 1, r + 1]
        return []
```
### Time & Space Complexity

- Time complexity: O(n)O(n)
- Space complexity: O(1)O(1)
# 15. 3Sum - Explanation

[Problem Link](https://neetcode.io/problems/three-integer-sum/)

## Description

Given an integer array `nums`, return all the triplets `[nums[i], nums[j], nums[k]]` where `nums[i] + nums[j] + nums[k] == 0`, and the indices `i`, `j` and `k` are all distinct.

The output should _not_ contain any duplicate triplets. You may return the output and the triplets in **any order**.

**Example 1:**

```java
Input: nums = [-1,0,1,2,-1,-4]

Output: [[-1,-1,2],[-1,0,1]]
```

Copy

Explanation:  
`nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.`  
`nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.`  
`nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.`  
The distinct triplets are `[-1,0,1]` and `[-1,-1,2]`.

**Example 2:**

```java
Input: nums = [0,1,1]

Output: []
```

Copy

Explanation: The only possible triplet does not sum up to 0.

**Example 3:**

```java
Input: nums = [0,0,0]

Output: [[0,0,0]]
```

Copy

Explanation: The only possible triplet sums up to 0.

**Constraints:**

- `3 <= nums.length <= 1000`
- `-10^5 <= nums[i] <= 10^5`
## 3. Two Pointers

- Python
- Java
- C++
- JavaScript
- C#
- Go
- Kotlin
- Swift

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        res = []
        nums.sort()

        for i, a in enumerate(nums):
            if a > 0:
                break

            if i > 0 and a == nums[i - 1]:
                continue

            l, r = i + 1, len(nums) - 1
            while l < r:
                threeSum = a + nums[l] + nums[r]
                if threeSum > 0:
                    r -= 1
                elif threeSum < 0:
                    l += 1
                else:
                    res.append([a, nums[l], nums[r]])
                    l += 1
                    r -= 1
                    while nums[l] == nums[l - 1] and l < r:
                        l += 1
                        
        return res
```

Copy

### Time & Space Complexity

- Time complexity: O(n2)O(n2)
- Space complexity:
    - O(1)O(1) or O(n)O(n) extra space depending on the sorting algorithm.
    - O(m)O(m) space for the output list.

> Where mm is the number of triplets and nn is the length of the given array.

# 567. Permutation In String - Explanation

[Problem Link](https://neetcode.io/problems/permutation-string/)

## Description

You are given two strings `s1` and `s2`.

Return `true` if `s2` contains a permutation of `s1`, or `false` otherwise. That means if a permutation of `s1` exists as a substring of `s2`, then return `true`.

Both strings only contain lowercase letters.

**Example 1:**

```java
Input: s1 = "abc", s2 = "lecabee"

Output: true
```

Copy

Explanation: The substring `"cab"` is a permutation of `"abc"` and is present in `"lecabee"`.

**Example 2:**

```java
Input: s1 = "abc", s2 = "lecaabee"

Output: false
```

Copy

**Constraints:**

- `1 <= s1.length, s2.length <= 1000`
## 3. Sliding Window

- Python
- Java
- C++
- JavaScript
- C#
- Go
- Kotlin
- Swift

```python
class Solution:
    def checkInclusion(self, s1: str, s2: str) -> bool:
        if len(s1) > len(s2):
            return False
        
        s1Count, s2Count = [0] * 26, [0] * 26
        for i in range(len(s1)):
            s1Count[ord(s1[i]) - ord('a')] += 1
            s2Count[ord(s2[i]) - ord('a')] += 1
        
        matches = 0
        for i in range(26):
            matches += (1 if s1Count[i] == s2Count[i] else 0)
        
        l = 0
        for r in range(len(s1), len(s2)):
            if matches == 26:
                return True
            
            index = ord(s2[r]) - ord('a')
            s2Count[index] += 1
            if s1Count[index] == s2Count[index]:
                matches += 1
            elif s1Count[index] + 1 == s2Count[index]:
                matches -= 1

            index = ord(s2[l]) - ord('a')
            s2Count[index] -= 1
            if s1Count[index] == s2Count[index]:
                matches += 1
            elif s1Count[index] - 1 == s2Count[index]:
                matches -= 1
            l += 1
        return matches == 26
```

Copy

### Time & Space Complexity

- Time complexity: O(n)O(n)
- Space complexity: O(1)O(1)
  
  # 20. Valid Parentheses - Explanation

[Problem Link](https://neetcode.io/problems/validate-parentheses/)

## Description

You are given a string `s` consisting of the following characters: `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`.

The input string `s` is valid if and only if:

1. Every open bracket is closed by the same type of close bracket.
2. Open brackets are closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

Return `true` if `s` is a valid string, and `false` otherwise.

**Example 1:**

```java
Input: s = "[]"

Output: true
```

Copy

**Example 2:**

```java
Input: s = "([{}])"

Output: true
```

Copy

**Example 3:**

```java
Input: s = "[(])"

Output: false
```

Copy

Explanation: The brackets are not closed in the correct order.

**Constraints:**

- `1 <= s.length <= 1000`

## 2. Stack

- Python
- Java
- C++
- JavaScript
- C#
- Go
- Kotlin
- Swift

```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        closeToOpen = { ")" : "(", "]" : "[", "}" : "{" }

        for c in s:
            if c in closeToOpen:
                if stack and stack[-1] == closeToOpen[c]:
                    stack.pop()
                else:
                    return False
            else:
                stack.append(c)
        
        return True if not stack else False
```

Copy

### Time & Space Complexity

- Time complexity: O(n)O(n)
- Space complexity: O(n)O(n)
# 22. Generate Parentheses - Explanation

[Problem Link](https://neetcode.io/problems/generate-parentheses/)

## Description

You are given an integer `n`. Return all well-formed parentheses strings that you can generate with `n` pairs of parentheses.

**Example 1:**

```java
Input: n = 1

Output: ["()"]
```

Copy

**Example 2:**

```java
Input: n = 3

Output: ["((()))","(()())","(())()","()(())","()()()"]
```

Copy

You may return the answer in **any order**.

**Constraints:**

- `1 <= n <= 7`
## 2. Backtracking

- Python
- Java
- C++
- JavaScript
- C#
- Go
- Kotlin
- Swift

```python
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        stack = []
        res = []

        def backtrack(openN, closedN):
            if openN == closedN == n:
                res.append("".join(stack))
                return

            if openN < n:
                stack.append("(")
                backtrack(openN + 1, closedN)
                stack.pop()
            if closedN < openN:
                stack.append(")")
                backtrack(openN, closedN + 1)
                stack.pop()

        backtrack(0, 0)
        return res
```

Copy

### Time & Space Complexity

- Time complexity: O(4nn)O(n​4n​)
- Space complexity: O(n)O(n)
# 206. Reverse Linked List - Explanation

[Problem Link](https://neetcode.io/problems/reverse-a-linked-list/)

## Description

Given the beginning of a singly linked list `head`, reverse the list, and return the new beginning of the list.

**Example 1:**

```java
Input: head = [0,1,2,3]

Output: [3,2,1,0]
```

Copy

**Example 2:**

```java
Input: head = []

Output: []
```

Copy

**Constraints:**

- `0 <= The length of the list <= 1000`.
- `-1000 <= Node.val <= 1000`

  
  
Recommended Time & Space Complexity

  
Hint 1

  
Hint 2

  
Hint 3

  
Hint 4

  

  

## 1. Recursion

- Python
- Java
- C++
- JavaScript
- C#
- Go
- Kotlin
- Swift

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if not head:
            return None

        newHead = head
        if head.next:
            newHead = self.reverseList(head.next)
            head.next.next = head
        head.next = None
        
        return newHead
```

Copy

### Time & Space Complexity

- Time complexity: O(n)O(n)
- Space complexity: O(n)O(n)

---

## 2. Iteration

- Python
- Java
- C++
- JavaScript
- C#
- Go
- Kotlin
- Swift

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        prev, curr = None, head

        while curr:
            temp = curr.next
            curr.next = prev
            prev = curr
            curr = temp
        return prev
```

Copy

### Time & Space Complexity

- Time complexity: O(n)O(n)
- Space complexity: O(1)O(1)
---

## 4. Two Pointers

- Python
- Java
- C++
- JavaScript
- C#
- Go
- Kotlin
- Swift

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        dummy = ListNode(0, head)
        left = dummy
        right = head

        while n > 0:
            right = right.next
            n -= 1

        while right:
            left = left.next
            right = right.next

        left.next = left.next.next
        return dummy.next
```

Copy

### Time & Space Complexity

- Time complexity: O(N)O(N)
- Space complexity: O(1)O(1)