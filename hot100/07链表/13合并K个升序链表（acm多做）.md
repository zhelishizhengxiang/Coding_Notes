[23. 合并 K 个升序链表 - 力扣（LeetCode）](https://leetcode.cn/problems/merge-k-sorted-lists/description/?envType=study-plan-v2&envId=top-100-liked)
```java
class Solution {
    //假设不能创建新的节点，复用原来的节点
    public ListNode mergeKLists(ListNode[] lists) {
        //使用小顶堆存当前每个链表的节点，然后使用小顶堆的特性直接弹出最小的，
        Queue<ListNode> queue=new PriorityQueue<>((s1,s2)->{
            return s1.val-s2.val;//小顶堆，并且比较的值节点值
        });
        //堆进行初始化
        for(ListNode node:lists){
            //如果有链表为空就不用入队列
            if(node!=null)
                queue.offer(node);
        }

        //定义一个新链表的头结点head，和一个不断维护的尾节点
        ListNode head=null;
        ListNode cur=null;

        while(!queue.isEmpty()){
            ListNode node=queue.poll();
            //如果头结点还为空，也就是出列的第一个元素
            if(head==null){
                head=node;
                cur=node;
            }
            else{
                cur.next=node;
                cur=cur.next;
            }

            //判断该节点所属链表后面是否还有节点,有节点，就入队列
            if(node.next!=null)
                queue.offer(node.next);

        }
        return head;
    }
}
```

==当遇到不让你输入链表长度，就直接输入链表的时候，可以使用split方法继续分割然后进行读取，并且一定要消除回车==
```java
import java.util.*;

class Node{
    int val;
    Node next;
    public Node(int val){
        this.val=val;
    }
}

public class Main{
    public static Node func(Node[] heads){
        Queue<Node> queue=new PriorityQueue<>((s1,s2)->s1.val-s2.val);
        for(Node i:heads){
            queue.offer(i);
        }

        Node head=null;
        Node tmp=null;
        while(!queue.isEmpty()){
            Node node=queue.poll();
            if(head==null){
                head=node;
                tmp=node;
            }else{
                tmp.next=node;
                tmp=node;
            }
            if(node.next!=null)
                queue.offer(node.next);
        }
        return head;
    }
    public static void main(String[] args){
        Scanner scanner=new Scanner(System.in);
        int n=scanner.nextInt();
        //消除回车符
        scanner.nextLine();
        Node[] nums=new Node[n];
        for(int i=0;i<n;i++){
            //使用split方法然后转为int
            String[] strs=scanner.nextLine().split(" ");
            Node head=new Node(Integer.parseInt(strs[0]));
            Node tmp=head;
            for(int j=1;j<strs.length;j++){
                Node node=new Node(Integer.parseInt(strs[j]));
                tmp.next=node;
                tmp=node;
            }
            nums[i]=head;
        }
        Node head=func(nums);
        Node tmp=head;
        while(tmp.next!=null){
            System.out.print(tmp.val+" ");
            tmp=tmp.next;    
        }
        System.out.println(tmp.val);
    }
}
```