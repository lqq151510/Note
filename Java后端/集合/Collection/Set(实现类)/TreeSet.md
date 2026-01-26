# Java 中的树、二叉树和平衡二叉树（含旋转机制）

下面这个表格对比了树、二叉树和平衡二叉树的核心特性：

|特性|树 (Tree)|二叉树 (Binary Tree)|平衡二叉树 (Balanced Binary Tree)|
|---|---|---|---|
|**定义**​|非线性数据结构，由节点和边组成，每个节点有零个或多个子节点|每个节点最多有两个子节点（左/右子节点）|特殊的二叉搜索树，任意节点的左右子树高度差 ≤ 1|
|**结构**​|任意结构，无子节点数限制|严格最多2个子节点|二叉树结构 + 平衡约束|
|**遍历方式**​|前序、后序、层序|前序、中序、后序、层序|同上，但搜索效率更高|
|**时间复杂度**​|取决于具体类型|普通二叉树：O(n)|搜索/插入/删除：O(log n)|
|**常见类型**​|普通树、B树、B+树、Trie树|普通二叉树、二叉搜索树、满二叉树、完全二叉树|AVL树、红黑树、伸展树|
|**应用场景**​|文件系统、数据库索引、XML/HTML解析|表达式树、哈夫曼编码、搜索算法|数据库索引、集合类（TreeMap、TreeSet）、内存分配|

## 1. 树 (Tree)

### 基本概念

树是一种分层的数据结构，由节点（node）和边（edge）组成：

- **根节点**：没有父节点的节点
    
- **子节点/父节点**：节点之间的关系
    
- **叶子节点**：没有子节点的节点
    
- **深度/高度**：从根到节点的边数/从节点到最远叶子的边数
    

### Java 实现

```
// 通用树节点定义
class TreeNode<T> {
    T data;
    List<TreeNode<T>> children;
    
    public TreeNode(T data) {
        this.data = data;
        this.children = new ArrayList<>();
    }
    
    public void addChild(TreeNode<T> child) {
        children.add(child);
    }
}

// 树的基本操作
class Tree<T> {
    private TreeNode<T> root;
    
    public Tree(T rootData) {
        root = new TreeNode<>(rootData);
    }
    
    // 前序遍历
    public void preOrderTraversal(TreeNode<T> node) {
        if (node == null) return;
        
        System.out.print(node.data + " ");
        for (TreeNode<T> child : node.children) {
            preOrderTraversal(child);
        }
    }
    
    // 后序遍历
    public void postOrderTraversal(TreeNode<T> node) {
        if (node == null) return;
        
        for (TreeNode<T> child : node.children) {
            postOrderTraversal(child);
        }
        System.out.print(node.data + " ");
    }
    
    // 层序遍历
    public void levelOrderTraversal() {
        if (root == null) return;
        
        Queue<TreeNode<T>> queue = new LinkedList<>();
        queue.offer(root);
        
        while (!queue.isEmpty()) {
            TreeNode<T> current = queue.poll();
            System.out.print(current.data + " ");
            
            for (TreeNode<T> child : current.children) {
                queue.offer(child);
            }
        }
    }
}
```

## 2. 二叉树 (Binary Tree)

二叉树是每个节点最多有两个子节点（左子节点和右子节点）的树结构。

### 二叉树类型

```
// 二叉树节点定义
class BinaryTreeNode {
    int val;
    BinaryTreeNode left;
    BinaryTreeNode right;
    
    public BinaryTreeNode(int val) {
        this.val = val;
        this.left = null;
        this.right = null;
    }
}

// 二叉树实现
class BinaryTree {
    private BinaryTreeNode root;
    
    // 各种遍历方式
    public void preOrder(BinaryTreeNode node) {
        if (node == null) return;
        System.out.print(node.val + " ");  // 访问根
        preOrder(node.left);              // 遍历左子树
        preOrder(node.right);             // 遍历右子树
    }
    
    public void inOrder(BinaryTreeNode node) {
        if (node == null) return;
        inOrder(node.left);               // 遍历左子树
        System.out.print(node.val + " ");  // 访问根
        inOrder(node.right);              // 遍历右子树
    }
    
    public void postOrder(BinaryTreeNode node) {
        if (node == null) return;
        postOrder(node.left);             // 遍历左子树
        postOrder(node.right);            // 遍历右子树
        System.out.print(node.val + " ");  // 访问根
    }
    
    // 非递归遍历（使用栈）
    public void inOrderIterative() {
        Stack<BinaryTreeNode> stack = new Stack<>();
        BinaryTreeNode current = root;
        
        while (current != null || !stack.isEmpty()) {
            // 遍历到最左节点
            while (current != null) {
                stack.push(current);
                current = current.left;
            }
            
            // 访问节点
            current = stack.pop();
            System.out.print(current.val + " ");
            
            // 转向右子树
            current = current.right;
        }
    }
}
```

