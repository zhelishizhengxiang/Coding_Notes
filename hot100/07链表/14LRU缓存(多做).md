[146. LRU 缓存 - 力扣（LeetCode）](https://leetcode.cn/problems/lru-cache/description/?envType=study-plan-v2&envId=top-100-liked)
```java
class LRUCache {
    //思路使用双向链表+哈希表来解决：哈希表解决查找时候的o(1)，双向链表模拟热度，热度越高的放在虚拟头结点的后面，热度最低的放在虚拟头结点的前面
    class ListNode{
        int val;
        int key;
        ListNode pre;
        ListNode next;

        //ListNode的构造函数
        public ListNode(int key,int val){
            this.val=val;
            this.key=key;
        }
    }
    
    ListNode head;
    ListNode tail;
    int capacity;
    //通过key值直接定位到对应节点
    Map<Integer,ListNode> map=new HashMap<>();

    public LRUCache(int capacity) {
        this.capacity=capacity;
        // 创建头尾节点进行初始化
        head=new ListNode(-1,-1);
        tail=new ListNode(-1,-1);
        head.next=tail;
        tail.pre=head;
    }

    //o(1)复杂度进行插入节点，lRU只在头部插入
    public void insert(ListNode node){
        ListNode a=head.next;
        head.next=node;
        node.pre=head;
        node.next=a;
        a.pre=node;
    }

    //O（1）复杂度进行删除元素
    public void remove(ListNode node){
        ListNode a=node.pre;
        ListNode b=node.next;
        a.next=b;
        b.pre=a;
    }
    
    public int get(int key) {
        //如果关键字不在缓存中
        if(!map.containsKey(key))
            return -1;
        
        ListNode node=map.get(key);
        //使用过，所以热度设置为最高
        remove(node);
        insert(node);
        return node.val;

    }
    
    public void put(int key, int value) {
        //如果key不存在于缓存中
        if(!map.containsKey(key)){
            // insert逻辑
            
            //如果数量已经超过缓存容量
            if(map.size()==capacity){
                //删除热度最低的节点
                ListNode a=tail.pre;
                remove(a);
                //相同哈希表中也有删除
                map.remove(a.key);
            }
            //添加元素
            ListNode node=new ListNode(key,value);
            insert(node);
            map.put(key,node);
            
        }
        else{
            //update逻辑,更新值，放到头结点后面
            ListNode node=map.get(key);
            node.val=value;
            //更新热度
            remove(node);
            insert(node);

        }
    }
}
```

acm版本
```java
import java.util.*;


class LRUCache{
    int capacity;
    public LRUCache(int capacity){
        this.capacity=capacity;
        head=new Node(-1,-1);
        tail=new Node(-1,-1);
        map=new HashMap<>();
        //及将头尾来凝结
        head.next=tail;
        tail.prev =head;
    }

    class Node{
        int key;
        int val;
        Node next;
        Node prev;
        public Node(int key,int val){
            this.val=val;
            this.key=key;
        }
    }

    Node head;
    Node tail;
    Map<Integer,Node> map;
    public void insert(Node node){
        node.next=head.next;
        head.next=node;
        node.prev=head;
        node.next.prev=node;
    }

    public void remove(Node node){
        Node tmp=node.next;
        Node pre=node.prev;
        tmp.prev=pre;
        pre.next=tmp;
    }
    public int get(int key){
        if(map.containsKey(key)){
            Node node=map.get(key);
            int val=node.val;
            remove(node);
            insert(node);
            return val;
        }
        return -1;
    }

    public void put(int key,int val){
        if(map.containsKey(key)){
            Node node=map.get(key);
            node.val=val;
            remove(node);
            insert(node);

        }
        else{
            if(map.size()==capacity){
                //delete
                Node deletenode=tail.prev;
                remove(deletenode);
                map.remove(deletenode.key);
            }
            Node newnode=new Node(key, val);
            map.put(key,newnode);
            insert(newnode);
        }
    }
}

public class Main {

    public static void main(String[] args) {
        Scanner scanner=new Scanner(System.in);

        int capacity=scanner.nextInt();
        scanner.nextLine();
        LRUCache cache=new LRUCache(capacity);
        while (scanner.hasNext()) {
            String[] nums=scanner.nextLine().split(" ");
            if(nums[0].equals("put"))
                cache.put(Integer.parseInt(nums[1]),Integer.parseInt(nums[2]));
            else{
                System.out.println(cache.get(Integer.parseInt(nums[1])));
            }

        }
    }
}
```