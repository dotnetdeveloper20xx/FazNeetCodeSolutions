# FazNeetCodeSolutions
# 🔍 Problem: Efficiently Search a 2D Matrix Using Binary Search

## ✅ Goal
Given a 2D matrix, determine whether a specific target number exists in it.

### Example:
Matrix:
```
[
  [ 1,  3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
```
Target: `16`

We should return `true` because 16 is present in the matrix.

---

## 🧠 How the Matrix is Organized
- Each row is sorted from left to right.
- The first number of a row is greater than the last number of the previous row.

So, the matrix behaves like one long sorted list:
```
[1, 3, 5, 7, 10, 11, 16, 20, 23, 30, 34, 50]
```

This allows us to use a **binary search**, which is a fast way to find a number in a sorted list.

---

## 🚀 What is Binary Search?
Binary search works by repeatedly dividing a sorted list in half:
- Check the middle element.
- If it's equal to the target → return `true`
- If it's smaller → search the right half
- If it's larger → search the left half

This continues until the element is found or the list is empty.

---

## 💡 Flattening the Matrix
Although the matrix is 2D, we can treat it like a 1D list by doing some math:

To get the actual row and column from a 1D index:
```csharp
row = mid / numberOfColumns;
col = mid % numberOfColumns;
```
This lets us access matrix[row][col] without actually converting the matrix.

---

## ✅ Final C# Code (Iterative Binary Search with Comments)
```csharp
public class MatrixSearch
{
    // Method to search for a target in a 2D matrix using binary search
    public bool SearchMatrix(int[][] matrix, int target)
    {
        // Check if the matrix is null or empty
        if (matrix == null || matrix.Length == 0 || matrix[0].Length == 0)
        {
            return false; // Return false if matrix is invalid
        }

        int rows = matrix.Length;          // Total number of rows in the matrix
        int cols = matrix[0].Length;       // Total number of columns in each row

        int left = 0;                      // Binary search left pointer
        int right = rows * cols - 1;       // Binary search right pointer (flattened index)

        // Start binary search
        while (left <= right)
        {
            int mid = (left + right) / 2;  // Get the middle index (flattened)

            int row = mid / cols;          // Convert flattened mid index to row
            int col = mid % cols;          // Convert flattened mid index to column

            int value = matrix[row][col];  // Access the matrix element at (row, col)

            if (value == target)
            {
                return true;               // Target found
            }
            else if (value < target)
            {
                left = mid + 1;            // Move to the right half
            }
            else
            {
                right = mid - 1;           // Move to the left half
            }
        }

        return false;                      // Target not found after search
    }
}
```

---

## 🧪 Example Usage
```csharp
class Program
{
    static void Main()
    {
        int[][] matrix = new int[][]
        {
            new int[] {1, 3, 5, 7},
            new int[] {10, 11, 16, 20},
            new int[] {23, 30, 34, 50}
        };

        MatrixSearch searcher = new MatrixSearch();
        bool found = searcher.SearchMatrix(matrix, 16);

        Console.WriteLine(found); // Output: True
    }
}
```

---

## 📌 Summary
- We treat the 2D matrix like a sorted 1D array.
- We apply binary search to find the target quickly.
- We convert 1D indices to 2D matrix positions



# 🧮 Problem: Product of Array Except Self (Without Division, O(n) Time)

## ✅ Goal
Given an array of integers `nums`, return an array `answer` where each element at index `i` is the **product of all elements in the array except `nums[i]`**, **without using division**, and with **O(n)** time complexity.

### Example:
```text
Input:  nums = [1, 2, 3, 4]
Output: answer = [24, 12, 8, 6]
```

### Why?
- For `answer[0]`: 2 * 3 * 4 = 24
- For `answer[1]`: 1 * 3 * 4 = 12
- For `answer[2]`: 1 * 2 * 4 = 8
- For `answer[3]`: 1 * 2 * 3 = 6

We achieve this **without dividing** the total product and without nested loops.

---

## 🔍 Key Insight
We can break the product at each index `i` into:
- A **prefix product**: product of all elements before `i`
- A **suffix product**: product of all elements after `i`