### 特殊二叉树类型

```
// 判断是否为二叉搜索树 (BST)
class BSTValidator {
    public boolean isValidBST(BinaryTreeNode root) {
        return validate(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }
    
    private boolean validate(BinaryTreeNode node, long min, long max) {
        if (node == null) return true;
        
        if (node.val <= min || node.val >= max) {
            return false;
        }
        
        return validate(node.left, min, node.val) && 
               validate(node.right, node.val, max);
    }
}

// 二叉搜索树操作
class BinarySearchTree {
    private BinaryTreeNode root;
    
    // 插入节点
    public void insert(int val) {
        root = insertRecursive(root, val);
    }
    
    private BinaryTreeNode insertRecursive(BinaryTreeNode node, int val) {
        if (node == null) {
            return new BinaryTreeNode(val);
        }
        
        if (val < node.val) {
            node.left = insertRecursive(node.left, val);
        } else if (val > node.val) {
            node.right = insertRecursive(node.right, val);
        }
        
        return node;
    }
    
    // 搜索节点
    public boolean search(int val) {
        return searchRecursive(root, val);
    }
    
    private boolean searchRecursive(BinaryTreeNode node, int val) {
        if (node == null) return false;
        
        if (val == node.val) return true;
        else if (val < node.val) return searchRecursive(node.left, val);
        else return searchRecursive(node.right, val);
    }
}
```

## 3. 平衡二叉树 (AVL树)

AVL树是最早的自平衡二叉搜索树，通过旋转操作保持平衡。

### AVL树节点

```
class AVLNode {
    int val;
    int height;  // 节点高度
    AVLNode left;
    AVLNode right;
    
    public AVLNode(int val) {
        this.val = val;
        this.height = 1;  // 新节点高度为1
    }
}
```

### 四种旋转操作详解

下面的流程图展示了AVL树的四种不平衡情况和对应的旋转操作：

```
flowchart TD
    A[插入/删除导致不平衡] --> B{检查平衡因子}
    
    B -->|左子树更高 LL型| C[右旋转]
    B -->|右子树更高 RR型| D[左旋转]
    B -->|左子树的右子树更高 LR型| E[先左旋后右旋]
    B -->|右子树的左子树更高 RL型| F[先右旋后左旋]
    
    C --> G[恢复平衡]
    D --> G
    E --> G
    F --> G
```

#### 3.1 右旋转 (LL旋转)

当左子树比右子树高，且不平衡节点在左子树的左子树时使用。

```
class AVLTree {
    private AVLNode root;
    
    // 获取节点高度
    private int height(AVLNode node) {
        return node == null ? 0 : node.height;
    }
    
    // 获取平衡因子
    private int getBalanceFactor(AVLNode node) {
        return node == null ? 0 : height(node.left) - height(node.right);
    }
    
    // 右旋转（针对LL情况）
    private AVLNode rightRotate(AVLNode y) {
        /*
            y (不平衡节点)          x
           / \                    /  \
          x   T3      →         T1    y
         / \                        /  \
        T1  T2                     T2  T3
        */
        AVLNode x = y.left;
        AVLNode T2 = x.right;
        
        // 执行旋转
        x.right = y;
        y.left = T2;
        
        // 更新高度
        y.height = Math.max(height(y.left), height(y.right)) + 1;
        x.height = Math.max(height(x.left), height(x.right)) + 1;
        
        return x;  // 返回新的根节点
    }
    
    // 左旋转（针对RR情况）
    private AVLNode leftRotate(AVLNode x) {
        /*
          x (不平衡节点)              y
         /  \                      /  \
        T1   y         →         x    T3
            / \                /  \
           T2  T3             T1   T2
        */
        AVLNode y = x.right;
        AVLNode T2 = y.left;
        
        // 执行旋转
        y.left = x;
        x.right = T2;
        
        // 更新高度
        x.height = Math.max(height(x.left), height(x.right)) + 1;
        y.height = Math.max(height(y.left), height(y.right)) + 1;
        
        return y;  // 返回新的根节点
    }
}
```

#### 3.2 左右旋转 (LR旋转)

先对左子节点左旋，再对当前节点右旋。

