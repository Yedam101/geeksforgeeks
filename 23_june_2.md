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
        # if not root:
        #     return 0
        # return 1 + max(self.maxDepth(root.right), self.maxDepth(root.left))

        def helper(root):
            if not root:
                return 0
            r = helper(root.right)
            l = helper(root.left)
            return 1 + max(r, l)
        return helper(root)
```

### 543. Diameter of Binary Tree

```
class Solution:
    def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:
        self.maxi = 0
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

### 100. Same Tree

```
class Solution:
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:

        def rec(p,q):
            if not p and not q:
                return True
            if (not p and q) or (not q and p):
                return False
            if p.val != q.val:
                return False
            return rec(p.left, q.left) and rec(p.right, q.right)
        
        return rec(p,q) 
```

### 프로그래머스 - 개인정보 수집 유효기간

```
def solution(today, terms, privacies):
    answer = []
    today = int(today[:4])*336+(int(today[5:7])-1)*28+int(today[8:])
    
    priv = [i.split() for i in privacies]
    priv = [[int(i[0][:4])*336+(int(i[0][5:7])-1)*28+int(i[0][8:]), i[1]] for i in priv]
    
    termd = {}
    for i in terms:
        termd[i[0]] = int(i[2:])*28

    for i in range(len(priv)):
        date = priv[i][0]+termd[priv[i][1]]
        if date <= today:
            answer.append(i+1)
    return answer
```