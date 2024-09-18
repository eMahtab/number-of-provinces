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
    public int findCircleNum(int[][] isConnected) {
        int n = isConnected.length;
        boolean[] visited = new boolean[n];
        int components = 0;
        for(int vertex = 0; vertex < n; vertex++) {
            if(!visited[vertex]) {
                visited[vertex] = true;
                components++;
                visitNeighbors(vertex, isConnected, visited);
            }
        }
        return components;
    }
    
    private void visitNeighbors(int vertex, int[][] isConnected, boolean[] visited) {
        for(int v = 0; v < isConnected.length; v++) {
            if(isConnected[vertex][v] == 1 && !visited[v]) {
                visited[v] = true;
                visitNeighbors(v, isConnected, visited);
            }
        }
    }
}

```

### Caution : StackOverflowError
Note that we need the `visited` array to mark the nodes/vertices that we have already visited otherwise we will run into StackOverflowError.
Even though its naive but lets say this loud, one vertex can not be part of two different graph components.

![StackOverflowError](graph.JPG?raw=true "StackOverflowError")

## Implementation 2 : BFS
```java
class Solution {
    public int findCircleNum(int[][] isConnected) {
        if(isConnected == null || isConnected.length == 0)
          return 0;

        int province = 0;
        int totalNodes = isConnected.length;
        Set<Integer> visited = new HashSet<>();
        Queue<Integer> queue = new ArrayDeque<>();
        for(int vertex = 0; vertex < totalNodes; vertex++){
            if(!visited.contains(vertex)) {
                province++;
                queue.add(vertex);
                while(!queue.isEmpty()) {
                    int u = queue.remove();
                    visited.add(u);
                    for(int v = 0; v < totalNodes; v++) {
                        if( u == v || visited.contains(v))
                           continue;
                        if(isConnected[u][v] == 1) {
                            queue.add(v);
                        }
                    }
                }
            }
        }

        return province;  
    }
}

```
