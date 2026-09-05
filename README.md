# LeetCode 106 - Construct Binary Tree from Inorder and Postorder Traversal

## Problem

Given two integer arrays:

* `inorder` — the inorder traversal of a binary tree.
* `postorder` — the postorder traversal of the same binary tree.

Construct the original binary tree and return its root.

### Traversal Order

**Inorder:**

```text
Left → Root → Right
```

**Postorder:**

```text
Left → Right → Root
```

## Example 1

### Input

```text
inorder = [9,3,15,20,7]
postorder = [9,15,7,20,3]
```

### Output

```text
[3,9,20,null,null,15,7]
```

### Explanation

The constructed tree is:

```text
        3
       / \
      9   20
         /  \
        15   7
```

The last element of `postorder` is always the root of the current tree.

Here, `3` is the root. In the `inorder` array, `3` divides the tree into:

```text
Left subtree  → [9]
Right subtree → [15,20,7]
```

The same process is repeated for both subtrees.

## Example 2

### Input

```text
inorder = [-1]
postorder = [-1]
```

### Output

```text
[-1]
```

## Approach

The key observation is that the **last element of postorder is the root**.

After finding the root in the inorder array:

* Elements before the root belong to the left subtree.
* Elements after the root belong to the right subtree.

We recursively construct both subtrees.

A dictionary is used to store the positions of values in the inorder array, allowing the root position to be found quickly.

Since we process postorder from right to left, the **right subtree must be constructed before the left subtree**.

## Algorithm

1. Store the index of every value in the inorder array.
2. Start from the last element of postorder.
3. Take the current value as the root.
4. Find its position in inorder.
5. Recursively construct the right subtree.
6. Recursively construct the left subtree.
7. Return the constructed root.

## Code

```python
class Solution:
    def buildTree(self, inorder, postorder):
        index_map = {value: i for i, value in enumerate(inorder)}
        postorder_index = len(postorder) - 1

        def build(left, right):
            nonlocal postorder_index

            if left > right:
                return None

            root_value = postorder[postorder_index]
            postorder_index -= 1

            root = TreeNode(root_value)

            mid = index_map[root_value]

            root.right = build(mid + 1, right)
            root.left = build(left, mid - 1)

            return root

        return build(0, len(inorder) - 1)
```

## Complexity

* **Time Complexity:** `O(n)`
* **Space Complexity:** `O(n)`

Each node is processed once, and the dictionary provides constant-time lookup for positions in the inorder traversal.

## LeetCode Details

**Problem Number:** 106
**Problem Name:** Construct Binary Tree from Inorder and Postorder Traversal
**Difficulty:** Medium
**Topics:** Binary Tree, Recursion, Depth-First Search, Arrays, Hash Table

## Language

Python 3

## Author

T.Nandhini
