K-Sum :star::star::star:
=
题目
-
给定 n 个不同的正整数，整数 k（k <= n）以及一个目标数字 target。　<br>
在这 n 个数里面找出 k 个数，使得这 k 个数的和等于目标数字，求问有多少种方案？<br>

#### Example 1
    输入:
    List = [1,2,3,4]
    k = 2
    target = 5
    输出: 2
    说明: 1 + 4 = 2 + 3 = 5

#### Example 2
    输入:
    List = [1,2,3,4,5]
    k = 3
    target = 6
    输出: 1
    说明: 只有这一种方案。 1 + 2 + 3 = 6
题解
-
最后一步：最后一个数An-1是否选入这K个数中<br>
    
    情况1：An-1不选入，则需要在前n-1个数中选K个数，使得和为target
    情况2：An-1选入，则需要在前n-1个数中选K-1个数，使得和为target-An-1
<br>设f[i][k][s]表示有多少种方法可以在前i个数中选出K个数,使得它们的和为s，转移方程如下：<br>
    
    f[i][k][s] = f[i-1][k][s] + f[i-1][k-1][s-Ai-1]|K>=1 and s>= Ai-1
时间复杂度O(NKTarget)，空间复杂度O(NKTarget)

代码
-
    def kSum(self, A, k, target):
        # write your code here
        n = len(A)
        f = [[[0] * (target + 1) for _ in range(k + 1)] for _ in range(n + 1)]
        
        f[0][0][0] = 1 
        
        for i in range(1, n + 1):
            for k in range(k + 1):
                for s in range(target + 1):
                    f[i][k][s] = f[i-1][k][s]
                    if k >= 1 and s >= A[i - 1]:
                        f[i][k][s] += f[i-1][k-1][s-A[i-1]]
        return f[n][k][target]