So:
```text
answer[i] = product of all elements before i * product of all elements after i
```

---

## ✅ C# Code With Line-by-Line Comments
```csharp
public class ProductArray
{
    public int[] ProductExceptSelf(int[] nums)
    {
        int n = nums.Length;                   // Length of the input array
        int[] answer = new int[n];             // Output array to hold result

        // Step 1: Calculate prefix products
        answer[0] = 1;                         // Nothing before index 0, so set to 1
        for (int i = 1; i < n; i++)
        {
            answer[i] = answer[i - 1] * nums[i - 1];
            // answer[i] holds product of all elements to the left of i
        }

        // Step 2: Calculate suffix products and multiply with prefix
        int right = 1;                         // Holds the product of all elements to the right
        for (int i = n - 1; i >= 0; i--)
        {
            answer[i] *= right;                // Multiply current prefix product with suffix
            right *= nums[i];                  // Update right to include current element
        }

        return answer;                         // Final result
    }
}
```

---

## 🧪 Example Usage
```csharp
class Program
{
    static void Main()
    {
        int[] nums = new int[] {1, 2, 3, 4};

        ProductArray calculator = new ProductArray();
        int[] result = calculator.ProductExceptSelf(nums);

        Console.WriteLine(string.Join(", ", result));
        // Output: 24, 12, 8, 6
    }
}
```

---

## 📌 Summary
- We compute the result in **O(n)** time by doing two passes:
  1. One from **left to right** for prefix products
  2. One from **right to left** for suffix products
- We multiply prefix and suffix to get the final result.
- We avoid using division, making the solution efficient and safe for arrays with zeroes.

---

# 🧮 Problem: Two Sum — O(n) Using HashMap

### ✅ Goal
Given an array of integers `nums` and an integer `target`, return the indices of the two numbers that add up to the target.

### Example:
```
Input:  nums = [2, 7, 11, 15], target = 9
Output: [0, 1] // Because nums[0] + nums[1] = 2 + 7 = 9
```

---

### 🔍 Key Insight
Use a dictionary to store values we've seen and their indices. While iterating, for each number, check if its complement (target - number) exists in the dictionary.

---

### ✅ C# Code With Line-by-Line Comments
```csharp
public class Solution
{
    public int[] TwoSum(int[] nums, int target)
    {
        Dictionary<int, int> map = new();     // Dictionary to store number and its index

        for (int i = 0; i < nums.Length; i++)
        {
            int complement = target - nums[i]; // What number do we need to reach the target?

            if (map.ContainsKey(complement))   // If we already saw that number, we found the answer
            {
                return new int[] { map[complement], i }; // Return indices of the two numbers
            }

            map[nums[i]] = i; // Otherwise, store the current number with its index
        }

        return Array.Empty<int>(); // No solution found (problem guarantees one exists)
    }
}
```

---

### 🧪 Example Usage
```csharp
class Program
{
    static void Main()
    {
        int[] nums = new int[] {2, 7, 11, 15};
        int target = 9;

        Solution solver = new Solution();
        int[] result = solver.TwoSum(nums, target);

        Console.WriteLine(string.Join(", ", result));
        // Output: 0, 1
    }
}
```

---

### 📌 Summary
- Time complexity is **O(n)** because we loop through the array once.
- We avoid nested loops by using a **dictionary** to remember what we’ve seen.
- Efficient, easy to understand, and works on all valid inputs.

---


# 🧮 Problem: Find Duplicates — O(n) Using HashSet

### ✅ Goal
Given an array of integers, check if any value appears **at least twice**. Return `true` if any duplicates exist, otherwise `false`.

### Example:
```
Input:  nums = [1, 2, 3, 4, 1]
Output: true  // Because the number 1 appears more than once
```

---

### 🔍 Key Insight
Use a `HashSet` to track numbers we've seen. If we try to add a number and it's already in the set, it's a duplicate.

---

### ✅ C# Code With Line-by-Line Comments
```csharp
public class DuplicateChecker
{
    public bool HasDuplicate(int[] nums)
    {
        HashSet<int> seen = new(); // Create a set to track seen numbers

        foreach (int num in nums)
        {
            if (!seen.Add(num)) // Try to add the number to the set
            {
                return true;    // If Add returns false, it's already in the set => duplicate found
            }
        }

        return false; // No duplicates were found in the array
    }
}
```

