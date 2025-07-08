[47. 参加科学大会（第六期模拟笔试）](https://kamacoder.com/problempage.php?pid=1047)

朴素dijstra算法与prim算法类似，都是从节点的角度去考虑问题


**算法可以同时求 起点到所有节点的最短路径**

**如果需要打印路径的话，做法与prim算法相同，定义一个parent数组去做**
```java
// 朴素Dijkstra算法：在有权图（权值非负数）中求从起点到其他节点的最短路径算法。
//需要注意两点：dijkstra 算法可以同时求 起点到所有节点的最短路径,权值不能为负数
import java.util.*;
public class Main{
    public static void main(String[] args){
        Scanner scanner=new Scanner(System.in);
        int n=scanner.nextInt();
        int edges=scanner.nextInt();
        int[][] grid=new int[n+1][n+1];
        //初始化为最最大
        for(int[] i:grid)
            Arrays.fill(i,Integer.MAX_VALUE);
         
        for(int i=0;i<edges;i++){
            int begin=scanner.nextInt();
            int end=scanner.nextInt();
            grid[begin][end]=scanner.nextInt();
        }
        //标记是否被访问过
        boolean[] visited=new boolean[n+1];
        //该点离源点的最短距离
        int[] minDist=new int[n+1];
        //必须要初始化成最大阿志
        Arrays.fill(minDist,Integer.MAX_VALUE);
        //从节点1开始
        minDist[1]=0;
        //dijkstra三部曲：
        //总共执行n次
        for(int i=0;i<n;i++){
            //第一步，选源点到哪个节点近且该节点未被访问过
            int dist=Integer.MAX_VALUE;
            int cur=-1;
            for(int j=1;j<=n;j++){
                if(minDist[j]<dist && !visited[j]){
                    dist=minDist[j];
                    cur=j;
                }
            }
            //如果cur还是-1说明没找到联通的点了，也就是到达不了重点车站
            if(cur==-1)
                break;
            //第二步，该最近节点被标记访问过
            visited[cur]=true;
            //第三步，更新非访问节点到源点的距离（即更新minDist数组）
            for(int j=1;j<=n;j++){
                //下面如果grid[cur][j]是最大值可能会溢出，所以先判断是否有这条边
                if(!visited[j] && grid[cur][j]!=Integer.MAX_VALUE &&minDist[cur]+grid[cur][j]<minDist[j]){
                    minDist[j]=minDist[cur]+grid[cur][j];
                }
            }
        }
        if(minDist[n]==Integer.MAX_VALUE)
            System.out.println(-1);
        else
            System.out.println(minDist[n]);
    }
}
```