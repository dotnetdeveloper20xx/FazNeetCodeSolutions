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
