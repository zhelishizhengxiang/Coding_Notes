[98. 所有可达路径](https://kamacoder.com/problempage.php?pid=1170)：图论得模板题，给出图然后输出所有可能路径

图的**深度优先遍历其实就是递归方法就是回溯，这与二叉树相同，因为二叉树其实也是一种图  
因为该题是有向图，所以不需要用visited数组来标记

代码如下：

**邻接矩阵版本**
```java
public class Main {
    static List<List<Integer>> result = new ArrayList<>(); // 收集符合条件的路径
    static List<Integer> path = new ArrayList<>(); // 1节点到终点的路径

    public static void dfs(int[][] graph, int x, int n) {
        // 当前遍历的节点x 到达节点n
        if (x == n) { // 找到符合条件的一条路径
            result.add(new ArrayList<>(path));
            return;
        }
        for (int i = 1; i <= n; i++) { // 遍历节点x链接的所有节点
            if (graph[x][i] == 1) { // 找到 x链接的节点
                path.add(i); // 遍历到的节点加入到路径中来
                dfs(graph, i, n); // 进入下一层递归
                path.remove(path.size() - 1); // 回溯，撤销本节点
            }
        }
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int m = scanner.nextInt();

        // 节点编号从1到n，所以申请 n+1 这么大的数组
        int[][] graph = new int[n + 1][n + 1];

        for (int i = 0; i < m; i++) {
            int s = scanner.nextInt();
            int t = scanner.nextInt();
            // 使用邻接矩阵表示无向图，1 表示 s 与 t 是相连的
            graph[s][t] = 1;
        }

        path.add(1); // 无论什么路径已经是从1节点出发
        dfs(graph, 1, n); // 开始遍历

        // 输出结果
        if (result.isEmpty()) System.out.println(-1);
        for (List<Integer> pa : result) {
            for (int i = 0; i < pa.size() - 1; i++) {
                System.out.print(pa.get(i) + " ");
            }
            System.out.println(pa.get(pa.size() - 1));
        }
    }
}
```


邻接表做法
```java
import java.util.*;
public class Main {
    public static List<List<Integer>> res=new ArrayList<>();
    public static List<Integer> path=new ArrayList<>();
    public static void dfs(List<List<Integer>> graph,int n,int node){
        if(node==n){
            res.add(new ArrayList<>(path));
            return;
        }
        //遍历对当前节点的所有邻接点
        for(int i:graph.get(node)){
            path.add(i);
            dfs(graph,n,i);
            path.remove(path.size()-1);
        }
    }
    public static void main(String[] args){
        Scanner scanner=new Scanner(System.in);
        int n=scanner.nextInt();
        int edges=scanner.nextInt();
        //使用邻接表构建图,初始化为n+1个
        List<List<Integer>> graph=new ArrayList<>(n+1);
        //此时还需要对每个节点继续宁初始化
        for(int i=0;i<=n;i++){
            graph.add(new LinkedList<>());
        }
        //读入邻接表
        for(int i=0;i<edges;i++){
            int begin=scanner.nextInt();
            int end=scanner.nextInt();
            //邻接表添加元素
            graph.get(begin).add(end);
             
        }
        path.add(1);
        dfs(graph,n,1);
        if(res.size()==0)
            System.out.println(-1);
        else{
            for(List<Integer> result:res){
                for(int i=0;i<result.size()-1;i++){
                    System.out.print(result.get(i)+" ");
                }
                System.out.println(result.get(result.size()-1));
            }
        }
         
    }
     
}
```