```
// 左右旋转（针对LR情况）
private AVLNode leftRightRotate(AVLNode z) {
    /*
        z (不平衡节点)          z          T2
       / \                    / \        /  \
      y   T4      →         T2  T4  →  y     z
     / \                    / \        / \   / \
    T1  T2                 y   T3     T1  T3 T4
         \                /
         T3             T1
    */
    z.left = leftRotate(z.left);  // 先对左子节点左旋
    return rightRotate(z);        // 再对当前节点右旋
}
```

#### 3.3 右左旋转 (RL旋转)

先对右子节点右旋，再对当前节点左旋。

```
// 右左旋转（针对RL情况）
private AVLNode rightLeftRotate(AVLNode z) {
    /*
        z (不平衡节点)         z             T3
       /  \                 /  \           /  \
      T1   y     →        T1   T3    →   z     y
          / \                 /  \      / \   / \
         T3  T4              T2   y    T1 T2 T3 T4
        / \                      /  \
       T2                        T4
    */
    z.right = rightRotate(z.right);  // 先对右子节点右旋
    return leftRotate(z);             // 再对当前节点左旋
}
```

### 完整的AVL树实现

```
class AVLTree {
    private AVLNode root;
    
    // 插入节点
    public void insert(int val) {
        root = insertRecursive(root, val);
    }
    
    private AVLNode insertRecursive(AVLNode node, int val) {
        // 1. 标准的BST插入
        if (node == null) {
            return new AVLNode(val);
        }
        
        if (val < node.val) {
            node.left = insertRecursive(node.left, val);
        } else if (val > node.val) {
            node.right = insertRecursive(node.right, val);
        } else {
            return node;  // 不允许重复值
        }
        
        // 2. 更新当前节点高度
        node.height = 1 + Math.max(height(node.left), height(node.right));
        
        // 3. 获取平衡因子，检查是否不平衡
        int balance = getBalanceFactor(node);
        
        // 4. 根据不平衡类型进行旋转
        // LL情况
        if (balance > 1 && val < node.left.val) {
            return rightRotate(node);
        }
        
        // RR情况
        if (balance < -1 && val > node.right.val) {
            return leftRotate(node);
        }
        
        // LR情况
        if (balance > 1 && val > node.left.val) {
            return leftRightRotate(node);
        }
        
        // RL情况
        if (balance < -1 && val < node.right.val) {
            return rightLeftRotate(node);
        }
        
        return node;  // 返回未修改的节点
    }
    
    // 删除节点
    public void delete(int val) {
        root = deleteRecursive(root, val);
    }
    
    private AVLNode deleteRecursive(AVLNode node, int val) {
        if (node == null) return null;
        
        // 1. 标准的BST删除
        if (val < node.val) {
            node.left = deleteRecursive(node.left, val);
        } else if (val > node.val) {
            node.right = deleteRecursive(node.right, val);
        } else {
            // 找到要删除的节点
            if (node.left == null || node.right == null) {
                // 有一个或没有子节点
                AVLNode temp = (node.left != null) ? node.left : node.right;
                node = temp;  // 用子节点替换或设为null
            } else {
                // 有两个子节点：找到中序后继
                AVLNode temp = getMinNode(node.right);
                node.val = temp.val;  // 复制值
                node.right = deleteRecursive(node.right, temp.val);  // 删除后继
            }
        }
        
        if (node == null) return null;
        
        // 2. 更新高度
        node.height = 1 + Math.max(height(node.left), height(node.right));
        
        // 3. 检查平衡
        int balance = getBalanceFactor(node);
        
        // 4. 根据不平衡类型进行旋转
        // LL情况
        if (balance > 1 && getBalanceFactor(node.left) >= 0) {
            return rightRotate(node);
        }
        
        // LR情况
        if (balance > 1 && getBalanceFactor(node.left) < 0) {
            return leftRightRotate(node);
        }
        
        // RR情况
        if (balance < -1 && getBalanceFactor(node.right) <= 0) {
            return leftRotate(node);
        }
        
        // RL情况
        if (balance < -1 && getBalanceFactor(node.right) > 0) {
            return rightLeftRotate(node);
        }
        
        return node;
    }
    
    // 获取最小节点
    private AVLNode getMinNode(AVLNode node) {
        AVLNode current = node;
        while (current.left != null) {
            current = current.left;
        }
        return current;
    }
    
    // 中序遍历
    public void inOrderTraversal() {
        inOrderRecursive(root);
        System.out.println();
    }
    
    private void inOrderRecursive(AVLNode node) {
        if (node != null) {
            inOrderRecursive(node.left);
            System.out.print(node.val + "(h=" + node.height + ") ");
            inOrderRecursive(node.right);
        }
    }
}
```

