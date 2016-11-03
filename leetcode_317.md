### Leetcode 317.
###Shortest Distance from All Buildings
Link: https://leetcode.com/problems/shortest-distance-from-all-buildings/

略为麻烦的一道题，有几个坑连自己也没有注意到

1. 这道题存在无解的情况：也是后来才发现一定要用count去检查0字格子都必须让所有buliding都access到，才可以update最全局最优解。
2. 刚开始这道题的时候，想用 best first search去解，写一个Dijkstra出来，后来想了想，其实也没有必要这么麻烦，就直接用breath first search解了。注意的是breath first search的方法也就只有全部格子cost为1的时候才适合找最短路径，否者一定是要用best first search去找的。
3. 另外还有个细节的地方，visit要在expend的时候(既是放入queue前)去更新，否则会有会出现重复访问的问题， 但是count也是这里出错的。
4. 还需要做琢磨 是 expand还是poll后更新visit和cost，要清楚semantic(语意)

```java
public class Solution {
    private class Cell {
        int x;
        int y;
        int cost;
        
        public Cell(int x, int y, int cost) {
            this.x = x;
            this.y = y;
            this.cost = cost;
        }
    }

    public int shortestDistance(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[][]  allCost = new int[m][n];
        int result = Integer.MAX_VALUE;
        int numOfB = 0;
        int[][] count = new int[m][n];
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {
                    boolean[][] visit = new boolean[m][n];
                    int[][] oneCost = new int[m][n];
                    bfs(grid, oneCost, visit, i, j, count);
                    addMatrix(allCost, oneCost, grid);
                    numOfB++;
                }
            }
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (allCost[i][j] > 0 && count[i][j] == numOfB) {
                    result = Math.min(result, allCost[i][j]);
                }
            }
        }
        return result == Integer.MAX_VALUE ? -1 : result;
    }
    
    public void bfs (int[][] grid, int[][] one, boolean[][] visit, int x, int y, int[][] count) {
        int m = grid.length, n = grid[0].length;
        int[] dx = {0, 0, -1, 1};
        int[] dy = {-1, 1, 0, 0};
        LinkedList<Cell> queue = new LinkedList<Cell>();
        queue.offer(new Cell(x, y, 0));
        int expended = 0;
        
        while (!queue.isEmpty()) {
            Cell curr = queue.poll();
            one[curr.x][curr.y] = curr.cost;
            
            for (int i = 0; i < 4; i++) {
                int nx = curr.x + dx[i];
                int ny = curr.y + dy[i];
                
                if (nx >= 0 && ny >= 0 && nx < m && ny < n && !visit[nx][ny] && grid[nx][ny] == 0) {
                    visit[nx][ny] = true;
                    queue.offer(new Cell(nx, ny, curr.cost + 1));
                    count[nx][ny] += 1;
                }
            }
        }
    }
    
    public void addMatrix (int[][] one, int[][] two, int[][] grid) {
        int m = grid.length, n = grid[0].length;
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                one[i][j] += two[i][j];
            }
        }
    }
}
```