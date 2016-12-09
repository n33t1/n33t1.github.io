## Trees

 - A tree is a connected, acyclic, undirected graph.
 - Binary Tree
    - A binary tree T is a finite set of nodes with one of the following
properties:  
        - A. T is a tree if the set of nodes is empty. 
        - B. The set consists of a root and exactly two distinct binary trees
Tleft and Tright
    - Three Operations: 
        - N: Visit a node (the root of the current tree) and perform some action.  
        - L: Make a recursive descent to the left subtree 
        - R: Make a recursive descent to the right subtree
    - Binary Tree Traversal
        - Visit every node, without skipping any node. 
        - There are six simple recursive algorithms for tree traversal:
            - In-order: RNL, LNR 
            - ```
                template <typename T>
                void inOrderOutput(tnode<T> *t, const string& separator = “ “){
                 // recursive scan In Order ends in an empty subtree
                    if (t != NULL) {
                    inOrderOutput<T>(t->left,separator); // L step
                    cout << t->nodeValue << separator; // N step
                    inOrderOutput<T>(t->right,separator); // R step
                    }
                }
                ```
            - Post-order: RLN, LRN 
            - ```
                template <typename T>
                void postOrderOutput(tnode<T> *t, const string& separator = “ “){
                 // recursive scan Post Order ends in an empty subtree
                    if (t != NULL) {
                        postOrderOutput<T>(t->left,separator); // L step
                        postOrderOutput<T>(t->right,separator); // R step
                        cout << t->nodeValue << separator; // N step
                    }
                }
                ```
            - Pre-order: NRL, NLR
             - ```
                template <typename T>
                void preOrderOutput(tnode<T> *t, const string& separator = “ “){
                 // recursive scan Pre Order ends in an empty subtree
                    if (t != NULL) {
                        cout << t->nodeValue << separator; // N step
                    preOrderOutput<T>(t->left,separator); // L step
                    preOrderOutput<T>(t->right,separator); // R step
                    }
                }
                ```
    - Tree applications
        - Deleting a tree
            - Use recursive tree traversal to de-allocate the nodes in a binary tree.
            - Post Order: LRN ensures that the parent is deleted last to avoid
memory leaks.
             - ```
                template <typename T>
                void deleteTree(tnode<T> *t) {
                    if(t != NULL) {
                        deleteTree(t->left);
                        deleteTree(t->right);
                        delete t;
                    }
                }
                ```
        - Counting the leaves
            - Scan the tree to count the leaves:
                - Keep a counter with the number of leaves 
                - Scan the tree in any order, increment counter at leaf
                - ```
                template <typename T>
                void countLeaf(tnode<T> *t, int & count) {
                if (t != NULL) {
                     if (t->left == NULL && t->right == NULL)
                            count++; // found a leaf
                            countLeaf(t->left, count);
                            countLeaf(t->right, count);
                    }
                }
                ```
        - Finding the depth of a tree
            - Scan the tree to find the deepest leaf:
             - ```
                template <typename T>
                void depth(tnode<T> *t) {
                    int depthVal, depthLeft, depthRight;
                    if(t == NULL)
                        depthVal = -1; // depth of an empty tree
                    else {
                        depthLeft = depth(t->left);
                        depthRight = depth(t->right);
                        depthVal = 1 + (depthLeft > depthRight ? depthLeft: depthRight);
                 }
                    return depthVal;
                }
                ```
    - Level order
        - The recursive traverse methods go down and up the tree.
        - Sometimes, we want to go by levels instead
        - Iterative Level Order Scan 
        - Uses a queue as an intermediate container
        - ```
               template <typename T>
                void levelOrderOutput(tnode<T> *t, const string& separator = “ “) {
                    // store siblings in queue
                    queue <tnode<T> *> q;
                    tnode<T> *p;
                    // insert the root
                    q.push(t);
                    // continue breadth first
                    while (!q.empty()) {
                        // delete the element from the queue
                         p = q.front();
                         q.pop();
                        cout << p->nodeValue << separator;
                        if (p->left != NULL)
                            q.push(p->left); // insert left child
                        if (p->right != NULL)
                            q.push(p->right); // insert right child
                        }
                }
                ```

## Spanning Trees

 - IsConnected()
    - Returns true iff the graph is connected. 
    - Brute force: Given a pair (X,Y) we can check if Y is reachable from X
by traversing the graph and checking if we reach Y.
 - IsCyclic()
    - Returns true iff there are no cycles. 
    - During traversal, if we can see an already visited neighbor (other
than predecessor), we know there must be a cycle.
 - Minimum spanning trees
    - Every time we add 1 edge to a spanning tree, we create a cycle. 
    - Every time we remove 1 edge from a spanning tree, we get a
disconnected graph.
    - A tree has exactly E = N - 1 edges
    - A cut (S, V-S) of an undirected graph G = (V, E ) is a partition of V. 
    - An edge (u,v) crosses the cut (S, V-S) if one of its endpoints is in S and the other is in V-S.
    - A cut respects a set A of edges if no edge in A crosses the cut. 
    - An edge is a light edge crossing a cut if its weight is the minimum of
any edge crossing the cut.


## Bellman-Ford Algorithm

 - Single Source Shortest Paths
 - It can handle negative weights (local)
 - Returns false if it contains reachable negative cycles
 
```
Bellman-Ford(G,w,s)
    Init-Single-Source(G,s)
    for i=1 to |G.V| - 1
        for each edge (u,v) in G.E
            Relax(u,v,w)
        for each edge (u,v) in G.E
            if v.d > u.d + w(u,v)
                return FALSE
            return TRUE 
```

## Dijkstra’s Algorithm

 - Also a Single Source Shortest Paths alg
 - It requires all positive weights (global)
 - It has lower complexity than Bellman Ford’s alg.
 
```
Dijkstra(G,w,s)
    Init-Single-Source(G,s)
    S = empty
    Q = G.V
    while Q !empty
        u = Extract-Min(Q)
        S = S U {u}
        for each vertex v in G.Adj(u)
            Relax(u,v,w) 
```


## Topological Sort

Call DFS(G) to compute “finishing times” v.f for each vertex. As each vertex is finished, insert it at the front of a list return the (linked) list of vertices
