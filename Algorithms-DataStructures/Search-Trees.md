# Search Trees

## Binary Search Tree

BST is a Binary Tree such that each position `p` storing a KV pair `(k, v)` such that:
1. Keys stored in the left subtree of `p` < `k`
2. Keys stored in the right subtree of `p` > `k`

![[Pasted image 20230731125117.png]]
^ Only Keys are shown, values are omitted


### Proposition: In-order traversal

>In-order traversal of Binary Search Tree visits positions in increasing order of their keys

### Proposition: Search

> Search for a value in BST runs on O(h) time, where h is the height of the BST, also equivalent to O(log n)


### Performance

![[Pasted image 20230731125426.png]]


## Balanced Search Trees

### Rotation

![[Pasted image 20230731125552.png]]

To maintain the BST property after a rotation:
- if `x` was the left child of `y`, then after rotation `y` becomes the right child of `x`


### Tri-node Restructuring

Tri-node restructuring is a compound operation which uses either a single rotation or a double rotation to balance a tree.

#### Algorithm

Consider `x` with parent `y` and with grandparent `z` (`z` -> `y` -> `x`)

> The goal is to restructure the subtree rooted at `z` in order to reduce the overall path length to `x` and its subtrees

For tri-node restructuring, we rename `xyz` to `abc` such that in-order property is preserved (i,e `a` < `b` < `c`)

There are 4 possible orientations mapping `xyz` to `abc` as shown in screenshot below:

Tri-node restructuring replaces `z` with node identified as `b`, and makes `a` and `c` children of `z`, while maintaining in-order property

![[Pasted image 20230731130320.png]]

^ Case#3 above, when we do first rotation at `b -> c`, it transforms into Case#1, which after another rotation gives the result. Same with Case#4 and Case#2


---

## AVL Trees

An AVL tree is a BST with the following properties:

- *height-balance property*: 
	- For every position `p` of `T`, the heights of the children of `p` differ by at most 1
		- where:
			- height of a Position `p` = max number of nodes from p to a leaf, including `p`
			- thus a leaf has a height of 1


![[Pasted image 20230731145405.png]]


### Proposition: height

> The height of an AVL tree storing `n` entries is `O(log n)`


### Balancing

When we detect imblanace in an AVL tree, for example:

- *after insertion* of position `p`
	- We go up from `p` till we encounter a node `z` that is unbalanced (i.e. z's child's height are not balanced)
	- let `y` = `z`'s child with higher height
	- let `x` = `y`'s child with higher height
	- we do `trinode restructuring` on `x`
	- and then continue our search up from `x`

![[Pasted image 20230731150758.png]]


- *after deletion*
	- let `p` is the parent of the removed node
	- let `z` be the first unbalanced position going up from `p`
	- let `y` = child of `z` with larger height (note that position `y` is the child of `z` that is NOT an acestor of `p`)
	- let `x` = child of `y` with larger height
		- else: if both child of `y` have the same height:
		- then let `x` = child of `y` on the same side as `y` is to `z`
	- perform `trinode restructing` on `x`
	- continue looking up the ancestors of `z` for any height imbalance and repeat trinode-restructuring

![[Pasted image 20230731151333.png]]


### Performance

![[Pasted image 20230731151434.png]]


---

## Splay Trees

