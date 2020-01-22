### 问题描述
![desc](https://jayden-doc.oss-cn-shenzhen.aliyuncs.com/leetcode/LC-20012201.png '')

### leetcode网址
[题目来源](https://leetcode-cn.com/problems/maximal-rectangle/)


### 解法一
    暴力法，遍历每个节点，以节点为矩形的右下点，然后从下往上根据矩形的高不同求出每个高度下的最大矩形，这里有一个优化点，可以先求出每个节点往左连续为1的个数

    public int maximalRectangle(char[][] matrix) {
        if(matrix.length>0) {
            int[][] a = new int[matrix.length][matrix[0].length];
            for (int i = 0; i < matrix.length; i++) {
                for (int j = 0; j < matrix[0].length; j++) {
                    if (j == 0) {
                        a[i][j] = matrix[i][j] - '0';
                        continue;
                    }
                    if (matrix[i][j] == '1') a[i][j] = a[i][j - 1] + 1;
                    else a[i][j] = 0;
                }
            }
            int maxArea = 0;
            for (int i = 0; i < matrix.length; i++) {
                for (int j = 0; j < matrix[0].length; j++) {
                    if (matrix[i][j] == '1') {
                        int minLength = Integer.MAX_VALUE;
                        for (int k = i; k > -1; k--) {
                            if (matrix[k][j] == '1') {
                                minLength = Math.min(minLength, a[k][j]);
                                int area = (i - k + 1) * minLength;
                                if (area > maxArea) maxArea = area;
                            } else {
                                break;
                            }
                        }
                    }
                }
            }
            return maxArea;
        }else {
            return 0;
        }
    }

    时间复杂度：O(n*n*m)
    空间复杂度：O(n*m)

### 解法二
    转换法，将二维数组逐行统计转换成为类似于leetcode中的https://leetcode-cn.com/problems/largest-rectangle-in-histogram的题目，然后使用栈的方式求的最大面积

    public int maximalRectangle(char[][] matrix) {
        int maxArea = 0;
        if(matrix.length>0) {
            int[] heights = new int[matrix[0].length];
            for (int i = 0; i < matrix.length; i++) {
                for (int j = 0; j < matrix[0].length; j++) {
                    if (matrix[i][j] == '0') heights[j] = 0;
                    else heights[j] = heights[j] + 1;
                }
                int area = largestRectangleArea(heights);
                if (area > maxArea) maxArea = area;
            }
        }
        return maxArea;
    }

    public int largestRectangleArea(int[] heights) {
        Stack<Integer> stack = new Stack<>();
        stack.push(-1);
        int maxArea = 0;
        for(int i=0;i<heights.length;i++){
            while (stack.peek()!=-1 && heights[i] <= heights[stack.peek()]){
                int area = heights[stack.pop()] * (i-stack.peek()-1);
                System.out.println(area);
                if(area > maxArea) maxArea = area;
            }
            stack.push(i);
        }
        while (stack.peek()!=-1){
            int area = heights[stack.pop()] * (heights.length-stack.peek()-1);
            if(area > maxArea) maxArea = area;
        }
        return maxArea;
    }

    时间复杂度：O(n*m)
    空间复杂度：O(n)

### 解法三
    同样的类比leetcode之前的题目，用数组来实现

    public int maximalRectangle(char[][] matrix) {
        int maxArea = 0;
        if(matrix.length>0) {
            int[] heights = new int[matrix[0].length];
            for (int i = 0; i < matrix.length; i++) {
                for (int j = 0; j < matrix[0].length; j++) {
                    if (matrix[i][j] == '0') heights[j] = 0;
                    else heights[j] = heights[j] + 1;
                }
                int area = largestRectangleArea(heights);
                if (area > maxArea) maxArea = area;
            }
        }
        return maxArea;
    }

    public static int largestRectangleArea(int[] heights) {
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

    时间复杂度：O(n*m)
    空间复杂度：O(n)

### 解法四
    对解法三的优化，使用动态规划实现，第i层和第i+1层是存在某种关系的，如果i+1层存在0，则需要比较left[i]和0下标大小，right[i]也一样

    public static int maximalRectangle(char[][] matrix) {
        int maxArea = 0;
        if(matrix.length>0) {
            int[] heights = new int[matrix[0].length];
            int[] left = new int[heights.length];
            int[] right = new int[heights.length];
            Arrays.fill(left,-1);
            Arrays.fill(right, matrix[0].length);
            for (int i = 0; i < matrix.length; i++) {
                for (int j = 0; j < matrix[0].length; j++) {
                    if (matrix[i][j] == '0') heights[j] = 0;
                    else heights[j] = heights[j] + 1;
                }
                int binary = -1;//上次为0的下标
                for (int j = 0; j < matrix[0].length; j++) {
                    if(matrix[i][j] == '1'){
                        left[j] = Math.max(left[j], binary);
                    }else{
                        left[j] = -1;
                        binary = j;
                    }
                }
                binary = matrix[0].length;
                for (int j = matrix[0].length-1; j >= 0; j--) {
                    if(matrix[i][j] == '1'){
                        right[j] = Math.min(right[j], binary);
                    }else{
                        right[j] = matrix[0].length;
                        binary = j;
                    }
                }
                for(int k=0;k<heights.length;k++){
                    int area = (right[k] - left[k] - 1) * heights[k];
                    if(maxArea < area) maxArea = area;
                }
            }
        }
        return maxArea;
    }

    时间复杂度：O(n*m)
    空间复杂度：O(n)