### Leetcode 286
### 286. Walls and Gates

Link: https://leetcode.com/problems/walls-and-gates/

经典的二维数组加DFS, 没什么特别的，
不过有一点优化：
```java
rooms[x][y] < dis
```
通过剪枝省略了多余的部分。

```java
public class Solution {
    public void wallsAndGates(int[][] rooms) {
        if (rooms == null || rooms.length == 0) {
            return;
        }
        int m = rooms.length, n = rooms[0].length;
        boolean[][] visit = new boolean[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (rooms[i][j] == 0) {
                    dfs(rooms, visit, i, j, 0);
                }
            }
        }
    }
    
    public void dfs (int[][] rooms, boolean[][] visit, int x, int y, int dis) {
        int m = rooms.length, n = rooms[0].length;
        
        // rooms[x][y] < dis very crucial for branch pruning
        if (x < 0 || y < 0 || x >= m || y >= n || visit[x][y] || rooms[x][y] == - 1 || rooms[x][y] < dis) {
            return;
        }
        
        rooms[x][y] = Math.min(rooms[x][y], dis);
        visit[x][y] = true;
        dfs(rooms, visit, x + 1, y, dis + 1);
        dfs(rooms, visit, x - 1, y, dis + 1);
        dfs(rooms, visit, x, y + 1, dis + 1);
        dfs(rooms, visit, x, y - 1, dis + 1);
        visit[x][y] = false;
    }
}
```

