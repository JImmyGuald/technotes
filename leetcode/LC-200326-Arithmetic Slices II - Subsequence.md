### 问题描述
    数组 A 包含 N 个数，且索引从 0 开始。该数组子序列将划分为整数序列 (P0, P1, ..., Pk)，P 与 Q 是整数且满足 0 ≤ P0 < P1 < ... < Pk < N。
    如果序列 A[P0]，A[P1]，...，A[Pk-1]，A[Pk] 是等差的，那么数组 A 的子序列 (P0，P1，…，PK) 称为等差序列。值得注意的是，这意味着 k ≥ 2。
    函数要返回数组 A 中所有等差子序列的个数。
    输入包含 N 个整数。每个整数都在 -2^31 和 2^31-1 之间，另外 0 ≤ N ≤ 1000。保证输出小于 2^31-1。
    示例：
    输入：[2, 4, 6, 8, 10]
    输出：7
    解释：
    所有的等差子序列为：
    [2,4,6]
    [4,6,8]
    [6,8,10]
    [2,4,6,8]
    [4,6,8,10]
    [2,4,6,8,10]
    [2,6,10]

### leetcode网址
[题目来源](https://leetcode-cn.com/problems/arithmetic-slices-ii-subsequence)

### 解法一
    动态规划：dp[i][k]表示以A[i]结尾的，公差为k的等差数列个数
    状态方程：dp[i][A[i]-A[j]] += dp[j][A[i]-A[j]] (0 <= j < i)
    初始化的时候如果dp[i][k]都置为0，那还是解决不了问题，这个时候需要引入
    弱等差数列，即至少有两个数而且相邻的数差是一致的，
    状态方程变为：dp[i][A[i]-A[j]] += dp[j][A[i]-A[j]] + 1 (0 <= j < i)

    public int numberOfArithmeticSlices(int[] A) {
        Map<Integer,Integer>[] dp = new Map[A.length];
        int result = 0;
        for(int i=0;i<A.length;i++){
            dp[i] = new HashMap<>();
            for(int j=0;j<i;j++){
                long minus = (long)A[i] - (long)A[j];
                if(minus<Integer.MIN_VALUE || minus>Integer.MAX_VALUE){
                    continue;
                }
                int diff = (int) minus;
                int sum = dp[j].getOrDefault(diff,0);
                int origin = dp[i].getOrDefault(diff,0);
                dp[i].put(diff,origin+sum+1);
                result += sum;
            }
        }
        return result;
    }