### 问题描述
![desc](https://jayden-doc.oss-cn-shenzhen.aliyuncs.com/leetcode/LD-20012101.png '')

### 解法一
    求以每根柱子的高为高度的最大矩形面积，遍历每根柱子，求出最大面积
    以柱子的高为高度的最大矩形面积 = (右边离该柱子最近且比该柱子矮的柱子小标-左边离该柱子最近比该柱子矮的柱子小标-1)*柱子高度

    public int largestRectangleArea(int[] heights) {
        int left[] = new int[heights.length];
        int right[] = new int[heights.length];
        for(int i=0;i<heights.length;i++){
            if(i == 0) {
                left[i] = -1;
                continue;
            }
            int l = i-1;
            while(l > -1 && heights[l] >= heights[i]){
                l--;
            }
            left[i] = l;
        }
        for(int i=0;i<heights.length;i++){
            if(i == heights.length-1) {
                right[i] = heights.length;
                continue;
            }
            int r = i+1;
            while(r < heights.length && heights[r] >= heights[i]){
                r++;
            }
            right[i] = r;
        }
        int maxArea = 0;
        for(int i=0;i<heights.length;i++){
            int area = (right[i] - left[i] - 1) * heights[i];
            if(maxArea < area) maxArea = area;
        }
        return maxArea;
    }


### 解法二
    跟解法一类似，主要是优化当左边或右边大的时候直接跳到该下标的left[n]或right[n]
   
    public int largestRectangleArea(int[] heights) {
        int left[] = new int[heights.length];
        int right[] = new int[heights.length];
        for(int i=0;i<heights.length;i++){
            if(i == 0) {
                left[i] = -1;
                continue;
            }
            int l = i-1;
            while(l > -1 && heights[l] >= heights[i]){
                l = left[l];
            }
            left[i] = l;
        }
        for(int i=heights.length-1;i>=0;i--){
            if(i == heights.length-1) {
                right[i] = heights.length;
                continue;
            }
            int r = i+1;
            while(r < heights.length && heights[r] >= heights[i]){
                r = right[r];
            }
            right[i] = r;
        }
        int maxArea = 0;
        for(int i=0;i<heights.length;i++){
            int area = (right[i] - left[i] - 1) * heights[i];
            if(maxArea < area) maxArea = area;
        }
        return maxArea;
    }

### 解法三
    使用栈来实现，如果