---

### 🧪 Example Usage
```csharp
class Program
{
    static void Main()
    {
        int[] nums = new int[] {1, 2, 3, 4, 1};

        DuplicateChecker checker = new DuplicateChecker();
        bool hasDuplicate = checker.HasDuplicate(nums);

        Console.WriteLine(hasDuplicate); // Output: True
    }
}
```

### 📌 Summary
- Time complexity is **O(n)** — we only loop once.
- A `HashSet` lets us check for duplicates in constant time.
- This is the most efficient and clean way to detect duplicates.

---

# 3️⃣ 🧮 Problem: Max Subarray Sum (Kadane’s Algorithm) — O(n)

### ✅ Goal
Find the contiguous subarray within an array (containing at least one number) that has the largest sum and return that sum.

### Example:
```
Input:  nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
Output: 6
Explanation: The subarray [4, -1, 2, 1] has the largest sum = 6
```

### 🔍 Key Insight
Use **Kadane’s Algorithm**:
- At each element, decide to start a new subarray or extend the existing one.
- Track the maximum sum seen so far.

### ✅ C# Code With Comments
```csharp
public class MaxSubarraySolver
{
    public int MaxSubArray(int[] nums)
    {
        int current = nums[0]; // Start with the first element
        int max = nums[0];     // Also start with first element as max

        for (int i = 1; i < nums.Length; i++)
        {
            current = Math.Max(nums[i], current + nums[i]); // Start new or extend
            max = Math.Max(max, current); // Track max
        }

        return max;
    }
}
```

---

# 4️⃣ 🧮 Problem: Sort Array using Merge Sort — O(n log n)

### ✅ Goal
Sort an array of integers using the Merge Sort algorithm.

### Example:
```
Input:  [5, 2, 3, 1]
Output: [1, 2, 3, 5]
```

### 🔍 Key Insight
Use recursion to split the array, then merge the sorted halves.

### ✅ C# Code With Comments
```csharp
public class MergeSorter
{
    public int[] MergeSort(int[] nums)
    {
        if (nums.Length <= 1) return nums;

        int mid = nums.Length / 2;
        int[] left = MergeSort(nums[..mid]);
        int[] right = MergeSort(nums[mid..]);

        return Merge(left, right);
    }

    private int[] Merge(int[] left, int[] right)
    {
        List<int> result = new();
        int i = 0, j = 0;

        while (i < left.Length && j < right.Length)
            result.Add(left[i] < right[j] ? left[i++] : right[j++]);

        result.AddRange(left[i..]);
        result.AddRange(right[j..]);

        return result.ToArray();
    }
}
```

---

# 5️⃣ 🧮 Problem: Binary Search — O(log n)

### ✅ Goal
Find the index of a target element in a **sorted** array.

### Example:
```
Input: nums = [-1, 0, 3, 5, 9, 12], target = 9
Output: 4
```

### 🔍 Key Insight
Binary search splits the array and searches one half.

### ✅ C# Code With Comments
```csharp
public class BinarySearcher
{
    public int BinarySearch(int[] nums, int target)
    {
        int left = 0, right = nums.Length - 1;

        while (left <= right)
        {
            int mid = (left + right) / 2;

            if (nums[mid] == target) return mid;
            if (nums[mid] < target) left = mid + 1;
            else right = mid - 1;
        }

        return -1;
    }
}
```

---

# 6️⃣ 🧮 Problem: Merge Intervals — O(n log n)

### ✅ Goal
Merge overlapping intervals.

### Example:
```
Input: [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
```

### 🔍 Key Insight
Sort by start times, then merge overlapping intervals.

### ✅ C# Code With Comments
```csharp
public class IntervalMerger
{
    public int[][] Merge(int[][] intervals)
    {
        Array.Sort(intervals, (a, b) => a[0].CompareTo(b[0]));
        List<int[]> merged = new();
        int[] current = intervals[0];

        foreach (var interval in intervals)
        {
            if (interval[0] <= current[1])
                current[1] = Math.Max(current[1], interval[1]);
            else
            {
                merged.Add(current);
                current = interval;
            }
        }

        merged.Add(current);
        return merged.ToArray();
    }
}
```

