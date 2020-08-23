通配符匹配 :star::star::star:
=
题目
-
给定一个字符串 (s) 和一个字符模式 (p) ，实现一个支持 '?' 和 '\*' 的通配符匹配。<br>

    '?' 可以匹配任何单个字符。
    '\*' 可以匹配任意字符串（包括空字符串）。
#### Example 1
    输入:
    s = "aa"
    p = "a"
    输出: false
    解释: "a" 无法匹配 "aa" 整个字符串。
#### Example 2
    输入:
    s = "aa"
    p = "*"
    输出: true
    解释: '*' 可以匹配任意字符串。
#### Example 3
    输入:
    s = "acdcb"
    p = "a*c?b"
    输出: false
    
题解
-
**双序列型动态规划**<br>
（1）如果p[n-1]是一个正常字符，则如果s[m-1]=p[n-1]，能否匹配取决于s[0...m-2]和p[0...n-2]是否匹配<br>
（2）如果B[n-1]是'?'，则s[m-1]一定可以与之匹配，之后取决于s[0...m-2]和p[0...n-2]<br>
（3）如果p[n-1]是'\*'，如果s[m-1]没有与之匹配，则取决于s[0...m-1]和p[0...n-2]。若s[m-1]被'\*'匹配，之后取决于s[0...m-2]和p[0...n-1]是否匹配<br>
##### 转移方程
设f[i][j]为s的前i个字符s[0...i-1]和p的前j个字符p[0...j-1]能否匹配<br>

        f[i][]j = f[i-1][j-1], 并且i>1,p[j-1] = '?'或者s[i-1]=p[j-1]
        f[i][j] = f[i-1][j] or f[i][j-1]，如果p[j-1]='\*'
代码
-
    class Solution(object):
    def isMatch(self, s, p):
        """
        :type s: str
        :type p: str
        :rtype: bool
        """
        m = len(s)
        n = len(p)
        f = [[False] * (n + 1) for _ in range(m + 1)]
        for i in range(m + 1):
            for j in range(n + 1):
                if (i == 0 and j == 0):
                    f[i][j] = True
                    continue
                if (j == 0):
                    # i > 0
                    f[i][j] = False
                    continue
                if p[j - 1] != '*':
                    if (i > 0 and (s[i-1] == p[j-1] or p[j-1] == "?")):
                        f[i][j] = f[i][j] or f[i-1][j-1]
                else:
                    f[i][j] = f[i][j] or f[i][j-1]
                    if (i > 0):
                        f[i][j] = f[i][j] or f[i-1][j]
        return f[m][n]
来源：力扣(LeetCode)
