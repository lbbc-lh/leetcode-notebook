## 值传递三要素：
* 状态语义（边界状态、目标状态）
* 递推公式
* 搜索方向（本质上是已知到未知）
## BFS
时间复杂度O(n)
1. 统计入度
2. 出队（出队顺序就是拓扑排序的顺序）
3. 扩展（以下内容按顺序）
* 拓扑排序
* 判断环路
* 递推公式
* 队列类型（**优先队列**，双端队列，普通队列）
#### 509.斐波那契数(FOR)
```
class Solution {
    public int fib(int n) {
      if(n == 0 || n == 1)
      return n;
      int[]dp=new int[n+1];//定义dp数组，因为是0-n，数组长度为n+1
      dp[0]=0;
      dp[1]=1;
      for(int i = 2; i <= n; i++ )
      {
      dp[i]=dp[i-1]+dp[i-2];}
      return dp[n];
    }
}
```
#### 509.斐波那契数（BFS）
```
class Solution {
    public int fib(int n) {
        if(n == 0) return 0;
        if(n == 1) return 1;
        int[] dp = new int[n+1];
        dp[0] = 0;
        dp[1] = 1;
        int[] indeg = new int[n+1];//入度数组
        Arrays.fill(indeg, 2);//定义除了0和1以外的入度都是2
        indeg[0] = 0;
        indeg[1] = 0;
        Queue<Integer> queue = new LinkedList<>();//队列
        queue.offer(0);
        queue.offer(1);
        while(!queue.isEmpty()){
            int x = queue.poll();//出队操作
            if(x == n){
                return dp[x];
            }
            List<Integer> next = x == 0 ? Arrays.asList(2) : Arrays.asList(x+1,x+2);
            for(int y : next){
                if(y > n) continue;
                dp[y] += dp[x];
                indeg[y]--;//每次得到一个结果，入度-1
                if(indeg[y] == 0) queue.offer(y);//入队，条件是如果入度为0，即在队列中的都是已知的
            }
        }
        return 0;
    }
}
```
