最小调整代价 :star::star::star:
=
题目
-
给一个整数数组，调整每个数的大小，使得相邻的两个数的差不大于一个给定的整数target，调整每个数的代价为调整前后的差的绝对值，求调整代价之和最小是多少。数组中的每个元素都小于等于100<br>

#### Example 1
    输入: A = [1,4,2,3], target=1
    输出:  2(B = [2,3,2,3])
题解
-
最后一步：将A改成B，A[n-1]改成X，这一步代价是|A[n-1]-X|<br>
需要确保|X-B[n-2]| < = target <br>
<br>
设f[i][j]为将A的前i个元素改成B的最小代价，确保前i个改好的元素中任意两个相邻的元素的差不超过target，并且A[i-1]改成j<br>
<br>
转移方程

    f[i][j] = min{f[i-1][k] + |j - A[i-1]|}, j-target <= k <= j + target, 1<= k <= 100
代码
-
    
    def MinAdjustmentCost(self, A, target):
        # write your code here

        n = len(A)
        if n == 0:
            return 0
            
        f = [[float("inf")] * (100 + 1) for _ in range(n + 1)]
        
        for i in range(1, 101):
            f[1][i] = abs(A[0] - i)
        
        for i in range(2, n + 1):
            for j in range(1, 101):
                for k in range(j-target, j+target+1):
                    if (k < 1 or k > 100):
                        continue
                    f[i][j] = min(f[i][j], f[i-1][k]) 
                f[i][j] += abs(j - A[i-1])
        return min(f[n])
                    
                    
