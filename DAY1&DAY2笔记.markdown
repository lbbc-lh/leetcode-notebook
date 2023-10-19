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
* 等待队列→入队代表已经求出，出队代表开始扩展（先序入队|出队）
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
## DFS
* 用堆栈来做，但是不可见。
* 运行时栈→入栈代表开始执行，出栈代表执行结束→先序入栈，后序出栈
#### 509.斐波那契数（DFS）
```
class Solution {
    public int fib(int n) {
        if(n == 0) return 0;
        if(n == 1) return 1;
        int[] dp = new int[n + 1];
        dp[1] = 1;

        //入度统计
        int[] indeg = new int[n + 1];
        Arrays.fill(indeg, 2);
        indeg[0] = 0;
        indeg[1] = 0;

        //队列：队列中的状态点的值已经求出来了
        int[] begin = {0, 1};

        for(int x : begin) dfs(x, n, dp, indeg);
        return dp[n];
    }

    private void dfs(int x, int n, int[] dp, int[] indeg) {
        if(x == n) return;
        int[] next = x == 0 ? new int[]{2} : new int[]{x + 1, x + 2};
        for(int y : next) {
            if(y > n) continue;
            dp[y] += dp[x];
            indeg[y]--;
            if(indeg[y] == 0) dfs(y, n, dp, indeg);
        }
    }
}
```
### 栈模拟手动实现DFS先序代码
**带注释的JS代码**
```
class Solution {
    public int fib(int n) {
        if(n == 0) return 0;
        if(n == 1) return 1;
        int[] dp = new int[n + 1];
        dp[1] = 1;

        //入度统计
        int[] indeg = new int[n + 1];
        Arrays.fill(indeg, 2);
        indeg[0] = 0;
        indeg[1] = 0;

        //队列：队列中的状态点的值已经求出来了
        int[] begin = {0, 1};

        for(int x : begin) dfs(x, n, dp, indeg);
        return dp[n];
    }

    private void dfs(int x, int n, int[] dp, int[] indeg) {
        if(x == n) return;
        int[] next = x == 0 ? new int[]{2} : new int[]{x + 1, x + 2};
        for(int y : next) {
            if(y > n) continue;
            dp[y] += dp[x];
            indeg[y]--;
            if(indeg[y] == 0) dfs(y, n, dp, indeg);
        }
    }
}
```
**JAVA代码**
```
  public class Solution {
    public int fib(int n) {
        if(n == 0) return 0;
        if(n == 1) return 1;
        int[] dp = new int[n+1];
        dp[1] = 1;
        int[] indeg = new int[n+1];
        Arrays.fill(indeg, 2);
        indeg[0] = 0;
        indeg[1] = 0;
        int[] begin = {0,1};
        for(int x : begin) dfs(x,dp,indeg,n);
        return dp[n];
    }
    public void dfs(int x, int[] dp, int[] indeg, int n){
        Stack<int[]> stack = new Stack<>();
        stack.push(new int[]{x,0});
        while(!stack.isEmpty()){
            int[] cur = stack.peek();
            int x1 = cur[0];
            int vis = cur[1];
            boolean case1 = x1 == 0 && vis == 1;
            boolean case2 = x1 != 0 && vis == 2;
            if(!case1 && !case2){
                int next = x1 == 0 ? 2 : x1 + vis + 1;
                cur[1]++;
                if(next > n) continue;
                indeg[next]--;
                dp[next] += dp[x1];
                if(indeg[next] == 0) stack.push(new int[]{next,0});
            }
            else stack.pop();
        }
    }
}
```
