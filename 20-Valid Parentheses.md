[20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/description/)

思路：
很简单的题，用栈就可以实现：
```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack();
        char[] brackets = s.toCharArray();
        for(char c : brackets) {
            if (c == '(' || c == '{' || c == '[') {
                stack.push(c);
            } else {
                if(stack.isEmpty()) return false;
                char m = stack.peek();
                if(c == ')' && m == '('){
                    stack.pop();
                }else if(c == '}' && m == '{') {
                    stack.pop();
                }else if(c ==']' && m== '[') {
                    stack.pop();
                } else {
                    return false;
                }
            }
        }
        if(!stack.isEmpty()) return false;
        return true;
    }
}
```
简化版：
```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        for (char c : s.toCharArray()) {
            if (c == '(')
                stack.push(')');
            else if (c == '{')
                stack.push('}');
            else if (c == '[')
                stack.push(']');
            else if (stack.isEmpty() || stack.pop() != c)
                return false;
        }
        return stack.isEmpty();
    }
}
```