---

# 7️⃣ 🧮 Problem: Kth Largest Element — O(n log k)

### ✅ Goal
Find the **kth largest element** in an unsorted array.

### Example:
```
Input: nums = [3,2,1,5,6,4], k = 2
Output: 5
```

### 🔍 Key Insight
Use a min-heap (PriorityQueue) to keep top-k largest elements.

### ✅ C# Code With Comments
```csharp
public class KthLargestFinder
{
    public int FindKthLargest(int[] nums, int k)
    {
        PriorityQueue<int, int> minHeap = new();

        foreach (var num in nums)
        {
            minHeap.Enqueue(num, num);
            if (minHeap.Count > k)
                minHeap.Dequeue();
        }

        return minHeap.Peek();
    }
}
```

---

# 8️⃣ 🧮 Problem: Top K Frequent Elements — O(n log k)

### ✅ Goal
Return the `k` most frequent elements.

### Example:
```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```

### 🔍 Key Insight
Count frequencies, then maintain a min-heap of size k.

### ✅ C# Code With Comments
```csharp
public class TopKFrequentFinder
{
    public int[] TopKFrequent(int[] nums, int k)
    {
        Dictionary<int, int> freq = new();
        foreach (var num in nums)
            freq[num] = freq.GetValueOrDefault(num, 0) + 1;

        PriorityQueue<int, int> heap = new();
        foreach (var entry in freq)
        {
            heap.Enqueue(entry.Key, entry.Value);
            if (heap.Count > k)
                heap.Dequeue();
        }

        return heap.UnorderedItems.Select(x => x.Element).ToArray();
    }
}
```


# 1️⃣2️⃣ 🧮 Problem: Palindrome Check — O(n)

### ✅ Goal
Determine whether a given string is a **palindrome** — meaning it reads the same backward as forward.

### Example:
```
Input:  "racecar"
Output: true
```

### 🔍 Key Insight
Use **two pointers**:
- One starts from the beginning
- One starts from the end
- Compare characters until they meet

### ✅ C# Code With Line-by-Line Comments
```csharp
public class PalindromeChecker
{
    public bool IsPalindrome(string s)
    {
        int left = 0;
        int right = s.Length - 1; // Pointers from both ends

        while (left < right)
        {
            if (s[left] != s[right])
                return false; // Mismatch found

            left++; // Move inward
            right--;
        }

        return true; // All characters matched
    }
}
```

### 🧪 Example Usage
```csharp
class Program
{
    static void Main()
    {
        string word = "racecar";

        PalindromeChecker checker = new PalindromeChecker();
        bool isPal = checker.IsPalindrome(word);

        Console.WriteLine(isPal); // Output: True
    }
}
```

### 📌 Summary
- A very efficient two-pointer solution.
- Time complexity: **O(n)**, space: **O(1)**.
- You can add logic to ignore punctuation and spaces for advanced versions.

---

# 1️⃣3️⃣ 🧮 Problem: Matrix Search (Sorted Matrix) — O(m + n)

### ✅ Goal
Search for a target value in a **sorted 2D matrix** where:
- Each row is sorted left to right
- Each column is sorted top to bottom

### Example:
```
Matrix:
[
  [1,  4,  7, 11, 15],
  [2,  5,  8, 12, 19],
  [3,  6,  9, 16, 22],
  [10,13, 14,17, 24],
  [18,21, 23,26, 30]
]
Target: 5 → Output: true
Target: 20 → Output: false
```

### 🔍 Key Insight
Start from the **top-right corner**:
- If the current value is less than the target → go **down**
- If it’s greater → go **left**

This guarantees linear time: at most **m + n** steps.

