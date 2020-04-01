### 问题描述
    难度：中等
    给定一个从1 到 n 排序的整数列表。
    首先，从左到右，从第一个数字开始，每隔一个数字进行删除，直到列表的末尾。
    第二步，在剩下的数字中，从右到左，从倒数第一个数字开始，每隔一个数字进行删除，直到列表开头。
    我们不断重复这两步，从左到右和从右到左交替进行，直到只剩下一个数字。
    返回长度为 n 的列表中，最后剩下的数字。

    示例：

    输入:
    n = 9,
    1 2 3 4 5 6 7 8 9
    2 4 6 8
    2 6
    6

    输出:
    6

### leetcode网址
[题目来源](https://leetcode-cn.com/problems/elimination-game//)

### 解法一
    只需要每轮删除之后的第一个数first，等差数列的公差diff和总数count，知道count为1。每一轮删除diff都会翻倍，count都会是原来的一半，first要根据删除的方向来定，如果是从左往右删除，first会变成first+diff，如果是从右往左，要看count的奇偶性，如果count是奇数first变成first+diff，如果count是偶数first不变

    public int lastRemaining(int n) {
        int first = 1,diff = 1,count = n;
        boolean leftFlag = true;
        while(count>1){
            if(leftFlag){
                first += diff;
                leftFlag = false;
            }else{
                if(count%2==1){
                    first += diff;
                }
                leftFlag = true;
            }
            diff *= 2;
            count /= 2;
        }
        return first;
    }