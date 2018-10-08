[752. Open the Lock](https://leetcode.com/problems/open-the-lock/description/)

这是一道典型的广度优先搜素的题目，我们先来看看广度优先搜索的模板：
```java
/**
 * Return the length of the shortest path between root and target node.
 */
int BFS(Node root, Node target) {
    Queue<Node> queue;  // store all nodes which are waiting to be processed
    Set<Node> used;     // store all the used nodes
    int step = 0;       // number of steps neeeded from root to current node
    // initialize
    add root to queue;
    add root to used;
    // BFS
    while (queue is not empty) {
        step = step + 1;
        // iterate the nodes which are already in the queue
        int size = queue.size();
        for (int i = 0; i < size; ++i) {
            Node cur = the first node in queue;
            return step if cur is target;
            for (Node next : the neighbors of cur) {
                if (next is not in used) {
                    add next to queue;
                    add next to used;
                }
            }
            remove the first node from queue;
        }
    }
    return -1;          // there is no path from root to target
}
```
思路：
+ 由于每一步只能移动一位，所以从`0000`到下一步共有8种可能。这八种可能同属于一步，我们把它放在一层。接着我们遍历这一层，这一层的每一个元素往下一步也有8种可能，我们把它加入第三层，所以每层遍历的节点数是 1 -> 8 -> 64 -> 512 ...
+ 当然每层遍历的元素要跳过deadends, 以及已经遍历过的节点，因为可能会有重复
+ 当最后某一层出现了目标值，则遍历结束，返回遍历的层数。


做法：
+ 用两个Set来代替queue, 一个是begin，一个是temp，这样避免了每层迭代都要计算队列大小的问题，同时，也避免了队列元素的出队
+ 每次遍历当前层（begin）, 将下一层的元素添加到temp中；遍历结束后再将begin指向下一层。
+ deads集合除了用来存放deadends，也可以用来存放已经遍历过的节点。
```java
class Solution {
    public int openLock(String[] deadends, String target) {
        Set<String> begin = new HashSet<>();
        Set<String> deads = new HashSet<>(Arrays.asList(deadends));

        begin.add("0000");
        int level = 0;

        while(!begin.isEmpty()) {
            Set<String> temp = new HashSet<>();
            for(String s : begin) { //遍历当前层
                if(s.equals(target)) return level;
                if(deads.contains(s)) continue;
                deads.add(s); // visited
                StringBuilder sb = new StringBuilder(s);
                for(int i = 0; i < 4; i ++) { // 添加下一层的节点
                    char c = sb.charAt(i);
                    String next = sb.substring(0, i) + (c == '9' ? 0 : c - '0' + 1) + sb.substring(i + 1);
                    String prev = sb.substring(0, i) + (c == '0' ? 9 : c - '0' - 1) + sb.substring(i + 1);
                    if(!deads.contains(next))
                        temp.add(next);
                    if(!deads.contains(prev))
                        temp.add(prev);
                }
            }
            begin = temp;
            level++;
        }
        return -1;
    }
}
```

优化：
+ 从两侧遍历
```java
class Solution {
    public int openLock(String[] deadends, String target) {
        Set<String> begin = new HashSet<>();
        Set<String> end = new HashSet<>();
        Set<String> deads = new HashSet<>(Arrays.asList(deadends));

        begin.add("0000");
        end.add(target);
        int level = 0;

        while (!begin.isEmpty() && !end.isEmpty()) {
            Set<String> temp = new HashSet<>();
            for (String s : begin) {
                if (end.contains(s)) return level; // 首尾相遇
                if (deads.contains(s)) continue;
                deads.add(s); // visited
                StringBuilder sb = new StringBuilder(s);
                for (int i = 0; i < 4; i++) {
                    char c = sb.charAt(i);
                    String next = sb.substring(0, i) + (c == '9' ? 0 : c - '0' + 1) + sb.substring(i + 1);
                    String prev = sb.substring(0, i) + (c == '0' ? 9 : c - '0' - 1) + sb.substring(i + 1);
                    if (!deads.contains(next))
                        temp.add(next);
                    if (!deads.contains(prev))
                        temp.add(prev);
                }
            }
            begin = end;
            end = temp;
            level++;
        }
        return -1;
    }
}
```