### ✅ C# Code With Line-by-Line Comments
```csharp
public class MatrixSearcher
{
    public bool SearchMatrix(int[][] matrix, int target)
    {
        int row = 0;
        int col = matrix[0].Length - 1; // Start at top-right corner

        while (row < matrix.Length && col >= 0)
        {
            if (matrix[row][col] == target)
                return true; // Found it

            if (matrix[row][col] > target)
                col--; // Too big, move left
            else
                row++; // Too small, move down
        }

        return false; // Exhausted search
    }
}
```

### 🧪 Example Usage
```csharp
class Program
{
    static void Main()
    {
        int[][] matrix = new int[][]
        {
            new int[] {1, 4, 7, 11, 15},
            new int[] {2, 5, 8, 12, 19},
            new int[] {3, 6, 9, 16, 22},
            new int[] {10,13,14,17,24},
            new int[] {18,21,23,26,30}
        };

        MatrixSearcher searcher = new MatrixSearcher();

        Console.WriteLine(searcher.SearchMatrix(matrix, 5));  // Output: True
        Console.WriteLine(searcher.SearchMatrix(matrix, 20)); // Output: False
    }
}
```

### 📌 Summary
- Great example of **greedy navigation**.
- Time complexity: **O(m + n)**.
- Always start at **top-right** or **bottom-left** in such matrices.

---


# 9️⃣ 🧮 Problem: Sliding Window Maximum — O(n)

### ✅ Goal
Given an array of integers and a window size `k`, return the **maximum value in each window** as it slides across the array.

### Example:
```
Input:  nums = [1,3,-1,-3,5,3,6,7], k = 3
Output: [3,3,5,5,6,7]
```

### 🔍 Key Insight
- Use a **deque (double-ended queue)** to track the indices of potential maximums.
- Keep the **largest element at the front**, and remove elements that slide out of the window.

### ✅ C# Code With Line-by-Line Comments
```csharp
public class SlidingWindowMax
{
    public int[] MaxSlidingWindow(int[] nums, int k)
    {
        LinkedList<int> deque = new(); // Stores indices of useful elements
        List<int> result = new();      // Final result array

        for (int i = 0; i < nums.Length; i++)
        {
            // Remove indices that are out of this window
            if (deque.Count > 0 && deque.First.Value <= i - k)
                deque.RemoveFirst();

            // Remove indices of smaller elements from the back
            while (deque.Count > 0 && nums[deque.Last.Value] < nums[i])
                deque.RemoveLast();

            deque.AddLast(i); // Add current element index

            // Add to result once window is fully overlapping
            if (i >= k - 1)
                result.Add(nums[deque.First.Value]);
        }

        return result.ToArray();
    }
}
```

### 🧪 Example Usage
```csharp
class Program
{
    static void Main()
    {
        int[] nums = new int[] {1, 3, -1, -3, 5, 3, 6, 7};
        int k = 3;

        SlidingWindowMax solver = new SlidingWindowMax();
        var result = solver.MaxSlidingWindow(nums, k);

        Console.WriteLine(string.Join(", ", result)); // Output: 3, 3, 5, 5, 6, 7
    }
}
```

### 📌 Summary
- Deque gives constant-time max lookup and removal.
- Total time: **O(n)**, each element is added/removed once.
- Perfect for live data streaming, signal processing, or game analytics.

---

## 🔟 🧮 Problem: Find Substring (Pattern Match) — O(n)

### ✅ Goal
Check if a **specific pattern** (substring) exists in a larger string.

### Example:
```
Input:  s = "hello world", pattern = "lo w"
Output: true
```

### 🔍 Key Insight
Use a **sliding window** of the pattern’s length and compare each substring with the target pattern.

### ✅ C# Code With Line-by-Line Comments
```csharp
public class SubstringFinder
{
    public bool ContainsSubstring(string s, string pattern)
    {
        int patternLength = pattern.Length;

        for (int i = 0; i <= s.Length - patternLength; i++)
        {
            string window = s.Substring(i, patternLength); // Get current substring

            if (window == pattern)
                return true; // Pattern found
        }

        return false; // No match found
    }
}
```

### 🧪 Example Usage
```csharp
class Program
{
    static void Main()
    {
        string text = "hello world";
        string pattern = "lo w";

        SubstringFinder finder = new SubstringFinder();
        bool exists = finder.ContainsSubstring(text, pattern);

        Console.WriteLine(exists); // Output: True
    }
}
```

