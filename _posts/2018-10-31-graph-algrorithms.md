---
layout: post
title: "Graph Algorithms<sup>TM</sup>"
description: "Graph Algorithms<sup>TM</sup> for undirected (weighted) graphs"
date: 2018-10-31
tags: [algorithms]
comments: true
share: true
---

Graph Algorithms<sup>TM</sup>

1. [Graph Terminology](#a1)
2. [Representation](#a2)
3. [Graph Traversal and Their Applications](#a3)
4. [Shortest Path](#a4)

### <a name="a1"></a>Graph Terminology

**Vertices(V)**: Objects in a graph. Also known as node.    

**Edges(E)**: A set of pairs of vertices. In a weighted graph, each edge is assigned a weight. A graph is directed if the edges can be traversed in one direction only.

**Graph**: A pair of sets (V, E).

**Path**: A walk is a sequence of edges, where each successive pair of edges shares one vertex. A walk if it visits each vertex at most once. The length of a path is the number of edges in it. 

**Simple Graph**: Loops are edges from a vertex to itself. Parallel Edges are multiple edges with the same endpoints. Simple graphs are Graphs without loops and parallel edges. Multi-graphs are Non-simple graphs.

**Neighbor**: If there is an edge connects vertices {u, v}, u and v are neighbors. A graph is regular if the degree of every node is a constant d. A graph is complete if the degree of every node is n−1, i.e., the graph contains all possible edges between the nodes.

**Degree**: How many neighbors a node has. 

**Cycle**: A path that starts and ends at the same vertex, and has at least one edge.
**Forests**: Graph without cycle for undirected acyclic graph. 

**Spanning Tree**: A subgraph of graph G that is a tree and contains every vertex of G. A graph has a spanning tree if and only if it is connected.

**Directed Graph**: Assuming we have a directed edge u -> v, then
**Predecessor**: u is a predecessor of v 
**Successor**: v is a successor of u
**In-degree**: The number of predecessor for a node 
**Out-Degree**: The number of successor for a node
**Strongly Connected**: There is a directed path from any vertex to any other
**Acyclic**: The graph does not has any cycle.

* **Connectivity**:
  * **Subgraph**: Given a graph G = (V, E), a subgraph for G is G' = (V', E') where V' <= V and E' <= E. 
  * **Connected Graph**: There is a path between any 2 arbitrary vertices. 
  * **Component**: A maximal connected subgraph in a disconnected graph. 
  * **Tree**: A connected graph that consists of n nodes and n−1 edges. There is a unique path between any two nodes of a tree.

* **Coloring**:
  * In a coloring of a graph, each node is assigned a color so that no adjacent nodes have the same color.     
  * Bipartite: A graph is bipartite if it is possible to color it using two colors. It turns out that a graph is bipartite exactly when it does not contain a cycle with an odd number of edges.     


### <a name="a2"></a>Graph Representation

A graph can be represented using either an adjacency matrix or an adjacency list. An adjacency matrix is good for checking and updating the connectivity between 2 vertices. An adjacency list is helpful for saving space when the graph is sparse. 

In Python, we usually use a dict to store a graph, with vertex as key and a list of neighbors as values. If the edges has weight, we can use a tuple of `(neighbor, weight)` to represent it. 

### <a name="a3"></a>Graph Traveral and Their Applications

* **Depth-first search (DFS)**
    * Visits all the nodes reachable from v in depth-first order. Use recursion or stacks for implementation. 
    * The time complexity of depth-first search is `O(n+ m)` where n is the number of nodes and m is the number of edges, because the algorithm processes each node and edge once.
    * Pseudocode:
        ```
        RecursiveDFS(v, visited):
            Mark v as visited
            for each edge v → u:
                If u is not visited:
                    RecursiveDFS(u, visited)
        ```
        ```
        IterativeDFS(s):
        Push(s)
        while the stack is not empty
            v ← Pop
            Mark v as visited
            for each edge v → u:
                If u is not visited:
                    Push(u)
        ```

* **Breadth-first search (BFS)**
    * Visits all the nodes reachable from v in breadth-first order.
    * Like in depth-first search, the time complexity of breadth-first search is `O(n+ m)`, where n is the number of nodes and m is the number of edges.
    * Pseudocode:
        ```
        BFS(s):
            Initialize a queue Q
            Mark s as visited and push it to Q
            While Q is not empty:
                Take the front element (v) of Q
                For each edge v → u:
                    If u is not visited, mark it as visited and push it to Q
        ```

* **Applications (with undirected graphs)**: 
    * **Connectivity Check**:     
        A graph is connected if there is a path between any two nodes of the graph. Thus, we can check if a graph is connected by starting at an arbitrary node and finding out if we can reach all other nodes.
    * **Finding Cycles**:     
        A graph contains a cycle if during a graph traversal, we find a node whose neighbor (other than the previous node in the current path) has already been visited.    
        Another way to find out whether a graph contains a cycle is to simply calculate the number of nodes and edges in every component. If a component contains c nodes and no cycle, it must contain exactly c −1 edges (so it has to be a tree). If there are c or more edges, the component surely contains a cycle.
    * **Bipartiteness Check**:    
        A graph is bipartite if its nodes can be colored using two colors so that there are no adjacent nodes with the same color.    
        The idea is to color the starting node blue, all its neighbors red, all their neighbors blue, and so on. If at some point of the search we notice that two adjacent nodes have the same color, this means that the graph is not bipartite. Otherwise the graph is bipartite and one coloring has been found.

### <a name="a4"></a>Shortest Path

| Syntax      | Bellman–Ford | Dijkstra | Floyd–Warshall |
| ----------- | ----------- | ----------- | ----------- | 
| Source Type      | Single Source | Single Source | Multi Source
| Cycle   | Fail on Negative Cycle        | |
| Negative Edge  | Can detect Negative Edge        |  Fail on Negative Edge |

* **Bellman–Ford algorithm**
    * The time complexity of the algorithm is `O(n*m)`, because the algorithm consists of n−1 rounds and iterates through all m edges during a round. 
    * If there are no negative cycles in the graph, all distances are final after n −1 rounds, because each shortest path can contain at most n−1 edges.
    * Implementation:
        ```python
        # Nodenum: [(neighbor, weight), ...]
        g1 = {
            0: [(1, 1), (2, 7)],
            1: [(5, 15), (3, 9)],
            2: [(4, 4)],
            3: [(5, 5), (4, 10)],
            4: [(5, 3)],
            5: []
        }

        def bellmanFord(self, graph, source):
            distArray = [float("inf")] * len(graph)
            distArray[source] = 0
            for i in range(len(graph) - 1):
                # i is the start node for each connection
                for dest, weight in graph[i]:
                    print(i, dest, weight, distArray)
                    if distArray[i] != float("inf") and distArray[i] + weight < distArray[dest]:
                        distArray[dest] = distArray[i] + weight
            
            # If we want to detect negative cycle here
            for i in range(len(graph) - 1):
                for dest, weight in graph[i]:
                    if distArray[i] != float("inf") and distArray[i] + weight < distArray[dest]:
                        print("Negative Cycle!")
                        return
            
            return distArray
        ```

* **Dijkstra’s algorithm**
    * The time complexity of the above implementation is O(n+ mlogm), because the algorithm goes through all nodes of the graph and adds for each edge at most one distance to the priority queue.
    * Implementation:
        ```python
        # Nodenum: [(neighbor, weight), ...]
        g1 = {
            0: [(1, 1), (2, 7)],
            1: [(5, 15), (3, 9)],
            2: [(4, 4)],
            3: [(5, 5), (4, 10)],
            4: [(5, 3)],
            5: []
        }

        def dijkstra(self, graph, source):
            # min distance to source
            distanceArr = [float("inf")] * len(graph)
            # whether the node of index is visited yet
            inSetYetArr = [False] * len(graph)
            distanceArr[source] = 0
            
            for _ in range(len(graph) - 1):
                # Pick the min distance node from the set
                #   of nodes not yet processed
                minIdx = self.minDistanceIdx(distanceArr, inSetYetArr)
                inSetYetArr[minIdx] = True

                for dest, weight in graph[minIdx]:
                    if not inSetYetArr[dest] and weight != 0 and distanceArr[minIdx] != float("inf") and distanceArr[minIdx] + weight < distanceArr[dest]:
                        distanceArr[dest] = distanceArr[minIdx] + weight

            return distanceArr

        def minDistanceIdx(self, distanceArr, inSetYetArr):
            min, minIdx = float("inf"), -1
            for i in range(len(distanceArr)):
                if not inSetYetArr[i] and distanceArr[i] <= min:
                    min, minIdx = distanceArr[i], i
            return minIdx
        ```

* **Floyd–Warshall algorithm**
  * The time complexity of the algorithm is O(n^3), because it contains three nested loops that go through the nodes of the graph.
  * Implementation: 
  ```python
    # Nodenum: [(neighbor, weight), ...]
    g1 = {
        0: [(1, 1), (2, 7)],
        1: [(5, 15), (3, 9)],
        2: [(4, 4)],
        3: [(5, 5), (4, 10)],
        4: [(5, 3)],
        5: []
    }

    def adjacencyListToMatrix(self, graph):
        res = [[float("inf")] * len(graph) for _ in range(len(graph))]
        for i in range(len(graph)):
            res[i][i] = 0
        for i in range(len(graph)):
            for dest, weight in graph[i]:
                res[i][dest] = weight
        return res

    def floydWarshall(self, graph):
        dist = self.adjacencyListToMatrix(graph)
        for k in range(len(graph)): 
            for i in range(len(graph)): 
                for j in range(len(graph)): 
                    # Assuming k is the current node which connects node i and node j
                    # dist[i][k] represents cost of i -> k
                    # dist[k][j] represents cost of k -> j
                    dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
        return dist
  ```