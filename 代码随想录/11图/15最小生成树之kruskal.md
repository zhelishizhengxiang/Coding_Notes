[53. 寻宝（第七期模拟笔试）](https://kamacoder.com/problempage.php?pid=1053)

如果需要将最小生成树的边进行输出的话，只需要用一个list在join的时候存进list即可
```java
//kruskal最小生成树算法：是从边的角度采用贪心的策略每次寻找权值最小的边作为最小生成树的边
 
//算法流程：直接对所有边进行一次排序，然后从小到大遍历边，如果边的两个点不在同一个集合当中，也就是不在最小生成树当中，那么就将其作为足校生成树的边，如果是同一个集合，那
 
import java.util.*;
 
class Edge{
    int begin;
    int end;
    int val;
 
    public Edge(int begin,int end,int val){
        this.begin=begin;
        this.end=end;
        this.val=val;
    }
}
 
public class Main{
    //并查集模版代码
    static int[] father;
 
    static void init(){
        for(int i=0;i<father.length;i++){
            father[i]=i;
        }
    }
 
    static int find(int u){
        if(father[u]==u)
            return u;
        else{
            father[u]=find(father[u]);
            return father[u];
        }
    }
 
    static void join(int u,int v){
        u=find(u);
        v=find(v);
        if(u==v)
            return;
        father[u]=v;
    }
 
    public static void main(String[] args){
        Scanner scanner=new Scanner(System.in);
        List<Edge> list=new ArrayList<>();
        int v=scanner.nextInt();
        //得到节点数就可以实例化father并且初始化并查集
        father=new int[v+1];
        init();
        int e=scanner.nextInt();
        while(e-->0){
            int begin=scanner.nextInt();
            int end=scanner.nextInt();
            int val=scanner.nextInt();
            list.add(new Edge(begin,end,val));
        }
 
        //给边排序
        Collections.sort(list,(s1,s2)->{
            return s1.val-s2.val;
        });
 
        //遍历集合选边
        int res=0;
        for(Edge edge:list){
            int u=find(edge.begin);
            v=find(edge.end);
 
            //当二者不在一个集合里面时，将其加入到生成树当中
            if(u!=v){
                join(edge.begin,edge.end);
                res+=edge.val;
            }
        }
 
        System.out.println(res);
    }
}

```