### 📌 Summary
- Sliding window through the main string.
- Simple and effective for basic pattern searches.
- For large-scale or wildcard patterns, use KMP or Regex (future upgrade).

---

## 1️⃣1️⃣ 🧮 Problem: Longest Substring Without Repeating Characters — O(n)

### ✅ Goal
Return the length of the **longest substring without repeating characters**.

### Example:
```
Input:  s = "abcabcbb"
Output: 3  // Longest is "abc"
```

### 🔍 Key Insight
Use a **sliding window** and **HashSet**:
- Move the window forward, keeping characters unique.
- When a duplicate appears, move the start pointer until it's gone.

### ✅ C# Code With Line-by-Line Comments
```csharp
public class UniqueSubstringFinder
{
    public int LengthOfLongestSubstring(string s)
    {
        HashSet<char> seen = new(); // Track characters in current window
        int left = 0, maxLen = 0;

        for (int right = 0; right < s.Length; right++)
        {
            while (!seen.Add(s[right])) // If already seen, shrink from the left
            {
                seen.Remove(s[left++]);
            }

            maxLen = Math.Max(maxLen, right - left + 1); // Update max length
        }

        return maxLen;
    }
}
```

### 🧪 Example Usage
```csharp
class Program
{
    static void Main()
    {
        string input = "abcabcbb";

        UniqueSubstringFinder finder = new UniqueSubstringFinder();
        int length = finder.LengthOfLongestSubstring(input);

        Console.WriteLine(length); // Output: 3
    }
}
```

### 📌 Summary
- This problem is a classic **sliding window** technique.
- HashSet gives constant time checks and removals.
- Time complexity: **O(n)** — best possible.

---

# 🧩 Advanced Coding Challenges Set 2 – C# Solutions with Clear Explanations

This document covers 9 advanced coding challenges in C#, each solved with the most optimal approach. Every section includes:
- ✅ Goal
- 🔍 Key Insight
- ✅ C# Code with Learning Comments
- 🧪 Example Usage
- 📌 Summary

---

## 1️⃣ 🧮 Problem: Minimum Window Substring — Advanced Sliding Window with Hash Maps

### ✅ Goal
Given strings `s` and `t`, return the **smallest substring of `s`** that contains all characters in `t` (including frequency).

### Example:
```
Input:  s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
```

### 🔍 Key Insight
Use two dictionaries:
- One to count characters in `t`
- One for the current window in `s`
Use two pointers to slide the window and adjust when all required characters are matched.

### ✅ C# Code With Learning Comments
```csharp
public class MinWindowFinder
{
    public string MinWindow(string s, string t)
    {
        if (s.Length < t.Length) return "";

        Dictionary<char, int> need = new();
        foreach (char c in t)
            need[c] = need.GetValueOrDefault(c, 0) + 1;

        Dictionary<char, int> window = new();
        int left = 0, right = 0, valid = 0;
        int minLen = int.MaxValue, start = 0;

        while (right < s.Length)
        {
            char c = s[right++];
            if (need.ContainsKey(c))
            {
                window[c] = window.GetValueOrDefault(c, 0) + 1;
                if (window[c] == need[c]) valid++;
            }

            while (valid == need.Count)
            {
                if (right - left < minLen)
                {
                    minLen = right - left;
                    start = left;
                }

                char d = s[left++];
                if (need.ContainsKey(d))
                {
                    if (window[d] == need[d]) valid--;
                    window[d]--;
                }
            }
        }

        return minLen == int.MaxValue ? "" : s.Substring(start, minLen);
    }
}
```

---

## 2️⃣ 🧮 Problem: Longest Consecutive Sequence — O(n) Using HashSet

### ✅ Goal
Given an unsorted array, find the length of the **longest consecutive sequence**.

### Example:
```
Input: [100, 4, 200, 1, 3, 2]
Output: 4  // Sequence: 1, 2, 3, 4
```

### 🔍 Key Insight
Use a HashSet to detect sequence starts. Only start counting if `num - 1` is **not** in the set.

