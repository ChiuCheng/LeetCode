[494. Target Sum](https://leetcode.com/problems/target-sum/description/)

思路：  
这一题leetcode的discuss里面给了一个[超级巧妙的解法](https://leetcode.com/problems/target-sum/discuss/97334/Java-(15-ms)-C++-(3-ms)-O(ns)-iterative-DP-solution-using-subset-sum-with-explanation)，将这个问题转换成求解子数组的和等于一个目标值的问题，用动态规划去做就好了，详细说明请点上面的链接过去看，顺便膜拜下原作者。

实现：  
```java
class Solution {
    public int findTargetSumWays(int[] nums, int S) {
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        return sum < S || (S + sum) % 2 != 0 ? 0 : findSubset(nums, (S + sum) >>> 1);
    }

    private int findSubset(int[] nums, int target) {
        int[] dp = new int[target + 1];
        dp[0] = 1;
        for (int num : nums) {
            for (int i = target; i >= num; i--) {
                dp[i] += dp[i - num];
            }
        }
        return dp[target];
    }
}
```
这里有两点需要注意：
1. `sum < S` 这个条件不仅有利于尽早判断是否有有效的组合方式，还可以避免下面dp的运算出现`Memory exceed`；
2. dp的运算的`for (int i = target; i >= num; i--)`是从target递减来算的，顺序不可以倒过来(不可以由num往target算)，倒过来会导致结果变大且不合理，读者可以自己想一想是为什么。
