[200. Number of Islands](https://leetcode.com/problems/number-of-islands/description/)

思路：
这题看着有点像我们小时候玩的扫雷游戏，具体的思路是：
1. 遍历数组，如果发现了一个‘1’，则首先将它标记为0，然后再递归把该点的上下左右的点全部标记成0，因为这些点都和1属于同一个岛。注意这里不是简单的标记相邻的点，而用的是递归，因为相邻的点自身有可能也连接着其他1，这样就能把连着的一大片1全部标记到了
2. 这里将‘1’标记成0有两个好处，一是所有被标记成0的点都可以看做是已经访问过的点，可以跳过；二是成片的‘1’都被标记成‘0’后这样这一片‘1’就只会被计数一次；

简单的理解就是，找到一个‘1’，就把它连着的所有‘1’都消灭掉，这样一个岛就没了，接下来就去消灭下一个岛，直到整个数组全部变成‘0’。


```java
class Solution {
    int row;
    int col;
    public int numIslands(char[][] grid) {
        if (grid.length == 0) return 0;
        int num = 0;
        row = grid.length;
        col = grid[0].length;

        for (int r = 0; r < row; r++) {
            for (int c = 0; c < col; c++) {
                if (grid[r][c] == '1') {
                    DFSMarking(grid, r, c);
                    num++;
                }
            }
        }
        return num;
    }

    private void DFSMarking(char[][] grid, int r, int c) {
        if (r < 0 || c < 0 || r >= row || c >= col || grid[r][c] == '0') return;
        grid[r][c] = '0';
        DFSMarking(grid, r - 1, c); // top
        DFSMarking(grid, r + 1, c); // down
        DFSMarking(grid, r, c - 1); // left
        DFSMarking(grid, r, c + 1); // right
    }
}
```