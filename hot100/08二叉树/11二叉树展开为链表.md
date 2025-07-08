[114. 二叉树展开为链表 - 力扣（LeetCode）](https://leetcode.cn/problems/flatten-binary-tree-to-linked-list/submissions/623170448/?envType=study-plan-v2&envId=top-100-liked)

跟链表构造有关的操作，统一操作都是使用虚拟头结点来操作
```java
class Solution {
    //采用前序遍历，一遍遍历，一遍连接，并且为了能头结点进行操作，需要将pre指针初始化成一个虚拟节点，以完成统一操作
    TreeNode pre=new TreeNode(-1);
    public void traversal(TreeNode root){
        if(root==null)
            return ;
        
        //前序遍历
        pre.right=root;
        pre.left=null;
        pre=root;
        //为了在递归深入的时候，修改left和right指针，所以要拷贝备份
        TreeNode left=root.left;
        TreeNode right=root.right;
        traversal(left);
        traversal(right);

    }
    public void flatten(TreeNode root) {
        traversal(root);
    }
}
```

acm模式
```java
import java.util.*;

class TreeNode{
    int val;
    TreeNode left;
    TreeNode right;
    public TreeNode(int val){
        this.val=val;
    }
}
public class Main {
    public static TreeNode build(String[] nums){
        if(nums.length==0)
            return null;
        Queue<TreeNode> queue=new ArrayDeque<>();
        TreeNode root=new TreeNode(Integer.parseInt(nums[0]));
        int index=1;
        queue.offer(root);
        while (index<nums.length && !queue.isEmpty()) {
            TreeNode node=queue.poll();
            if(index<nums.length && !nums[index].equals("null")){
                TreeNode newnode=new TreeNode(Integer.parseInt(nums[index]));
                node.left=newnode;
                queue.offer(newnode);
            }
            index++;

            if(index<nums.length && !nums[index].equals("null")){
                TreeNode newnode=new TreeNode(Integer.parseInt(nums[index]));
                node.right=newnode;
                queue.offer(newnode);
            }
            index++;
        }
        return root;
    }
    public static TreeNode pre=null;
    public static void func(TreeNode root){
        if(root==null)
            return;
        //保存两个指针
        TreeNode left=root.left;
        TreeNode right=root.right;
        if(pre!=null){
            pre.left=null;
            pre.right=root;
        }
        pre=root;
        func(left);
        func(right);

    }
    public static List<String> traversal(TreeNode root){
        List<String> list=new ArrayList<>();
        if(root==null)
            return null;
        Queue<TreeNode> queue=new LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty()){
            TreeNode node=queue.poll();
            if(node==null){
                list.add("null");
            }else{
                list.add(node.val+"");
                queue.offer(node.left);
                queue.offer(node.right);
            }
        }

        while(list.get(list.size()-1).equals("null")){
            list.remove(list.size()-1);
        }
        return list;

    }
    public static void main(String[] args) {
        Scanner scanner=new Scanner(System.in);
        String[] nums=scanner.nextLine().split(" ");
        TreeNode root=build(nums);
        func(root);
        List<String> list=traversal(root);
        for(String i:list)
            System.out.print(i+" ");
    }
}
```
