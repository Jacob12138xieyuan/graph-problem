# graph-problem

## Undirectional graph DFS (leetcode friend circle problem)
```
arrays = [['a','b','c'], ['b','e'],['d','e','f'],['x','y'],['z','y']]
connectedGraph(arrays)

# adjacency list of undirectional graph
# {'a': ['b', 'c'], 
#  'b': ['a', 'c', 'e'], 
#  'c': ['a', 'b'], 
#  'e': ['b', 'd', 'f'], 
#  'd': ['e', 'f'], 
#  'f': ['d', 'e'], 
#  'x': ['y'], 
#  'y': ['x', 'z'], 
#  'z': ['y']}
```
use recursion
```
def connectedGraph(arrays):
    # get adjcency list
    matrix = {}
    visited = {}
    result = []
    for array in arrays:
        for i, a in enumerate(array):
            if a not in matrix:
                matrix[a] = []
                visited[a] = 0
            remain = array[:i] + array[i+1:]
            for r in remain:
                matrix[a].append(r)
    for key in matrix:
        if visited[key] == 1:
            continue
        res = []
        dfs(key,matrix,visited,res)
        result.append(res)
    return result
    
def dfs(key,matrix,visited,res):
    #print(visited)
    if visited[key] == 1:
        return
    visited[key] = 1
    res.append(key)
    neighbors = matrix[key]
    for neighbor in neighbors:
        if visited[neighbor] == 0:
            dfs(neighbor,matrix,visited,res)
```

use queue
```
from collections import deque 
def connectedGraph(arrays):
    # get adjcency list
    matrix = {}
    visited = {}
    result = []
    for array in arrays:
        for i, a in enumerate(array):
            if a not in matrix:
                matrix[a] = []
                visited[a] = 0
            remain = array[:i] + array[i+1:]
            for r in remain:
                matrix[a].append(r)
                
    for key in matrix:
        if visited[key] == 1:
            continue
        q = deque([key])
        res = []
        while q:
            node = q.popleft()
            if visited[node] == 1:
                continue
            visited[node] = 1
            res.append(node)
            neighbors = matrix[node]
            for neighbor in neighbors:
                q.append(neighbor)
        result.append(res)
    return result
```
## Friend circle
Input: <br>
[[1,1,0], <br>
 [1,1,0],<br>
 [0,0,1]]<br>
Output: 2<br>
Explanation:The 0th and 1st students are direct friends, so they are in a friend circle. 
The 2nd student himself is in a friend circle. So return 2.
```
def findCircleNum(self, M):
        res = 0
        n = len(M) # number of students
        visited = [0]*n # record if a student is visited
        for i in range(n): # iterate every student
            if visited[i]:
                continue     
            self.dfs(M, i, visited) # do dfs on this student, if all are friends, this loop only run once
            res += 1
        return res
        
    def dfs(self, M, curr, visited):
        if visited[curr]:
            return
        visited[curr] = 1 # if current student is not visited
        for i in range(len(M)): # iterate this row of M to find neighbors
            if M[curr][i] and not visited[i]: # neighbor exists and not visited
                self.dfs(M, i, visited) # do dfs on neighbor
```
