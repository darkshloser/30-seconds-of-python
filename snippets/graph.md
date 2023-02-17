---
title: Python Data Structures - Graph
tags: math,class,dictionary,list
cover: blog_images/purple-flower-macro-1.jpg
excerpt: A graph is a data structure consisting of a set of vertices connected by a set of edges.
firstSeen: 2023-02-17T12:21:39+02:00
---

### Definition

A graph is a data structure consisting of a set of nodes or vertices and a set of edges that represent connections between those nodes. Graphs can be directed or undirected, while their edges can be assigned numeric weights.

![Graph visualization](./blog_images/ds-graph.png)

Each node in a graph data structure must have the following properties:

- `key`: The key of the node
- `value`: The value of the node

Each edge in a graph data structure must have the following properties:

- `a`: The starting node of the edge
- `b`: The target node of the edge
- `weight`: An optional numeric weight value for the edge

The main operations of a graph data structure are:

- `addNode`: Inserts a new node with the specific key and value
- `addEdge`: Inserts a new edge between two given nodes, optionally setting its weight
- `removeNode`: Removes the node with the specified key
- `removeEdge`: Removes the edge between two given nodes
- `findNode`: Retrieves the node with the given key
- `hasEdge`: Checks if the graph has an edge between two given nodes
- `setEdgeWeight`: Sets the weight of a given edge
- `getEdgeWeight`: Gets the weight of a given edge
- `adjacent`: Finds all nodes for which an edge exists from a given node
- `indegree`: Calculates the total number of edges to a given node
- `outdegree`: Calculates the total number of edges from a given node

### Implementation

```py
class Graph:
    def __init__(self, directed=True):
        self.directed = directed
        self.nodes = []
        self.edges = {}

    def addNode(self, key, value=None):
        self.nodes.append({'key': key, 'value': value or key})

    def addEdge(self, a, b, weight=None):
        self.edges[(a, b)] = {'a': a, 'b': b, 'weight': weight}
        if not self.directed:
            self.edges[(b, a)] = {'a': b, 'b': a, 'weight': weight}

    def removeNode(self, key):
        self.nodes = [n for n in self.nodes if n['key'] != key]
        for (a, b), edge in list(self.edges.items()):
            if a == key or b == key:
                del self.edges[(a, b)]

    def removeEdge(self, a, b):
        del self.edges[(a, b)]
        if not self.directed:
            del self.edges[(b, a)]

    def findNode(self, key):
        return next((n for n in self.nodes if n['key'] == key), None)

    def hasEdge(self, a, b):
        return (a, b) in self.edges

    def setEdgeWeight(self, a, b, weight):
        self.edges[(a, b)]['weight'] = weight
        if not self.directed:
            self.edges[(b, a)]['weight'] = weight

    def getEdgeWeight(self, a, b):
        return self.edges[(a, b)]['weight']

    def adjacent(self, key):
        return [edge['b'] for edge in self.edges.values() if edge['a'] == key]

    def indegree(self, key):
        return sum(1 for edge in self.edges.values() if edge['b'] == key)

    def outdegree(self, key):
        return sum(1 for edge in self.edges.values() if edge['a'] == key)
```

- Create a `class` with a `constructor` that initializes an empty array, `nodes`, and a `Map`, `edges`, for each instance. The optional argument, `directed`, specifies if the graph is directed or not.

- Define an `addNode()` method, which uses `Array.prototype.push()` to add a new node in the `nodes` array.
- Define an `addEdge()` method, which uses `Map.prototype.set()` to add a new edge to the `edges` Map, using `JSON.stringify()` to produce a unique key.
- Define a `removeNode()` method, which uses `Array.prototype.filter()` and `Map.prototype.delete()` to remove the given node and any edges connected to it.
- Define a `removeEdge()` method, which uses `Map.prototype.delete()` to remove the given edge.
- Define a `findNode()` method, which uses `Array.prototype.find()` to return the given node, if any.
- Define a `hasEdge()` method, which uses `Map.prototype.has()` and `JSON.stringify()` to check if the given edge exists in the `edges` Map.
- Define a `setEdgeWeight()` method, which uses `Map.prototype.set()` to set the weight of the appropriate edge, whose key is produced by `JSON.stringify()`.
- Define a `getEdgeWeight()` method, which uses `Map.prototype.get()` to get the eight of the appropriate edge, whose key is produced by `JSON.stringify()`.
- Define an `adjacent()` method, which uses `Map.prototype.values()`, `Array.prototype.reduce()` and `Array.prototype.push()` to find all nodes connected to the given node.
- Define an `indegree()` method, which uses `Map.prototype.values()` and `Array.prototype.reduce()` to count the number of edges to the given node.
- Define an `outdegree()` method, which uses `Map.prototype.values()` and `Array.prototype.reduce()` to count the number of edges from the given node.

```py
g = Graph()

g.addNode('a')
g.addNode('b')
g.addNode('c')
g.addNode('d')

g.addEdge('a', 'c')
g.addEdge('b', 'c')
g.addEdge('c', 'b')
g.addEdge('d', 'a')

list(map(lambda x: x['value'], g.nodes))  # ['a', 'b', 'c', 'd']
list(map(lambda x: f"{x['a']} => {x['b']}", g.edges.values()))
# ['a => c', 'b => c', 'c => b', 'd => a']

g.adjacent('c')  # ['b']

g.indegree('c')  # 2
g.outdegree('c')  # 1

g.hasEdge('d', 'a')  # True
g.hasEdge('a', 'd')  # False

g.removeEdge('c', 'b')

list(map(lambda x: f"{x['a']} => {x['b']}", g.edges.values()))
# ['a => c', 'b => c', 'd => a']

g.removeNode('c')

list(map(lambda x: x['value'], g.nodes))  # ['a', 'b', 'd']
list(map(lambda x: f"{x['a']} => {x['b']}", g.edges.values()))
# ['d => a']

g.setEdgeWeight('d', 'a', 5)
g.getEdgeWeight('d', 'a')  # 5
```
