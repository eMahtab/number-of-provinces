# Number of provinces
## https://leetcode.com/problems/number-of-provinces

There are n cities. Some of them are connected, while some are not. If city a is connected directly with city b, and city b is connected directly with city c, then city a is connected indirectly with city c.

A province is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an n x n matrix isConnected where isConnected[i][j] = 1 if the ith city and the jth city are directly connected, and isConnected[i][j] = 0 otherwise.

Return the total number of provinces.

![Number of provinces](number-of-provinces.JPG?raw=true "Number of provinces")

## Approach : 
In graph components which are connected can be reached starting from any single node of the connected group. Thus, to find the number of connected components, we start from every node which isn't visited right now and apply DFS starting with it. We increment the count of connected components for every new starting node.

## Implementation 1 : DFS

```java
class Solution {
    public int findCircleNum(int[][] graph) {
        int[] visited = new int[graph.length];
        int provinces = 0;
        for (int i = 0; i < graph.length; i++) {
            if (visited[i] == 0) {
                provinces++;
                dfs(graph, visited, i);
            }
        }
        return provinces;
    }
    
    private void dfs(int[][] graph, int[] visited, int i) {
        for (int j = 0; j < graph.length; j++) {
            if (graph[i][j] == 1 && visited[j] == 0) {
                visited[j] = 1;
                dfs(graph, visited, j);
            }
        }
    }
}

```

### Caution : StackOverflowError
Note that we need the `visited` array to mark the nodes/vertices that we have already visited otherwise we will run into StackOverflowError.
Even though its naive but lets say this loud, one vertex can not be part of two different graph components.
![StackOverflowError](graph.JPG?raw=true "StackOverflowError")