### ✅ C# Code With Learning Comments
```csharp
public class SequenceFinder
{
    public int LongestConsecutive(int[] nums)
    {
        HashSet<int> set = new(nums);
        int longest = 0;

        foreach (int num in nums)
        {
            if (!set.Contains(num - 1)) // only start from the beginning of a sequence
            {
                int current = num;
                int length = 1;

                while (set.Contains(current + 1))
                {
                    current++;
                    length++;
                }

                longest = Math.Max(longest, length);
            }
        }

        return longest;
    }
}
```

---

## 3️⃣ 🧮 Problem: Valid Parentheses — O(n) Using Stack

### ✅ Goal
Check if a string of brackets is valid (every opening has a matching closing).

### Example:
```
Input: "()[]{}"
Output: true
```

### 🔍 Key Insight
Use a stack to push opening brackets. For each closing bracket, pop and match.

### ✅ C# Code With Learning Comments
```csharp
public class ParenthesesValidator
{
    public bool IsValid(string s)
    {
        Stack<char> stack = new();
        Dictionary<char, char> map = new()
        {
            { ')', '(' },
            { ']', '[' },
            { '}', '{' }
        };

        foreach (char c in s)
        {
            if (map.ContainsValue(c))
                stack.Push(c);
            else if (map.ContainsKey(c))
            {
                if (stack.Count == 0 || stack.Pop() != map[c])
                    return false;
            }
        }

        return stack.Count == 0;
    }
}
```

---

## 4️⃣ 🧮 Problem: Evaluate Reverse Polish Notation (RPN) — O(n) Using Stack

### ✅ Goal
Evaluate a postfix expression like ["2","1","+","3","*"] → Result = 9

### Example:
```
Input: ["2", "1", "+", "3", "*"]
Output: 9
```

### 🔍 Key Insight
Use a stack to compute: push numbers, pop for operations.

### ✅ C# Code With Learning Comments
```csharp
public class RpnEvaluator
{
    public int EvalRPN(string[] tokens)
    {
        Stack<int> stack = new();

        foreach (var token in tokens)
        {
            if (int.TryParse(token, out int num))
                stack.Push(num);
            else
            {
                int b = stack.Pop();
                int a = stack.Pop();

                stack.Push(token switch
                {
                    "+" => a + b,
                    "-" => a - b,
                    "*" => a * b,
                    "/" => a / b,
                    _ => throw new InvalidOperationException()
                });
            }
        }

        return stack.Pop();
    }
}
```

---

## 5️⃣ 🧮 Problem: Meeting Rooms — Scheduling with Intervals and Heap

### ✅ Goal
Determine if a person can attend all meetings (no overlaps).

### Example:
```
Input: [[0,30],[5,10],[15,20]]
Output: false
```

### 🔍 Key Insight
Sort intervals and check for overlaps.

### ✅ C# Code With Learning Comments
```csharp
public class MeetingScheduler
{
    public bool CanAttendMeetings(int[][] intervals)
    {
        Array.Sort(intervals, (a, b) => a[0].CompareTo(b[0]));

        for (int i = 1; i < intervals.Length; i++)
        {
            if (intervals[i][0] < intervals[i - 1][1])
                return false; // Overlap found
        }

        return true;
    }
}
```

---

## 6️⃣ 🧮 Problem: Group Anagrams — O(n k log k)

### ✅ Goal
Group all anagrams from a list of strings.

### Example:
```
Input: ["eat", "tea", "tan", "ate", "nat", "bat"]
Output: [["eat","tea","ate"],["tan","nat"],["bat"]]
```

### 🔍 Key Insight
Sort each word and use the sorted version as a dictionary key.

### ✅ C# Code With Learning Comments
```csharp
public class AnagramGrouper
{
    public IList<IList<string>> GroupAnagrams(string[] strs)
    {
        Dictionary<string, List<string>> map = new();

        foreach (var word in strs)
        {
            var chars = word.ToCharArray();
            Array.Sort(chars);
            string sorted = new string(chars);

            if (!map.ContainsKey(sorted))
                map[sorted] = new List<string>();

            map[sorted].Add(word);
        }

        return map.Values.ToList();
    }
}
```
[Previously included problems remain unchanged]

