一和零 :star::star:
=
题目
-
在计算机界中，我们总是追求用有限的资源获取最大的收益。<br>
现在，假设你分别支配着 m 个 0 和 n 个 1。另外，还有一个仅包含 0 和 1 字符串的数组。<br>
你的任务是使用给定的 m 个 0 和 n 个 1 ，找到能拼出存在于数组中的字符串的最大数量。每个 0 和 1 至多被使用一次。<br>

#### Example 1
    输入: Array = {"10", "0001", "111001", "1", "0"}, m = 5, n = 3
    输出: 4

    解释: 总共 4 个字符串可以通过 5 个 0 和 3 个 1 拼出，即 "10","0001","1","0" 。

#### Example 2
    输入: Array = {"10", "0", "1"}, m = 1, n = 1
    输出: 2

    解释: 你可以拼出 "10"，但之后就没有剩余数字了。更好的选择是拼出 "0" 和 "1" 。
题解
-
设f[i][j][k]为前i个01串最多有多少个被j个0和k个1组成<br><br>
转移方程<br>

    设Si中有ai个0和bi个1
    f[i][j][k] = max{f[i-1][j][k], f[i-1][j-ai-1][k-bi-1] + 1| j >= ai-1 and k >= bi-1}
时间复杂度O(tmn), 空间复杂度O{tmn}<br>

代码
-
    
    def findMaxForm(self, strs, m, n):
        """
        :type strs: List[str]
        :type m: int
        :type n: int
        :rtype: int
        """
        t = len(strs)
        f = [[[0] * (n + 1) for _ in range(m + 1)] for _ in range(t + 1)]
 
        for i in range(1, t + 1):
            a0 = a1 = 0
            for c in strs[i-1]:
                if c == '0':
                    a0 += 1
                else:
                    a1 += 1
            for j in range(m + 1):
                for k in range(n + 1):
                    f[i][j][k] = f[i-1][j][k]
                    if (j >= a0 and k >= a1):
                        f[i][j][k] = max(f[i][j][k], f[i-1][j-a0][k-a1] + 1)
        return f[t][m][n]
<br>
来源：力扣（LeetCode）