### Resources: 
- [Splay tree animation](https://yongdanielliang.github.io/animation/web/SplayTree.html) 
- [another Splay tree animation](https://cmps-people.ok.ubc.ca/ylucet/DS/SplayTree.html)


Splay trees do not enforce a logarithmic upperbound on the height of the tree, however, it is still efficient, which it derives from the process known as *splaying*, which is performed at the bottommost position `p` reached during every insertion, deletion or even a search 

Splay trees are tree version of move-to-front heuristic queues.
- Intuitively, a splay operation causes more frequently accessed elements to remain nearer to the root, thereby reducing the typical search times. 
- The surprising thing about splaying is that it allows us to guarantee a logarithmic amortized running time, for insertions, deletions, and searches.

### Splaying:

> We splay `x` by moving it to the root of BST through a sequence of restructurings

There are 3 cases to consider: (Note, we continue splaying up the tree until x becomes the root)

#### zig-zig

The node `x` and its parent y are both either left children or right children
- We promote `x`, making `y` a child of `x`, and `z` a child of `y`, while maintaining in-order property

Steps
- rotate on `y` first
- then rotate on `x`

![[Pasted image 20230731155042.png]]


#### zig-zag

One of `x` and `y` is a right child, and the other is left child.
- we promote `x` by making `x` have `y` and `z` as its children, maintaining in-order relationships

Steps:
- double rotation on `x`

![[Pasted image 20230731155215.png]]


#### zig

In this case, `x` does not have a grandparent

Steps:
- perform a single rotation on `x`

![[Pasted image 20230731155437.png]]


### Example of Splaying

![[Pasted image 20230731155640.png]]

![[Pasted image 20230731155647.png]]

### When to Splay

- When searching for key `k`:
	- if `k` is found at position `p`
		- we splay `p`
	- else:
		- we splay the leaf position at which the search terminates unsuccessfully
- When inserting key `k`:
	- We splay the newly created internal node where `k` gets created
- When deleting key `k`:
	- We splay position `p` that is the parent of the removed node (the removed node may be that originally contained k or a descendant node with replacement key)

![[Pasted image 20230731160356.png]]

![[Pasted image 20230731160435.png]]


### Performance

The performance of Splay trees for `m` insertions, deletions and searches is `O(m log n)`

### Notes:

- If trees are added to a splay tree in a strictly increasing order, (or accessed in a strictly increasing order) it results in a "linear" splay tree


---

## Multiway search tree

Map items stored in a search tree are pairs of the form `(k, v)` where `k` is the key and `v` is the value associated with the key


### Definition

Let `w` be a node of an ordered tree
- We say that `w` is a `d-node` if `w` has `d` children

We define a multiway search tree to be an ordered tree T that has the following properties
- Each internal node of T has at least 2 children
	- i.e. each internal node is a d-node with d >= 2
- Each internal d-node `w` of T with children `c1...cd` stores an ordered set of `d-1` KV pairs `(k1, v1) ... (kd-1, vd-1)` where k1 <= k2 <= ... kd-1
- we define k0 = NEGATIVE_INFINIT and kd = POSITIVE_INFINIT
	- For each item stored at a node in the subtree of `w` rooted at `ci`, `i = 1 .. d`, we have that `ki-1 <= k <= ki`

![[Pasted image 20230811225552.png]]

### Proposition

An `n-item` multiway search tree has `n + 1` external nodes


![[Pasted image 20230811225655.png]]


The primary efficiency goal for a multiway search tree is to keep the height as possible (one such strategy is the (2,4) tree)


---


## (2,4) Trees

Resource: [(2,4) tree animation](https://yongdanielliang.github.io/animation/web/24Tree.html)


(2,4) Trees is a multiway search tree with the following properties:
- *Size Property*: Every internal node has at most 4 children (hence aka 2-3-4 trees)
- *Depth Property*: All the external nodes have the same depth


![[Pasted image 20230811230544.png]]

### Proposition

The height of a 2-3-4 tree storing `n` items is `O(log n)`


### Operations

#### Insertion

To insert a new item `(k,v)` , we first perform a search for `k`.
- Assuming unsuccessful search, the search terminates at an external node `z`
- let `w` be the parent of `z`
- We insert the new item into node `w` and add a new child `y` (an external node) ti `w` on the left of `z`

Our insertion method preserves the *depth* property
- since we add a new external node at the same level as existing node

HOWEVER, insertion may violate the *size* property
- if a node `w` was previously a 4-node, then it would become a 5-node, which invalidates a (2,4) tree

Let `c1..c5` be children of `w`, and let `k1..k4` be the keys stored at `w`

On the overflow node at `w`, we perform a *split* operation on `w` as follows:
- Replace `w` with two nodes `w'` and `w''` where
	- `w'` is a 3-node with children c1, c2, c3 storing keys k1, k2
	- `w''` is a 2-node with children c4, c5 storing key k4
- if `w` is the root of T, create a new root node `u`
	- else let `u` be the parent of `w`
- Insert key `k3` into `u` and make `w'` and `w''` children of `u`, so that if `w` was child `i` of `u`, then `w'` and `w''` become children `i` and `i + 1` of `u`, respectively

As a consequence of a split operation on node `w`, a new overflow may occur at the parent `u` of `w`, which triggers in turn a *split* operation on `u`
- a split operation either eliminates the overflow or propagates it into the parent of the current node

![[Pasted image 20230811232248.png]]

![[Pasted image 20230811232305.png]]

![[Pasted image 20230811232545.png]]


NOTE: **Insertion is O(log n)**


#### Deletion

Deletion begins with a search of the key.

**Removing an item from a (2,4) tree can always be reduced to the case where the item to be removed is stored at node `w` whose children are external nodes**

Suppose, as opposed to above, that the item with key `k` that we wish to remove is stored in the `ith` item at a node `z` that has only internal-node children:
- In this case, we swap the item `(ki, vi)` with an appropriate item that is stored at a node `w` with external-node children as follows:
	1.  We find the rightmost internal node `w` in the subtree rooted at the `ith` child of `z`, noting that the children of node `w` are all external nodes
	2. We swap the item `(ki, vi)` at `z` with the last item of `w`
- After the above method, the item to be removed is stored at node `w` with only external node children)
- We can now simply remove the item from `w` and remove the `ith` external node of `w`
- This is depicted in figure below (step d):

![[Pasted image 20230811234327.png]]

Deletion preserves the *depth* property, but it may violate the *size* property
- if the node previously was a 2-node, it becomes a 1-node which is not allowed in (2,4) tree
- This is called an *underflow*

To remedy an *underflow* at node `w`:
- check if an immediate sibling of node `w` is a 3-node or a 4-node
	- if yes, perform a *transfer* operation
		- in which we move 
			- a child of `s` to `w`
			- a key of `s` to the parent `u` of `w` and `s`
			- and a key of `u` to `w` (steps b and c above diagram)
	- if no (either only 1 sibling or if both siblings are 2-node)
		- then we perform a *fusion* operation
		- in which we
			- merge `w` with a sibling
			- thus creating a new node `w'`
			- and move a key from the parent of `u` of `w` to `w'` (steps e and f above diagram)

![[Pasted image 20230811235625.png]]


NOTE: **The asymptotic performance of (2,4) trees is identical to that of an AVL tree**

(2,4) trees provide for fast map search and update operations.


(2,4) Trees have some disadvantages:
- may require many *split* or *fusion* operations to be performed after an insertion or removal

---


## Red-Black Trees

[Red-Black Tree visualization](https://www.cs.usfca.edu/~galles/visualization/RedBlack.html)

A BST with nodes colored red and black that satisfy the following properties:
- *Root Property*: The root is always black
- *Red Property*: The children of a red node (if any) are black
- *Depth Property*: All nodes with zero or 1 children have the same **black depth**, defined as the number of black ancestors (a node is considered as its own ancestor)

Example:
![[Pasted image 20230812122459.png]]


Wikipedia defines the Red-Black tree properties as follows:
   - Every node is either Red or Black
   - All NIL (empty child nodes of external nodes) are considered black
   - a red node does not have a red child (*Red Property*)
   - Every path from a node to its descendent NIL node goes thru the same number of black nodes (*Depth Property*)
   - if a node has exactly 1 child, it must be red (*Red Property*)


### Correspondence with (2,4) trees

Given a Red-Black tree, we can make a corresponding (2,4) tree by 
- merging every node `w` into its parent
- storing the entry from `w` at its parent
- with children of `w` becoming ordered children of parent

For example, the Red-Black tree [above](obsidian://open?vault=my-notes&file=Algorithms-DataStructures%2Fimages%2FPasted%20image%2020230812122459.png) corresponds to the (2,4) tree [in this diagram](obsidian://open?vault=my-notes&file=Algorithms-DataStructures%2Fimages%2FPasted%20image%2020230811230544.png)


Conversely, to transform any (2,4) tree to Red-Black tree, do the following transformations:
- Color each node `w` black
- if `w` is a 2-node, then keep the (black) children of `w` as is
- if `w` is a 3-node, then create a new red node `y`
	- give `w`'s last 2 (black) children to `y`
	- and make the first child of `w` and `y` be the 2 children of `w`
- If `w` is a 4-node, then create 2 new red nodes `y` and `z`, 
	- give `w`'s first 2 (black) children to `y`
	- give `w`'s last 2 (black) children to `z`
	- and make `y` and `z` be the 2 children of `w`

![[Pasted image 20230812124303.png]]


### Proposition: Height

The height of a red-black tree storing `n` entries is `O(log n)`


### Operations

#### Insertion

- Search for k in Tree until we reach a NIL subtree, and we introduce a new leaf `x` at that position, storing the item
- if `x` is the only node of T, and thus the root
	- we color it black
	- else: we color it red
- Insertion preserves the *root* and *depth* property, but may violate the red property
	- Indeed, if `x` is not the root of T, and the parent `y` of `x` is red, then we have a parent & child that are both red
	- Since `x` and its parent are red, and `x`'s grandparent is black, we call this violation a **double red** at node `x`

To remedy a **double red**, we consider 2 cases:

##### 1. **Case 1: the Sibling s of y is Black (or None)**:
- This formation has one red not (y) that is the parent of another red node (x), while we want it to have the 2 red nodes as siblings.
- fix this by doing *trinode restructuring* of T

![[Pasted image 20230812130826.png]]

##### 2. **Case 2: The Siblings s of y is Red**
- To fix this problem, we perform the equivalent of a *split* operation
- Namely we do a *recoloring*
	- we color `y` and `s` black
	- and their parent `z` red
		- (unless `z` is the root, in which case it remains black)
	- Notice that unless `z` is the root, the portion of any path through the affected part of the tree is incident to exactly one black node, both before and after (ths **black depth** property remains satisfied)
- However, it's possible that the **double-red** problem reappears after such a recoloring, albeit higher up in the tree T (since `z` may have a red parent)
	- If there is a **double-red**, we repeat the 2 cases discussed
- Thus, a recoloring either
	- eliminates the **double-red** problem at node `x`
	- or propagates it to the grandparent `z` of `x`

![[Pasted image 20230812131551.png]]


![[Pasted image 20230812131619.png]]

![[Pasted image 20230812131654.png]]


#### Deletion

Structurally, the deletion process results in the removal of a node that has at most one child (either that originally containing key `k` or its inorder predecessor) and the promotion of its remaining child.

If the removed node was red
	- this structural change does not affect *black depth* of any paths in the tree, so the resulting tree remains a valid Red-Black tree
	- This corresponds to shrinking of 3-node or 4-node

If the removed node was black
	- then it either had 0 children
	- or it had one child that was a red leaf (coz the NIL subtree of the removed node has *black height* = 0)
		- the removed node represents the black part of a corresponding 3-node
		- and we restore the red-black properties by recoloring the promoted child to black

The more complex case is when a non-root black leaf is removed
	- this corresponds to removal of an item from a 2-node
	- without rebalancing, such a change results in a deficit of one for the *black depth* along the path leading to the deleted item
	- By necessity, the removed node must have a sibling who subtree has black height 1 (given that this was a valid red-black tree prior to deletion of the black leaf)


To remedy this scenario:
- consider a node `z` with 2 subtree Theavy and Tlight
- such that root of Tlight = black (if any)
- such that black depth of Theavy = Black depth of Tlight + 1
- in the case of the removed black leaf, `z` is its parent, and the leaf is removed from Tlight, thus leaving it as empty subtree after deletion
- let `y` denote the root of Theavy
	- such a node exists because Theavy has a black height of at least 1

![[Pasted image 20230812212906.png]]

Consider 3 possible cases:

##### 1. **Case 1: Node `y` is Black and has a red child `x`**

We perform a *trinode restructuring*

Notice that the path to Tlight in the result includes on additional black node after the restructure, thereby resolving the deficit. 
- The other path's black depth remains unchanged

This corresponds to a *transfer* operation in (2,4) tree

![[Pasted image 20230812215835.png]]


##### 2. **Case 2: Node y is Black and Both children of y are Black (or None)**

This case corresponds to the *fusion* operation in (2,4) tree

We do a recoloring
- we color `y` red
- and if `z` is red, we color it black
- This does not introduce any red violation
	- because `y` does not have a red child

In the case that `z` was originally red
- this resolves the deficit (case a below diag)
-  The path leading to Tlight includes 1 additional black node in the result
- while recoloring did not affect the number of black nodes on the path to the subtrees of Theavy

In the case the `z` was originally black (and thus the parent in the corresponding (2,4) tree is a 2-node)
- the recoloring has not increased the number of black nodes on the path to Tlight
- in fact it has reduced the number of black nodes on the path to Theavy
- After this step, the 2 children of `z` will have the same black height
- HOWEVER, the entire tree rooted at `z` has become deficient
	- thereby propagating the problem higher in the tree

![[Pasted image 20230812222123.png]]


##### 3. **Case 3: Node y is Red**

Because `y` is red and THeavy has black depth at least 1, 
- `z` must be black and the 2 subtrees of `y` must each have a black root and a black depth equal to that of THeavy
- In this case, we perform a rotation about `y` and `z` 
- and the recolor `y` = black and `z` = red
- this denotes a reorientation of a 3-node in the corresponding (2,4) tree

This does not immediately resolve the deficit
- as the new subtree of `z` is an old subtree of `y` with black root `y'` and black height equal to that of the original THeavy
- We reapply the algo to resolve the deficit at `z`
	- knowing that the new chi `y'` , that is the root of THeavy, is now black
	- and therefore that either Case 1 applies or Case 2 applies
- Furthermore, the next application of algo will be last
	- coz Case 1 is always terminal 
	- and Case 2 will be terminal given `z` is red

![[Pasted image 20230812222907.png]]

![[Pasted image 20230812223004.png]]
^ In the above diagram, a dashed edge (like right of 7 in part c) represents a branch with a black deficiency that has not yet been resolved.
^ Case 1 restructuring happens in part c, d
^ Case 2 = f & g (and a recoloring in k)
^ Case 3= i j k


### Performance

#### Insertion

> The insertion of an item in a Red-black tree storing `n` items can be done in `O(Log n)` time and requires `O(log n)` recolorings and at most 1 trinode-restructuring


#### Deletion

> The algo for deleting an item from a Red-black tree with `n` items take `O(Log n)` time and performs `O(log n)` recolorings and at most 2 restructuring operations


---


# Performance Comparison

For `n` entries stored in the above trees, the worst case height is given by:
- Binary Tree -> `n`
- AVL Tree -> `2 log n`
- Splay tree -> `n`
- (2,4) tree -> `log n`
- Red-black -> `2 log n`