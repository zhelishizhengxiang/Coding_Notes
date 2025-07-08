[94. 城市间货物运输 I](https://kamacoder.com/problempage.php?pid=1152)

**bellman_ford和dijkstra一样都是可以同时计算出起点 到达 所有节点的最短距离，并不只是终点**

 **对所有边松弛一次，相当于只能确定 起点到达 与起点一条边相连的节点 的最短距离**  
![[assets/19bellman_ford算法/file-20250616201934571.png]]

```java
import java.util.*;
public class Main{
    public static void main(String[] args){
        Scanner scanner=new Scanner(System.in);
        int n=scanner.nextInt();
        int m=scanner.nextInt();
        //因为是对所有边进行松弛，所以对于这个有向图直接存每条边即可
        List<int[]> graph=new ArrayList<>();
        while(m-->0){
            int s=scanner.nextInt();
            int t=scanner.nextInt();
            int k=scanner.nextInt();
            graph.add(new int[]{s,t,k});
        }
        // 起点到各个节点的最短距离
        int[] mindist=new int[n+1];
        //初始化为最大，然后从节点1开始
        Arrays.fill(mindist,Integer.MAX_VALUE);
        mindist[1]=0;
        //对所有的边松弛n-1次
        for(int i=1;i<n;i++){
            //对每条边都要继续宁松弛
            for(int[] arr:graph){
                int from=arr[0];
                int to=arr[1];
                int val=arr[2];
                // minDist[from] != INT_MAX 防止从未计算过的节点出发，防止溢出
                if(mindist[from]!=Integer.MAX_VALUE && mindist[to]>mindist[from]+val)
                    mindist[to]=mindist[from]+val;
            }
        }
        if(mindist[n]==Integer.MAX_VALUE)
            System.out.println("unconnected");
        else
            System.out.println(mindist[n]);
    }
}
```