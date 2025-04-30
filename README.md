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

---

### 📌 Summary
- Time complexity is **O(n)** — we only loop once.
- A `HashSet` lets us check for duplicates in constant time.
- This is the most efficient and clean way to detect duplicates.

---

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

## 1️⃣3️⃣ 🧮 Problem: Matrix Search (Sorted Matrix) — O(m + n)

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




