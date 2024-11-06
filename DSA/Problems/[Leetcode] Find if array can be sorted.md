# [LeetCode] Find if Array can be sorted

## Problem Description

https://leetcode.com/problems/find-if-array-can-be-sorted/description/

## Key TakeAway

- Sort the Array Using Bubble Sort (Which involves swapping elements)
- Check if a certain element can not be swapped (Using the set Bits)
  - if swapping not possible: return false
- Return True

## New Info

- builtin_popcount(int): can be used to find out the set bits of an integer

## Solution

```cpp
class Solution {
public:
    bool canSortArray(vector<int>& nums) {
        int n = nums.size();
        bool swapped;

        for (int i = 0; i < n - 1; i++) {
            swapped = false;
            for (int j = 0; j < n - i - 1; j++) {
                if (nums[j] > nums[j + 1]) {
                    int setBits1 = __builtin_popcount(nums[j]);
                    int setBits2 = __builtin_popcount(nums[j+1]);

                    if(setBits1 != setBits2){
                        return false;
                    }

                    swap(nums[j], nums[j + 1]);
                    swapped = true;
                }
            }

            // If no two elements were swapped, then break
            if (!swapped)
                break;
        }
        return true;
    }
};
```