### 测试示例

```
public class AVLTreeExample {
    public static void main(String[] args) {
        AVLTree avlTree = new AVLTree();
        
        // 测试插入
        System.out.println("插入顺序: 10, 20, 30, 40, 50, 25");
        
        avlTree.insert(10);
        avlTree.insert(20);
        avlTree.insert(30);  // 触发旋转
        avlTree.insert(40);
        avlTree.insert(50);  // 触发旋转
        avlTree.insert(25);
        
        System.out.print("中序遍历: ");
        avlTree.inOrderTraversal();  // 输出: 10 20 25 30 40 50
        
        // 测试删除
        avlTree.delete(25);
        System.out.print("删除25后: ");
        avlTree.inOrderTraversal();  // 输出: 10 20 30 40 50
        
        avlTree.delete(40);
        System.out.print("删除40后: ");
        avlTree.inOrderTraversal();  // 输出: 10 20 30 50
    }
}
```

## 平衡因子计算与旋转规则总结

|不平衡情况|平衡因子|子树情况|旋转方法|
|---|---|---|---|
|**LL型**​|节点A: +2|左子节点B: +1 或 0|右旋转|
|**RR型**​|节点A: -2|右子节点B: -1 或 0|左旋转|
|**LR型**​|节点A: +2|左子节点B: -1|先左旋(B)后右旋(A)|
|**RL型**​|节点A: -2|右子节点B: +1|先右旋(B)后左旋(A)|

## Java 中的平衡二叉树实现

Java集合框架中已经实现了平衡二叉树：

### TreeMap (基于红黑树)

```
import java.util.TreeMap;
import java.util.Map;

public class TreeMapExample {
    public static void main(String[] args) {
        // TreeMap内部使用红黑树（一种近似平衡的二叉搜索树）
        TreeMap<Integer, String> treeMap = new TreeMap<>();
        
        treeMap.put(10, "Apple");
        treeMap.put(5, "Banana");
        treeMap.put(15, "Cherry");
        treeMap.put(3, "Date");
        treeMap.put(7, "Elderberry");
        
        // 自动按key排序
        for (Map.Entry<Integer, String> entry : treeMap.entrySet()) {
            System.out.println(entry.getKey() + ": " + entry.getValue());
        }
        // 输出: 3: Date, 5: Banana, 7: Elderberry, 10: Apple, 15: Cherry
    }
}
```

### TreeSet (同样基于红黑树)

```
import java.util.TreeSet;

public class TreeSetExample {
    public static void main(String[] args) {
        TreeSet<Integer> treeSet = new TreeSet<>();
        
        treeSet.add(30);
        treeSet.add(20);
        treeSet.add(40);
        treeSet.add(10);
        treeSet.add(25);
        
        System.out.println("有序集合: " + treeSet);  // [10, 20, 25, 30, 40]
        System.out.println("第一个元素: " + treeSet.first());  // 10
        System.out.println("最后一个元素: " + treeSet.last());  // 40
        System.out.println("大于等于15的最小元素: " + treeSet.ceiling(15));  // 20
    }
}
```

## 性能对比

|操作|普通二叉搜索树|AVL树|红黑树|
|---|---|---|---|
|**搜索**​|O(n) 最坏|O(log n)|O(log n)|
|**插入**​|O(n) 最坏|O(log n)|O(log n)|
|**删除**​|O(n) 最坏|O(log n)|O(log n)|
|**旋转次数**​|无|较多|较少|
|**平衡要求**​|无|严格（高度差≤1）|较宽松（5个性质）|
|**适用场景**​|数据随机分布|查找密集型|插入/删除频繁|

## 总结

1. **树**是基础数据结构，二叉树是每个节点最多有两个子节点的树。
    
2. **平衡二叉树**（如AVL树）通过旋转操作保持平衡，确保操作时间复杂度为O(log n)。
    
3. **旋转机制**是AVL树的核心，包括四种情况：LL（右旋）、RR（左旋）、LR（左右旋）、RL（右左旋）。
    
4. **Java中的实现**：TreeMap和TreeSet基于红黑树（一种近似平衡的二叉搜索树）。
    
5. **实际应用**：数据库索引、文件系统、内存管理、网络路由等。
    

理解这些数据结构和它们的平衡机制对于设计高效算法和系统至关重要。AVL树提供了严格的平衡保证，而红黑树则在旋转开销和平衡性之间取得了更好的折衷，这也是Java集合框架选择红黑树的原因。