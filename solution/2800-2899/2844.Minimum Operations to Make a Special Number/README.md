# [2844. 生成特殊数字的最少操作](https://leetcode.cn/problems/minimum-operations-to-make-a-special-number)

[English Version](/solution/2800-2899/2844.Minimum%20Operations%20to%20Make%20a%20Special%20Number/README_EN.md)

## 题目描述

<!-- 这里写题目描述 -->

<p>给你一个下标从 <strong>0</strong> 开始的字符串 <code>num</code> ，表示一个非负整数。</p>

<p>在一次操作中，您可以选择 <code>num</code> 的任意一位数字并将其删除。请注意，如果你删除 <code>num</code> 中的所有数字，则 <code>num</code> 变为 <code>0</code>。</p>

<p>返回最少需要多少次操作可以使 <code>num</code> 变成特殊数字。</p>

<p>如果整数 <code>x</code> 能被 <code>25</code> 整除，则该整数 <code>x</code> 被认为是特殊数字。</p>

<p>&nbsp;</p>

<p>&nbsp;</p>

<p><strong>示例 1：</strong></p>

<pre>
<strong>输入：</strong>num = "2245047"
<strong>输出：</strong>2
<strong>解释：</strong>删除数字 num[5] 和 num[6] ，得到数字 "22450" ，可以被 25 整除。
可以证明要使数字变成特殊数字，最少需要删除 2 位数字。</pre>

<p><strong>示例 2：</strong></p>

<pre>
<strong>输入：</strong>num = "2908305"
<strong>输出：</strong>3
<strong>解释：</strong>删除 num[3]、num[4] 和 num[6] ，得到数字 "2900" ，可以被 25 整除。
可以证明要使数字变成特殊数字，最少需要删除 3 位数字。</pre>

<p><strong>示例 3：</strong></p>

<pre>
<strong>输入：</strong>num = "10"
<strong>输出：</strong>1
<strong>解释：</strong>删除 num[0] ，得到数字 "0" ，可以被 25 整除。
可以证明要使数字变成特殊数字，最少需要删除 1 位数字。
</pre>

<p>&nbsp;</p>

<p><strong>提示</strong></p>

<ul>
	<li><code>1 &lt;= num.length &lt;= 100</code></li>
	<li><code>num</code> 仅由数字 <code>'0'</code> 到 <code>'9'</code> 组成</li>
	<li><code>num</code> 不含任何前导零</li>
</ul>

## 解法

<!-- 这里可写通用的实现逻辑 -->

<!-- tabs:start -->

### **Python3**

<!-- 这里可写当前语言的特殊实现逻辑 -->

```python
class Solution:
    def minimumOperations(self, num: str) -> int:
        @cache
        def dfs(i: int, k: int) -> int:
            if i == n:
                return 0 if k == 0 else n
            ans = dfs(i + 1, k) + 1
            ans = min(ans, dfs(i + 1, (k * 10 + int(num[i])) % 25))
            return ans

        n = len(num)
        return dfs(0, 0)
```

### **Java**

<!-- 这里可写当前语言的特殊实现逻辑 -->

```java
class Solution {
    private Integer[][] f;
    private String num;
    private int n;

    public int minimumOperations(String num) {
        n = num.length();
        this.num = num;
        f = new Integer[n][25];
        return dfs(0, 0);
    }

    private int dfs(int i, int k) {
        if (i == n) {
            return k == 0 ? 0 : n;
        }
        if (f[i][k] != null) {
            return f[i][k];
        }
        f[i][k] = dfs(i + 1, k) + 1;
        f[i][k] = Math.min(f[i][k], dfs(i + 1, (k * 10 + num.charAt(i) - '0') % 25));
        return f[i][k];
    }
}
```

### **C++**

```cpp
class Solution {
public:
    int minimumOperations(string num) {
        int n = num.size();
        int f[n][25];
        memset(f, -1, sizeof(f));
        function<int(int, int)> dfs = [&](int i, int k) -> int {
            if (i == n) {
                return k == 0 ? 0 : n;
            }
            if (f[i][k] != -1) {
                return f[i][k];
            }
            f[i][k] = dfs(i + 1, k) + 1;
            f[i][k] = min(f[i][k], dfs(i + 1, (k * 10 + num[i] - '0') % 25));
            return f[i][k];
        };
        return dfs(0, 0);
    }
};
```

### **Go**

```go
func minimumOperations(num string) int {
	n := len(num)
	f := make([][25]int, n)
	for i := range f {
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	var dfs func(i, k int) int
	dfs = func(i, k int) int {
		if i == n {
			if k == 0 {
				return 0
			}
			return n
		}
		if f[i][k] != -1 {
			return f[i][k]
		}
		f[i][k] = dfs(i+1, k) + 1
		f[i][k] = min(f[i][k], dfs(i+1, (k*10+int(num[i]-'0'))%25))
		return f[i][k]
	}
	return dfs(0, 0)
}

func min(a, b int) int {
	if a < b {
		return a
	}
	return b
}
```

### **TypeScript**

```ts
function minimumOperations(num: string): number {
    const n = num.length;
    const f: number[][] = Array.from({ length: n }, () => Array.from({ length: 25 }, () => -1));
    const dfs = (i: number, k: number): number => {
        if (i === n) {
            return k === 0 ? 0 : n;
        }
        if (f[i][k] !== -1) {
            return f[i][k];
        }
        f[i][k] = dfs(i + 1, k) + 1;
        f[i][k] = Math.min(f[i][k], dfs(i + 1, (k * 10 + Number(num[i])) % 25));
        return f[i][k];
    };
    return dfs(0, 0);
}
```

### **...**

```

```

<!-- tabs:end -->