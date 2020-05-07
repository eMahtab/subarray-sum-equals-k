# Subarray Sum Equals k
## https://leetcode.com/problems/subarray-sum-equals-k

Given an array of integers and an integer k, you need to find the total number of continuous subarrays whose sum equals to k.

Example 1:
Input:nums = [1,1,1], k = 2
Output: 2

**Note:**
1. The length of the array is in range [1, 20,000].
2. The range of numbers in the array is [-1000, 1000] and the range of the integer k is [-1e7, 1e7].


# Implementation : Naive
```java
class Solution {

	public int subarraySum(int[] nums, int k) {
		int count = 0;
		if (nums == null || nums.length == 0)
			return 0;

		for (int i = 1; i <= nums.length; i++) {
			for (int index = 0; index < nums.length - i + 1; index++) {
				int sum = 0;
				for (int j = index; j < index + i; j++)
					sum += nums[j];
				if (sum == k)
					count++;
			}
		}

		return count;
	}
	
}
```
# Implementation 2 : Let's do little better
```java
public class Solution {
    public int subarraySum(int[] nums, int k) {
        int count = 0;
        for (int start = 0; start < nums.length; start++) {
            int sum=0;
            for (int end = start; end < nums.length; end++) {
                sum+=nums[end];
                if (sum == k)
                    count++;
            }
        }
        return count;
    }
}
```
# Implementation 3 : Even Better
### Approach :
Say you are given an array e.g. [a0, a1, a2, a3, a4, a5, a6... an] . 
```
[a0, a1, a2, a3, a4, a5, a6... an]
	 ^	     ^	
	sumI	    sumJ
sumI = sum of numbers till a2 (a0 + a1 + a2)
sumJ = sum of numbers till a5 (a0 + a1 + a2 + a3 + a4 + a5)
```

Now lets say the difference between sumJ and sumI is equal to k. 
What that means is, the sum of numbers between a2 and a5 is equal to k ( a3 + a4 + a5 = k ), which means we found a subarray whose sum is equal to k.

We can write a3 + a4 + a5 = k as sumJ - sumI = k and sumJ - sumI = k can be written as sumJ - k = sumI

The expression sumJ - k = sumI, means have we already seen a sum which is equal to sum at current index j minus k. If yes, it means we found a subarray whose sum is equal to k. 

And we keep track of how many times we see a particular sum using a HashMap.

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        if(nums == null || nums.length == 0)
            return 0;
        int count = 0;
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0, 1);
        int sum = 0;
        for(int i = 0; i < nums.length; i++) {
            sum += nums[i];
            if(map.containsKey(sum - k))
                count += map.get(sum-k);
            map.put(sum, map.getOrDefault(sum,0) + 1);
        }
        return count;
    }
}
```

# References :
https://leetcode.com/articles/subarray-sum-equals-k
