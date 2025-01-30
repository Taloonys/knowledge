https://www.youtube.com/watch?v=RYT08CaYq6A&t=30s
https://www.youtube.com/watch?v=DjYZk8nrXVY&t=487s

**SHOULD SPLIT IN FILES**

# Prefix sum array
> - **When?** -> Wanna find summ of values in required (sub)range

```cpp
#include <vector>

class NumArray {
    std::vector<int> nums_;

public:
    NumArray(vector<int>& nums) 
        : nums_(nums.size())
    {
        //
        // For example we have array: 1, 2, 3, 4, 5
        // Here we form an array of accumulating character: 1, 3, 6, 10, 15
        // i.e. we accumulate bubble-sum values -> 0+1 = 1 , 1+2 = 3 , 1+2+3 = 6 ...
        // 
        std::partial_sum(nums.begin(), nums.end(), nums_.begin());
    }
    
    int sumRange(int left, int right) 
    {
        //
        // We already summed an array, so now we just use subtraction (formula) to get sum of a range of elements
        // sum[i, j] = P[j] - P[i-1]
        //

        return left == 0 
            ? nums_[right] 
            : nums_[right] - nums_[left - 1];           // <-- to be accurate, THIS is formula usage
    }
};
```

# Binary Search
> - Pick 3 positions: `low` (leftmost), `high` (rightmost) and `mid` (middle between `low` & `high`)
> - Compare "desired value" and `mid` value.
> - If `mid` value is (for example) `greater` than "desired" -> drop values that are `less` then `mid`.
> - New `low` value is now right after `mid` value; new `mid` value should be recalculated again
> - Repeat until find desired value...

> - **REQUIRED** sorted array

## Example when we know target value
```cpp
int FindMedianPos(const std::vector<int> nums, const int target) {
  if (nums.empty())                    // in case sequence is empty
      return -1;                  

	int low    = 0;
	int high   = nums.size() - 1;

	while (low <= high) {
		cons int mid = (high + low) / 2;    // recalculate mid value everytime
		
		if (nums[mid] == target)
		    return mid;                     // if found
		
		if (nums[mid] < target) 
		    low = mid + 1;                  // drop numbers left from the mid value
    else
        high = mid - 1;                 // drop numbers right from the mid value
	}
	
	return -1;                            // didn't found anything
}


int main() {
	std::vector v = {1, 2, 5, 9};                   // test case
	const auto median_pos = FindMedianPos(v, 5);    // find pos of value 5
	printf("median pos is %d", median_pos);

	return 0;
}
```

## Example we don't know target (aka find median)
-

## Example with multiple sequences
-

