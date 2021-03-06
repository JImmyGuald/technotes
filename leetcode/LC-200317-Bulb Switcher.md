### 问题描述
    初始时有 n 个灯泡关闭。 第 1 轮，你打开所有的灯泡。 第 2 轮，每两个灯泡你关闭一次。 第 3 轮，每三个灯泡切换一次开关（如果关闭则开启，如果开启则关闭）。第 i 轮，每 i 个灯泡切换一次开关。 对于第 n 轮，你只切换最后一个灯泡的开关。 找出 n 轮后有多少个亮着的灯泡。
    示例:
    输入: 3
    输出: 1 
    解释: 
    初始时, 灯泡状态 [关闭, 关闭, 关闭].
    第一轮后, 灯泡状态 [开启, 开启, 开启].
    第二轮后, 灯泡状态 [开启, 关闭, 开启].
    第三轮后, 灯泡状态 [开启, 关闭, 关闭]. 
    你应该返回 1，因为只有一个灯泡还亮着。

### leetcode网址
[题目来源](https://leetcode-cn.com/problems/bulb-switcher/)

### 解法一
    暴力法

### 解法二
    数学方法: 对于第j个灯泡，它会被操作多少次呢？这个要看j的因数的个数了，而判断灯泡是否亮着就是判断j的因数个数是否是奇数，可得，只有完全平方数的因数才会是奇数个，因此只需要去判断1~n有几个完全平方数，so easy！

    public int bulbSwitch(int n) {
        int num = 1;
        while (num*num <= n){
            num++;
        }
        return num - 1;
    }