# Bubble Sort

## Description

Bubble Sort is a simple sorting algorithm that repeatedly compares and swaps adjacent elements if they are in the wrong order. The largest unsorted element "bubbles" up to its correct position in each pass.

### Logic:

To sort an array with \( n \) elements, Bubble Sort requires up to \( n-1 \) iterations. In each iteration, we compare each pair of adjacent elements and swap them if necessary. By the \( i \)-th iteration, the largest unsorted element is placed at the end of the list.

The optimization here is the use of the `swapped` flag. If no elements are swapped during a pass, the array is already sorted, and the algorithm can stop early.

### Time Complexity:

- **Best Case**: \( O(n) \) when the array is already sorted. The algorithm will only need one pass through the array with no swaps, so the `swapped` flag will prevent further iterations.
- **Average Case**: \( O(n^2) \), which occurs when the elements are in random order. The algorithm still compares all pairs and may need several swaps.
- **Worst Case**: \( O(n^2) \), when the array is sorted in reverse order. Every pair will need to be swapped, resulting in the maximum number of comparisons and swaps.

### Space Complexity:

- **Space Complexity**: \( O(1) \), as Bubble Sort is an in-place sorting algorithm that doesn't require any additional memory.

## Code

```cpp
// Optimized version of Bubble Sort
void bubbleSort(vector<int>& arr) {
    int n = arr.size();
    bool swapped;

    for (int i = 0; i < n - 1; i++) {
        swapped = false;
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
                swapped = true;
            }
        }

        // Break early if no elements were swapped in the inner loop
        if (!swapped)
            break;
    }
}
```

## Link

[Bubble Sort Explanation on GeeksforGeeks](https://www.geeksforgeeks.org/bubble-sort-algorithm/)
