[150. Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/description/)

思路：
这道题其实是求后缀表达式的值，这和求中缀表达式的值有一定的相似之处：
[227. Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/description/)

但是由于后缀表达式运算符在操作数后，所以不能采用中缀表达式那种将所有运算转成加法的操作，而是遇到一个操作符立马计算：

```java
class Solution {
    Stack<Integer> opNum = new Stack<>();

    public int evalRPN(String[] tokens) {
        for (String token : tokens) {
            if ("+".equals(token) || "-".equals(token) || "*".equals(token) || "/".equals(token)) {
                int num2 = opNum.pop();
                int num1 = opNum.pop();
                opNum.push(cal(num1, num2, token));
            } else {
                opNum.push(Integer.parseInt(token));
            }
        }
        return opNum.pop();
    }

    private int cal(int num1, int num2, String op) {
        int result = 0;
        if ("+".equals(op)) {
            result = num1 + num2;
        }
        if ("-".equals(op)) {
            result = num1 - num2;
        }
        if ("*".equals(op)) {
            result = num1 * num2;
        }
        if ("/".equals(op)) {
            result = num1 / num2;
        }
        return result;
    }
}
```