[138. 随机链表的复制 - 力扣（LeetCode）](https://leetcode.cn/problems/copy-list-with-random-pointer/description/?envType=study-plan-v2&envId=top-100-liked)

```java
class Solution {
    public Node copyRandomList(Node head) {
        //思路：先遍历一遍节点讲节点拷贝出来之后将原节点与现节点的对应关系用map存下来，然后在遍历一遍节点讲映射关系通过哈希表存进去
        //特殊情况判断
        if(head==null)
            return head;
        Map<Node,Node> map=new HashMap<>();

        //Map 的键和值是否可以为null取决于具体的 Map 实现，而非泛型本身的限制。跟泛型规定什么元素没有关系
        Node cur=head;
        //记录前继节点
        Node pre=null;
        //记录当前节点的头节点
        Node head2=null;
        while(cur!=null){
            Node node=new Node(cur.val);
            //记录拷贝链表的头结点
            if(head2==null)
                head2=node;
            if(pre!=null)
                pre.next=node;
            //记录映射信息，记录的是原始结点和拷贝结点的映射关系，
            map.put(cur,node);
            pre=node;
            cur=cur.next;
            
        }
        //再次遍历，存放映射信息
        cur=head;
        while(cur!=null){
            //首先找到原始节点的映射的节点,对应的拷贝节点
            Node y= map.get(cur.random);
            //然后让现在的拷贝结点的random指向映射y
            map.get(cur).random=y;
            cur=cur.next;
        }
        //  cur=head2;
        // while(cur!=null){
        //     System.out.print(cur.val);
        //     System.out.println(cur.random);

        //     cur=cur.next;
        // }
        return head2;

    
    }
}
```

该题acm思路与lectcode不同，看明白即可
```java
```java
import java.util.*;

class Node{
    int val;
    Node next;
    Node random;
    public Node(int val){
        this.val=val;
    }
}

public class Main {

    public static void main(String[] args) {
        Scanner scanner=new Scanner(System.in);
        int n=scanner.nextInt();
        //索引和对应的节点的map
        Map<Integer, Node> map=new HashMap<>();
        //记录random值的数组
        int[] nums=new int[n];
        Node head=new Node(scanner.nextInt());
        nums[0]=scanner.nextInt();
        Node tmp=head;
        map.put(0,head);
        for(int i=1;i<n;i++){
            Node node=new Node(scanner.nextInt());
            nums[i]=scanner.nextInt();
            tmp.next=node;
            tmp=node;
            map.put(i,node);
        }

        //构造random指针
        tmp=head;
        for(int i=0;i<n;i++){
            if(nums[i]==-1)
                tmp.random=null;
            else
                tmp.random=map.get(nums[i]);
            tmp=tmp.next;
        }
        //构造完成以后，为了需要获取索引，需要从链表头部重新开始遍历来获取索引
        tmp=head;
        while(tmp!=null){
            System.out.print(tmp.val+" ");
            if(tmp.random==null)
                System.out.println(-1);
            else{
                Node cur=head;
                int index=0;
                while(cur!=tmp.random){
                    index++;
                    cur=cur.next;
                }
                System.out.println(index);
            }
            
            tmp=tmp.next;
        }

    }
}
```
```

