[94. 城市间货物运输 I](https://kamacoder.com/problempage.php?pid=1152)

![](assets/20SPFA(bellman_ford队列优化版)/file-20250617174733945.png)

所以如果图越稠密，则 SPFA的效率越接近与 Bellman_ford。反之，图越稀疏，SPFA的效率就越高。
```java
import java.util.*;
/**
* bellman_ford算法的队列优化版(SPFA)算法
*/
//邻接表中的节点
class Edge{
    int to;
    int val;
    public Edge(int to,int val){
        this.to=to;
        this.val=val;
    }
}
public class Main{
    public static void main(String[] args){
        Scanner scanner=new Scanner(System.in);
        int n=scanner.nextInt();
        int m=scanner.nextInt();
        //带有权值的邻接表存法
        List<List<Edge>> graph=new ArrayList<>(n+1);
        for(int i=0;i<=n;i++){
            graph.add(new LinkedList<>());
        }
        while(m-->0){
            int s=scanner.nextInt();
            int t=scanner.nextInt();
            int val=scanner.nextInt();
            graph.get(s).add(new Edge(t,val));
        }
        //使用队列进行优化
        Deque<Integer> queue=new ArrayDeque<>();
        //指明起点到节点的最短距离
        int[] mindist=new int[n+1];
        Arrays.fill(mindist,Integer.MAX_VALUE);
        //防止重复元素进入队列，重复节点入队列，下次从队列里取节点的时候，该节点要取很多次，而且都是重复计算。
        boolean[] visited=new boolean[n+1];
        //初始化
        mindist[1]=0;
        queue.offer(1);
        // 记录已经在队列里的元素，已经在队列的元素不用重复加入
        visited[1]=true;
        while(!queue.isEmpty()){
            int node=queue.poll();
            visited[node]=false;
            // 只需要对 上一次松弛的时候更新过的节点作为出发节点所连接的边 进行松弛就够了
            for(Edge edge: graph.get(node)){
                if(mindist[edge.to]>mindist[node]+edge.val){
                    mindist[edge.to]=mindist[node]+edge.val;
                    //松弛之后再看判断是否以及在队列，而不是在松弛的时候一起
                    if(visited[edge.to]==false){
                        queue.offer(edge.to);
                        visited[edge.to]=true;
                    }
                }
                     
            }
        }
        if(mindist[n]==Integer.MAX_VALUE)
            System.out.println("unconnected");
        else
            System.out.println(mindist[n]);
    }
}
```

