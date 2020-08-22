正则表达式匹配 :star::star::star:
=
题目
-
给你一个字符串 s 和一个字符规律 p，请你来实现一个支持 '.' 和 '*' 的正则表达式匹配。<br>

    '.'匹配任意单个字符
    '\*'匹配另个或多个前面的那一个元素

#### Example 1

    输入:
    s = "aa"
    p = "a"
    输出: false
    解释: "a" 无法匹配 "aa" 整个字符串。
#### Example 2
    输入：
    s = "aa"
    p = "a*"
    输出: true
    解释: 因为 '*' 代表可以匹配零个或多个前面的那一个元素, 在这里前面的元素就是 'a'。因此，字符串 "aa" 可被视为 'a' 重复了一次。
#### Example 3
    输入:
    s = "ab"
    p = ".*"
    输出: true
    解释: ".*" 表示可匹配零个或多个（'*'）任意字符（'.'）。
#### Example 4
    输入:
    s = "aab"
    p = "c*a*b"
    输出: true
    解释: 因为 '*' 表示零个或多个，这里 'c' 为 0 个, 'a' 被重复一次。因此可以匹配字符串 "aab"。
题解
-
**双序列动态规划**<br>
s为字符串，p为正则表达式。我们需要关注最后一步，即关注s和p的最后的字符，可分为以下三种情况。<br>
（1）p[n-1]是一个正常字符，且s[m-1]=p[n-1]，则能否匹配取决于s[0...m-2]和p[0...n-2]能否匹配。<br>
（2）p[n-1]是'.'，则s[m-1]一定是和'.'匹配，之后取决于s[0...m-2]和p[0...n-2]是否匹配。<br>
（3）p[n-1]是'\*'，如果s[m-1]是0个c，则取决于s[m-1]和p[n-3]是否匹配。<br>
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;如果s[m-1]是多个c的最后一个，则取决于s[m-2]和p[n-1]是否匹配。<br>
<br>
设f[i][j]为s的前i个字符和p的前j个字符能否匹配。转移方程如下：<br>

    f[i][j]=f[i-1][j-1], 如果i>0，并且p[j-1]='.'或者s[i-1]=p[j-1]
    f[i][j]=f[i][j-2] or (f[i-1][j] and (p[j-2]='.' or p[j-2]=s[i-2])), p[j-1]='\*'
时间复杂度O(mn)，空间复杂度O(mn)

代码
-
    class Solution(object):
    def isMatch(self, s, p):
        """
        :type s: str
        :type p: str
        :rtype: bool
        """
        # 如果B[n-1]是一个正常字符,且A[m-1]=B[n-1],则能否匹配取决于A[0...m-2]和B[0...n-2]是否匹配
        # 如果B[n-1]是'.',则A[m-1]一定是和'.'匹配，之后取决于A[0...m-2]和B[0...n-2]是否匹配
        # 如果B[n-1]是'*',
        #                A[m-1]是0个c，取决于A[0...m-1]和B[0...n-3]是否匹配
        #                A[m-1]是多个c,取决于A[0...m-2]和B[0...n-1]是否匹配
        # 设f[i][j]为A的前i个字符A[0...i-1]和B的前i个字符B[0...j-1]是否匹配 
        m = len(s)
        n = len(p)
        f = [[False] * (n + 1) for _ in range(m + 1)]
        for i in range(m + 1):
            for j in range(n + 1):
                if (i == 0 and j == 0):
                    f[i][j] = True
                    continue
                
                # i > 0
                if (j == 0):
                    f[i][j] == False
                    continue
                
                # j > 0
                if (p[j - 1] != '*'):
                    if (i > 0 and (s[i-1] == p[j-1] or p[j-1] == '.')):
                        f[i][j] = f[i][j] or f[i-1][j-1]
                else:
                    if (j > 1):
                        f[i][j] = f[i][j] or f[i][j-2]
                    if (i > 0 and j > 1):
                        if (p[j-2] == '.' or s[i-1] == p[j-2]):
                            f[i][j] = f[i][j] or f[i-1][j]
        return f[m][n]
<br>
来源：力扣（LeetCode）
