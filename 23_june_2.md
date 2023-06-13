### 226. Invert Binary Tree

```
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
                return
        self.invertTree(root.left) 
        self.invertTree(root.right)  
        root.left, root.right = root.right, root.left
        return root
```

### 104. Maximum Depth of Binary Tree

```
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        
        return 1 + max(self.maxDepth(root.right), self.maxDepth(root.left))
```

### 543. Diameter of Binary Tree

```
class Solution:
    def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:
        def helper(root):
            if not root:
                return 0
            
            lh = helper(root.left)
            rh = helper(root.right)
        
            self.maxi = max(self.maxi, lh + rh)

            return 1 + max(lh, rh)
        
        helper(root)
        return self.maxi
```