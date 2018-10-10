[739. Daily Temperatures](https://leetcode.com/problems/daily-temperatures/description/)

思路：
从字面意思上理解，就是在“未来”的数据里找一个离自己最近的大于自己的数，这个从结构上就可以联想到“后进先出”的栈。
实现：
+ 如果栈为空，或者当前温度不比栈顶元素大，则将当前温度入栈；
+ 否则，则当前温度比栈顶元素大，已经找到了比栈顶元素大的温度值，栈顶元素可以出栈了，出栈后再接着比较下一个栈顶元素。

```java
class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        Stack<Integer> stack = new Stack<>();
        int[] result = new int[temperatures.length];
        for (int i = 0; i < temperatures.length; i++) {
            while (!stack.isEmpty() && (temperatures[stack.peek()] < temperatures[i])) {
                result[stack.peek()] = i - stack.peek();
                stack.pop();
            }
            stack.push(i);
        }
        return result;
    }
}
```