## 7️⃣ 🧮 Problem: Trie (Prefix Tree) — Insert, Search, StartsWith

### ✅ Goal
Implement a Trie data structure to support:
- Insert a word
- Search if a word exists
- Check if a prefix exists

### Example:
```
Insert: "apple"
Search: "apple" → true
StartsWith: "app" → true
```

### 🔍 Key Insight
Use a nested dictionary or tree structure to represent each character in a word.

### ✅ C# Code With Learning Comments
```csharp
public class Trie
{
    private class TrieNode
    {
        public Dictionary<char, TrieNode> Children = new();
        public bool IsEndOfWord = false;
    }

    private readonly TrieNode root = new();

    public void Insert(string word)
    {
        TrieNode node = root;
        foreach (char c in word)
        {
            if (!node.Children.ContainsKey(c))
                node.Children[c] = new TrieNode();
            node = node.Children[c];
        }
        node.IsEndOfWord = true; // Mark end of word
    }

    public bool Search(string word)
    {
        TrieNode node = root;
        foreach (char c in word)
        {
            if (!node.Children.ContainsKey(c)) return false;
            node = node.Children[c];
        }
        return node.IsEndOfWord; // True only if full word matches
    }

    public bool StartsWith(string prefix)
    {
        TrieNode node = root;
        foreach (char c in prefix)
        {
            if (!node.Children.ContainsKey(c)) return false;
            node = node.Children[c];
        }
        return true; // Prefix exists
    }
}
```

---

## 8️⃣ 🧮 Problem: Word Ladder — Shortest Transformation Using BFS

### ✅ Goal
Transform word `beginWord` to `endWord` changing one letter at a time. Each transformation must be a valid word from the list.
Return the length of the shortest transformation path.

### Example:
```
beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
Output: 5  // "hit" → "hot" → "dot" → "dog" → "cog"
```

### 🔍 Key Insight
Use **Breadth-First Search (BFS)** to explore transformations level by level.

### ✅ C# Code With Learning Comments
```csharp
public class WordLadderSolver
{
    public int LadderLength(string beginWord, string endWord, IList<string> wordList)
    {
        HashSet<string> wordSet = new(wordList);
        if (!wordSet.Contains(endWord)) return 0;

        Queue<(string word, int steps)> queue = new();
        queue.Enqueue((beginWord, 1));

        while (queue.Count > 0)
        {
            var (word, steps) = queue.Dequeue();

            if (word == endWord) return steps;

            for (int i = 0; i < word.Length; i++)
            {
                char[] chars = word.ToCharArray();

                for (char c = 'a'; c <= 'z'; c++)
                {
                    chars[i] = c;
                    string newWord = new string(chars);

                    if (wordSet.Remove(newWord)) // Only use each word once
                        queue.Enqueue((newWord, steps + 1));
                }
            }
        }

        return 0; // No transformation found
    }
}
```

---

## 9️⃣ 🧮 Problem: Decode String — Stack-Based Expansion of Nested Encoded Strings

### ✅ Goal
Given an encoded string like "3[a2[c]]", return its decoded form → "accaccacc".

### Example:
```
Input: "3[a2[c]]"
Output: "accaccacc"
```

### 🔍 Key Insight
Use a **stack** to decode nested structures (numbers, brackets, and substrings).

### ✅ C# Code With Learning Comments
```csharp
public class StringDecoder
{
    public string DecodeString(string s)
    {
        Stack<int> countStack = new();
        Stack<string> stringStack = new();
        string current = "";
        int k = 0;

        foreach (char c in s)
        {
            if (char.IsDigit(c))
            {
                k = k * 10 + (c - '0'); // Build full number
            }
            else if (c == '[')
            {
                countStack.Push(k);
                stringStack.Push(current);
                k = 0;
                current = "";
            }
            else if (c == ']')
            {
                int count = countStack.Pop();
                string prev = stringStack.Pop();
                current = prev + string.Concat(Enumerable.Repeat(current, count));
            }
            else
            {
                current += c;
            }
        }

        return current;
    }
}
```


