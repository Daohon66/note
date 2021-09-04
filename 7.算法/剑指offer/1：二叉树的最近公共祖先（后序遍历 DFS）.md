1：二叉树的最近公共祖先（后序遍历 DFS）

**最近公共祖先的定义**： 设节点root 为节点p,q 的某公共祖先，若其左子节点 root.leftroot.left 和右子节点 root.rightroot.right 都不是 p,q 的公共祖先，则称root 是 “最近的公共祖先” 。

根据以上定义，若root 是 p,q 的 最近公共祖先 ，则只可能为以下情况之一：

p 和 q在 rootroot 的子树中，且分列 root 的 异侧（即分别在左、右子树中）；

p = root ，且 q 在 root 的左或右子树中；

```java
class Solution { 
    public  TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        //递归停止条件
        if(root==null) return null;//越过叶节点，终止递归，向上返回null值
        if(root==p) return root;//向下递归到p，终止递归，向上返回p值
        if(root==q) return root;//向下递归到q，终止递归，向上返回q值

//递归遍历左子树，只要在左子树中找到p或q，则先找到谁就返回谁
        TreeNode left = lowestCommonAncestor(root.left,p,q);

//递归遍历右子树，只要在右子树中找//到了p或q，则先找到谁就返回谁
        TreeNode right = lowestCommonAncestor(root.right,p,q);

//如果左子树和右子树都返回null 说明root为空则返回null
        if(left == null && right == null) return null;

//如果右子树为空，则最近公共祖先就在左子树中
        if(left != null && right == null) return left;

//如果左子树为空，则最近公共 祖先就在右子树中
        if(left == null && right != null) return right;

//if(left != null && right != null)左右子树都不为空，则root就是最近公共祖先  
        return root;
    }
}
```

