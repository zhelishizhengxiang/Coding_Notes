
[460. LFU 缓存 - 力扣（LeetCode）](https://leetcode.cn/problems/lfu-cache/description/)

使用双map来解决。其中一个存所有的缓存节点，一个存频次-双向链表。双向链表使用linkeHashSet来解决
```java
class LFUCache {
    class Node {
        int key;
        int val;
        int freq;//频次
        Node next;
        Node pre;
        public Node(){}
        public Node(int key,int val){
            this.val=val;
            this.key=key;
            this.freq=1;
        }
    }

    Map<Integer,Node> cache;//存储缓存内容
    Map<Integer,LinkedHashSet<Node>> freqMap; //存储每个频次对应的双向链表
    int min;//当前的最小频次
    int capacity;

    public LFUCache(int capacity) {
        cache=new HashMap<>();
        freqMap=new HashMap<>();
        int min=-1;
        this.capacity=capacity;
    }

    public int get(int key) {
        if(!cache.containsKey(key))
            return -1;
        Node node=cache.get(key);
        //增加频率
        freqInc(node);
        return node.val;
    }

    public void put(int key, int value) {
        if(cache.containsKey(key)){
            //update
            Node node=cache.get(key);
            node.val=value;
            freqInc(node);
            return ;
        }
        //insert
        if(cache.size()==capacity){
            LinkedHashSet<Node> lowSet=freqMap.get(min);
            Node deleteNode=lowSet.iterator().next();
            lowSet.remove(deleteNode);
            cache.remove(deleteNode.key);
        }
        Node newNode=new Node(key,value);
        LinkedHashSet<Node> set=freqMap.get(1);
        if(set==null){
            set=new LinkedHashSet<>();
            freqMap.put(1,set);
        }
        set.add(newNode);
        cache.put(key,newNode);
        min=1;
    }

    public void freqInc(Node node){
        //从原来的freq对应的链表中移除，并更新min
        LinkedHashSet<Node> set=freqMap.get(node.freq);
        set.remove(node);
        if(set.size()==0 && min==node.freq){
            min=node.freq+1;
        }
        node.freq++;
        set=freqMap.get(node.freq);
        if(set==null){
            set=new LinkedHashSet<>();
            freqMap.put(node.freq,set);
        }
        set.add(node);
    }
}

/**
 * Your LFUCache object will be instantiated and called as such:
 * LFUCache obj = new LFUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```