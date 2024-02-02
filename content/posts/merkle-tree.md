---
title: "Implementing a Merkle tree in Python"
description: "Build a Merkle tree in Python"
draft: false
date: 2018-01-06T00:00:00-05:00
tags: [development, bitcoin]
---

Unless you've been living under a rock for the past few years you have probably heard of Bitcoin, the decentralized, peer-to-peer, public, trustless, cash system protocol. Regardless of whether or not you think bitcoin's current valuation is in a bubble or worse that it's a Ponzi scheme, it's undeniable that the protocol solves a real and old problem in distributed computing, a.k.a the [Byzantine's General Problem](https://en.wikipedia.org/wiki/Byzantine_fault_tolerance#Byzantine_Generals'_Problem). One of the underlying data structures ensuring the immutability of the blockchain is the Merkle tree.

A Merkle tree is a binary tree in which each leaf node is a hash of a block of data and each internal node is a hash of its children. In the bitcoin protocol it ensures the immutability of the set of transactions by taking the hash of each data transaction and generating a single unique root hash representing the hash of the entire set.

![merkle-tree](/img/merkletree.png)

In this post we'll look at a possible implementation of a Merkle tree in python using the typing module. If you're not familiar with typing you can take a look at my last [post]({{site.url}}/2017/12/27/typing-python.html) where I go a little more in depth on how to use this module.
The API for the Merkle tree data structure is very straightforward and consists of a single method which exposes the root hash of the tree. The constructor takes a list of objects from which we wish to build our tree.

Our Node class stores its hash and a reference to the left and right child nodes. We'll add two static methods to do the hashing using the SHA-256 algorithm. We'll apply the hashing two times for an extra level of security. Because of the way the Merkle tree is structured we need an even number of leaf nodes to build our tree. If we have an odd number of data blocks then we just hash the last one two times, duplicating the last leaf node.

---

```python
from typing import List
import typing
import hashlib

class Node:
    def __init__(self, left, right, value: str)-> None:
        self.left: Node = left
        self.right: Node = right
        self.value = value

    @staticmethod
    def hash(val: str)-> str:
        return hashlib.sha256(val.encode('utf-8')).hexdigest()

    @staticmethod
    def doubleHash(val: str)-> str:
        return Node.hash(Node.hash(val))

```

Then the Merkle tree class itself comprised of two methods for recursively building the tree, two others for printing the nodes in prefix order and the one we mentionned earlier for getting the root hash.

```python
class MerkleTree:
    def __init__(self, values: List[str])-> None:
        self.__buildTree(values)

    def __buildTree(self, values: List[str])-> None:
        leaves: List[Node] = [Node(None, None, Node.doubleHash(e)) for e in values]
        self.root: Node = self.__buildTreeRec(leaves)

    def __buildTreeRec(self, nodes: List[Node])-> Node:
        half: int = len(nodes) // 2

        if len(nodes) == 1:
            return Node(nodes[0], nodes[0], Node.doubleHash(nodes[0].value))

        if len(nodes) == 2:
            return Node(nodes[0], nodes[1], Node.doubleHash(nodes[0].value + nodes[1].value))

        left: Node = self.__buildTreeRec(nodes[:half])
        right: Node = self.__buildTreeRec(nodes[half:])
        value: str = Node.doubleHash(left.value + right.value)
        return Node(left, right, value)

    def printTree(self)-> None:
        self.__printTreeRec(self.root)

    def __printTreeRec(self, node)-> None:
        if node != None:
            print(node.value)
            self.__printTreeRec(node.left)
            self.__printTreeRec(node.right)

    def getRootHash(self)-> str:
        return self.root.value
```

We can then test our data structure by sending in a list of strings and getting its corresponding merkle tree root hash.

```python
def test()-> None:
    elems = ["Hello", "mister", "Merkle"]
    if len(elems) == 0:
        raise Exception("Your list is empty, provide at least one element.")

    mtree = MerkleTree(elems)
    print(mtree.getRootHash())
```

```bash
~$ python3.6 merkletree.py
eac44a171f264412cc5744b9e206fcfb2ce77c1e9dc64d17a56ef7cc5eb386c7
```

That's it! Feel free to try this code and improve it at will.

Bye for now!
