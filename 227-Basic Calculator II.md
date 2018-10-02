[227. Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/description/)

思路：
1. 用栈来存放中间操作数
2. 每遇到一个新的运算符，先计算前一个运算符的计算结果
3. 加减运算不立即操作，乘除运算立即操作，这样可以**将所有的运算全部转换成加法操作**
4. 字符串遍历完毕后，栈中剩下的都是转换成加法的操作数，直接相加即可


```java
class Solution {
    public int calculate(String s) {
        Stack<Integer> opNum = new Stack<>();
        char prevOp = '+';
        int num = 0;
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (Character.isDigit(c)) {
                num = num * 10 + c - '0';
            }
            if (!Character.isDigit(c) && c != ' ' || i == s.length() - 1) {
                switch (prevOp) {
                    case '+':
                        opNum.push(num);
                        break;
                    case '-':
                        opNum.push(-num);
                        break;
                    case '*':
                        opNum.push(opNum.pop() * num);
                        break;
                    case '/':
                        opNum.push(opNum.pop() / num);
                        break;
                }
                prevOp = c;
                num = 0;
            }
        }
        int res = 0;
        while (!opNum.isEmpty()) {
            res += opNum.pop();
        }
        return res;
    }
}
```