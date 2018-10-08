[279. Perfect Squares](https://leetcode.com/problems/perfect-squares/description/)

思路：  
这是一道典型的动态规划题目，找到比自己小的完全平方数，然后加上差值。  
假设当前值为`i`, `j*j < i`, 则`i` = `j*j` + `i -j*j`, 进而： 
```
dp[i] = 1 + dp[i - j*j] //其中`1`代表完全平方数`j*j` 
```
这里`i - j*j`的值可能不是完全平方数，但是没有关系，`dp[i-j*j]`表示了组成该数所需要的最少完全平方数。

```java
class Solution {
    public int numSquares(int n) {
        int[] dp = new int[n + 1];
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[0] = 0;
        for (int i = 1; i < n + 1; i++) {
            for (int j = 1; j * j <= i; j++) {
                dp[i] = Math.min(dp[i - j * j] + 1, dp[i]);
            }
        }
        return dp[n];
    }
}
```
这里尤其注意的是用`for (int j = 1; j * j <= i; j++)`技巧来寻找小于i的完全平方数。