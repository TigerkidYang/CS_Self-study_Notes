# Graph: DFS and BFS

I write this note mainly based on this slide from UCB CS61B:

[Lec21](https://docs.google.com/presentation/d/1qfq21PwxzNkVyRZlir1-cN5ecI2MDpJ7uamR0tJMluE/edit?usp=sharing)
[Lec22](https://docs.google.com/presentation/d/1v5pLls6n5weicMenk_CXxNVC-9O-Uic1usRQYrdxBN0/edit?usp=sharing)

Pictures in the note are all from the slide.

## Motivation: Traversing, not Hierarchical

Let me remind me something about the tree. You should remember the tree if you have been through all those shits we used to talk about in the previous notes.

A **tree** consists of:
- A set of nodes.
- A set of edges that connect those nodes.
    - Constraint: There is exactly one path between any two nodes.

A **rooted tree** is a tree where we’ve chosen one node as the “root”.
- Every node N except the root has exactly one parent, defined as the first node on the path from N to the root.
- A node with no child is called a leaf.

We have done a lot of things with trees, such as search trees, heaps and disjoint sets. It is a very common concept, people see and use it every day. 

Sometimes we need to iterate over it, or people say, **traverse** it. Imagine you are an emperor, you have a lot of wives and children. You must have this data structure in your computer to record stuffs about them. Tree is a great choice, easy to keep track of each level of wives and who is who's child. But one day, you need to print a family list, you must find a way to traverse the tree, and must in order.

This is why people need an algorithm to traverse the tree. I am totally not lying to you.

What we use is called **Depth First Traversals**. I will use this tree in the pic below to show you.

![tree_to_traverse](tree_to_traverse.png)

The rough idea is to traverse the deep nodes, then traverse the shallow ones. In detail, there are three ways to do this, **Preorder**, **Inorder** and **Postorder**.

The **Preorder** is that we 'visit' a node and them traverse its children. For our example, start at D, print it, then we traverse its children and print, children have children, so also visit and traverse. Get to A, there is no more children, so print, and traverse another children, do stuff like this. 

I know you are not as smart as I am to see this clearly only by words. I will give you this code for you to understand. And the print order is `D B A C F E G`, you can see the code and try imagining it to see if you get the same.

```java
preOrder(BSTNode x) {
    if (x == null) return;
    print(x.key)
    preOrder(x.left)
    preOrder(x.right)
}
```

The **Inorder** traversal is traverse the left child, then visit the node, and then the right child. For our example, the print order is `A B C D E F G`.

```java
inOrder(BSTNode x) {
    if (x == null) return;
    inOrder(x.left)
    print(x.key)
    inOrder(x.right)
}
```

The **Postorder** traversal is traverse the left child, then the right child, and then visit the node. For our example, the print order is `A C B E G F D`.

```java
postOrder(BSTNode x) {
    if (x == null) return;
    postOrder(x.left)
    postOrder(x.right)
    print(x.key)
}
```

These codes can make the computer do the stuff for you correctly. But sometimes you may need to use your very unreliable brain and eyes to do this, that may cause tragic results. To save the world from you, I have this trick for you.

You just trace a path around the graph, from the top and do counter-clockwise. 

For Preorder, visit every time we pass the **LEFT** of a node.

For Inorder, visit every time we cross the **BOTTOM** of a node.

For Postorder, visit every time we pass the **RIGHT** of a node.

See this pic below for a postorder case example:

![postorder_visual_trick](postorder_visual_trick.png)

What goog are all these traversals?

Well, preorder is good for printing directory listing.See this pic below:

![preorder_directory_listing](preorder_directory_listing.png)

Postorder is good for gethering all the file sizes up. See this pic below:

![postorder_file_size](postorder_file_size.png)

So we know about the tree traversal. But it still has some problems. The most important, trees are perfect for hierarchical data, but not everything in the real world is hierarchical, right? For example, the metro map of a city, it is not hierarchical, and it's not a tree because it probably has a lot of cycles.

This is why we need graph.

## Graph Definition

A graph consists of:
- A set of nodes.
- A set of zero or more edges, each of which connects two nodes.

I think even you can see that all the trees are graphs.

A **simple graph** is a graph with:
- No edges that connect a vertex to itself, i.e. no “loops”.
- No two edges that connect the same vertices, i.e. no “parallel edges”.

See this pic below if not clear:

![simple_graph_or_not](simple_graph_or_not.png)

For simplicity, when we say graph, we usually mean simple graph, unless explicitly stated otherwise.

We have some terminology for graphs here:

- Graph:
    - Set of **vertices**, a.k.a. nodes.
    - Set of **edges**: Pairs of vertices.
    - Vertices with an edge between are **adjacent**.
    - Optional: Vertices or edges may have **labels** (or **weights**).
- A **path** is a sequence of vertices connected by edges.
    - A **simple path** is a path without repeated vertices.
- A **cycle** is a path whose first and last vertices are the same.
    - A graph with a cycle is **‘cyclic’**.
- Two vertices are **connected** if there is a path between them. If all vertices are connected, we say the graph is connected.
- And the edges are **directed** or **undirected**. 

See this pic below to see graph types:

![graph_types](graph_types.png)

For the metro maps, of course they are connected, undirected and cyclic.

## Depth First Traversal

There are a lot of intresting problems about graph. Some well known graph problems and their common names:
- **s-t Path**. Is there a path between vertices s and t?
- **Connectivity**. Is the graph connected, i.e. is there a path between all vertices?
- **Biconnectivity**. Is there a vertex whose removal disconnects the graph?
- **Shortest s-t Path**. What is the shortest path between vertices s and t?
- **Cycle Detection**. Does the graph contain any cycles?
- **Euler Tour**. Is there a cycle that uses every edge exactly once?
- **Hamilton Tour**. Is there a cycle that uses every vertex exactly once?
- **Planarity**. Can you draw the graph on paper with no crossing edges?
- **Isomorphism**. Are two graphs isomorphic (the same graph in disguise)?

We are going to solve a classic s-t connectivity problem. Which is basically just want you to figure out if there is a path between vertices s and t.

I think even you can see that this require us to traverse the graph somehow. So that we can make connect(s, t) work.

A very obvious approach is just do this recursion. Check if `s == t`, if so, ruturn ture. Otherwise, if connect(v, t) for any neighbor v of s, return ture. Or return false.

This is a easy classic recursion, I mean those shits that even you can figure out. It certainly cause big problems. It can be easily caught in a infinity loop.

See this pic if not clear:

![recursion_loop_problem](recursion_loop_problem.png)

To fix this, we need to mark nodes to prevent multiple visits. So the algorithm is like this:

connected(s, t):
- Mark s.
- Does s == t? If so, return true.
- Otherwise, if connected(v, t) for any unmarked neighbor v of s, return true.
- Return false.

See this gif below for a s-t connectivity demo:

![s-t_connectivity_demo](s-t_connectivity_demo.gif)

Basically, the idea is exploring one neighbor's entire subgraph before moving on to the next neighbor. It's what people call **Depth First Traversal**.

The depth first search is a very common algorithm in graph, it's powerful in many graph problems. For example, the s-t path problem, we have the Depth First Path algorithm.

dfs(v):
- Mark v.
- For each unmarked adjacent vertex w:
    - set edgeTo[w] = v. 
    - dfs(w)

See this gif below for a s-t path demo:

![dfp_demo](dfp_demo.gif)

By the way, what we do here is actually DFS Preorder, we can also do DFS Postorder. The different is do you do the action before or after DFS calls to the neighbors.



