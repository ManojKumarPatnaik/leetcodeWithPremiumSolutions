# [2951. 找出峰值](https://leetcode.cn/problems/find-the-peaks)

[English Version](/solution/2900-2999/2951.Find%20the%20Peaks/README_EN.md)

## 题目描述

<!-- 这里写题目描述 -->

<p>给你一个下标从 <strong>0</strong> 开始的数组 <code>mountain</code> 。你的任务是找出数组&nbsp;<code>mountain</code> 中的所有 <strong>峰值</strong>。</p>

<p>以数组形式返回给定数组中 <strong>峰值</strong> 的下标，<strong>顺序不限</strong> 。</p>

<p><strong>注意：</strong></p>

<ul>
	<li><strong>峰值</strong> 是指一个严格大于其相邻元素的元素。</li>
	<li>数组的第一个和最后一个元素 <strong>不</strong> 是峰值。</li>
</ul>

<p>&nbsp;</p>

<p><strong class="example">示例 1：</strong></p>

<pre>
<strong>输入：</strong>mountain = [2,4,4]
<strong>输出：</strong>[]
<strong>解释：</strong>mountain[0] 和 mountain[2] 不可能是峰值，因为它们是数组的第一个和最后一个元素。
mountain[1] 也不可能是峰值，因为它不严格大于 mountain[2] 。
因此，答案为 [] 。
</pre>

<p><strong class="example">示例 2：</strong></p>

<pre>
<strong>输入：</strong>mountain = [1,4,3,8,5]
<strong>输出：</strong>[1,3]
<strong>解释：</strong>mountain[0] 和 mountain[4] 不可能是峰值，因为它们是数组的第一个和最后一个元素。
mountain[2] 也不可能是峰值，因为它不严格大于 mountain[3] 和 mountain[1] 。
但是 mountain[1] 和 mountain[3] 严格大于它们的相邻元素。
因此，答案是 [1,3] 。
</pre>

<p>&nbsp;</p>

<p><strong>提示：</strong></p>

<ul>
	<li><code>3 &lt;= mountain.length &lt;= 100</code></li>
	<li><code>1 &lt;= mountain[i] &lt;= 100</code></li>
</ul>

## 解法

<!-- 这里可写通用的实现逻辑 -->

<!-- tabs:start -->

### **Python3**

<!-- 这里可写当前语言的特殊实现逻辑 -->

```python
class Solution:
    def findPeaks(self, mountain: List[int]) -> List[int]:
        return [
            i
            for i in range(1, len(mountain) - 1)
            if mountain[i - 1] < mountain[i] > mountain[i + 1]
        ]
```

### **Java**

<!-- 这里可写当前语言的特殊实现逻辑 -->

```java
class Solution {
    public List<Integer> findPeaks(int[] mountain) {
        List<Integer> ans = new ArrayList<>();
        for (int i = 1; i < mountain.length - 1; ++i) {
            if (mountain[i - 1] < mountain[i] && mountain[i + 1] < mountain[i]) {
                ans.add(i);
            }
        }
        return ans;
    }
}
```

### **C++**

```cpp
class Solution {
public:
    vector<int> findPeaks(vector<int>& mountain) {
        vector<int> ans;
        for (int i = 1; i < mountain.size() - 1; ++i) {
            if (mountain[i - 1] < mountain[i] && mountain[i + 1] < mountain[i]) {
                ans.push_back(i);
            }
        }
        return ans;
    }
};
```

### **Go**

```go
func findPeaks(mountain []int) (ans []int) {
	for i := 1; i < len(mountain)-1; i++ {
		if mountain[i-1] < mountain[i] && mountain[i+1] < mountain[i] {
			ans = append(ans, i)
		}
	}
	return
}
```

### **TypeScript**

```ts
function findPeaks(mountain: number[]): number[] {
    const ans: number[] = [];
    for (let i = 1; i < mountain.length - 1; ++i) {
        if (mountain[i - 1] < mountain[i] && mountain[i + 1] < mountain[i]) {
            ans.push(i);
        }
    }
    return ans;
}
```

### **...**

```

```

<!-- tabs:end -->
