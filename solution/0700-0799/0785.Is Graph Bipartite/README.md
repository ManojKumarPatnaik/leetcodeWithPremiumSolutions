# [785. 判断二分图](https://leetcode.cn/problems/is-graph-bipartite)

[English Version](/solution/0700-0799/0785.Is%20Graph%20Bipartite/README_EN.md)

## 题目描述

<!-- 这里写题目描述 -->

存在一个 <strong>无向图</strong> ，图中有 <code>n</code> 个节点。其中每个节点都有一个介于 <code>0</code> 到 <code>n - 1</code> 之间的唯一编号。给你一个二维数组 <code>graph</code> ，其中 <code>graph[u]</code> 是一个节点数组，由节点 <code>u</code> 的邻接节点组成。形式上，对于  <code>graph[u]</code> 中的每个 <code>v</code> ，都存在一条位于节点 <code>u</code> 和节点 <code>v</code> 之间的无向边。该无向图同时具有以下属性：

<ul>
	<li>不存在自环（<code>graph[u]</code> 不包含 <code>u</code>）。</li>
	<li>不存在平行边（<code>graph[u]</code> 不包含重复值）。</li>
	<li>如果 <code>v</code> 在 <code>graph[u]</code> 内，那么 <code>u</code> 也应该在 <code>graph[v]</code> 内（该图是无向图）</li>
	<li>这个图可能不是连通图，也就是说两个节点 <code>u</code> 和 <code>v</code> 之间可能不存在一条连通彼此的路径。</li>
</ul>

<p><strong>二分图</strong> 定义：如果能将一个图的节点集合分割成两个独立的子集 <code>A</code> 和 <code>B</code> ，并使图中的每一条边的两个节点一个来自 <code>A</code> 集合，一个来自 <code>B</code> 集合，就将这个图称为 <strong>二分图</strong> 。</p>

<p>如果图是二分图，返回 <code>true</code><em> </em>；否则，返回 <code>false</code> 。</p>

<p> </p>

<p><strong>示例 1：</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0785.Is%20Graph%20Bipartite/images/bi2.jpg" style="width: 222px; height: 222px;" />
<pre>
<strong>输入：</strong>graph = [[1,2,3],[0,2],[0,1,3],[0,2]]
<strong>输出：</strong>false
<strong>解释：</strong><code>不能将节点分割成两个独立的子集，</code>以使每条边都连通一个子集中的一个节点与另一个子集中的一个节点。</pre>

<p><strong>示例 2：</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0785.Is%20Graph%20Bipartite/images/bi1.jpg" style="width: 222px; height: 222px;" />
<pre>
<strong>输入：</strong>graph = [[1,3],[0,2],[1,3],[0,2]]
<strong>输出：</strong>true
<strong>解释：</strong><code>可以将节点分成两组: {0, 2} 和 {1, 3} 。</code></pre>

<p> </p>

<p><strong>提示：</strong></p>

<ul>
	<li><code>graph.length == n</code></li>
	<li><code>1 <= n <= 100</code></li>
	<li><code>0 <= graph[u].length < n</code></li>
	<li><code>0 <= graph[u][i] <= n - 1</code></li>
	<li><code>graph[u]</code> 不会包含 <code>u</code></li>
	<li><code>graph[u]</code> 的所有值 <strong>互不相同</strong></li>
	<li>如果 <code>graph[u]</code> 包含 <code>v</code>，那么 <code>graph[v]</code> 也会包含 <code>u</code></li>
</ul>

## 解法

<!-- 这里可写通用的实现逻辑 -->

**方法一：染色法判定二分图**

遍历所有节点进行染色，比如初始为白色，DFS 对节点相邻的点染上另外一种颜色。如果要染色某节点时，要染的目标颜色和该节点的已经染过的颜色不同，则说明不能构成二分图。

**方法二：并查集**

对于本题，如果是二分图，那么图中每个顶点的所有邻接点都应该属于同一集合，且不与顶点处于同一集合，因此我们可以使用并查集。遍历图中每个顶点，如果发现存在当前顶点与对应的邻接点处于同一个集合，说明不是二分图。否则将当前节点的邻接点相互进行合并。以下是并查集模板。

模板 1——朴素并查集：

```python
# 初始化，p存储每个点的父节点
p = list(range(n))


# 返回x的祖宗节点
def find(x):
    if p[x] != x:
        # 路径压缩
        p[x] = find(p[x])
    return p[x]


# 合并a和b所在的两个集合
p[find(a)] = find(b)
```

模板 2——维护 size 的并查集：

```python
# 初始化，p存储每个点的父节点，size只有当节点是祖宗节点时才有意义，表示祖宗节点所在集合中，点的数量
p = list(range(n))
size = [1] * n


# 返回x的祖宗节点
def find(x):
    if p[x] != x:
        # 路径压缩
        p[x] = find(p[x])
    return p[x]


# 合并a和b所在的两个集合
if find(a) != find(b):
    size[find(b)] += size[find(a)]
    p[find(a)] = find(b)
```

模板 3——维护到祖宗节点距离的并查集：

```python
# 初始化，p存储每个点的父节点，d[x]存储x到p[x]的距离
p = list(range(n))
d = [0] * n


# 返回x的祖宗节点
def find(x):
    if p[x] != x:
        t = find(p[x])
        d[x] += d[p[x]]
        p[x] = t
    return p[x]


# 合并a和b所在的两个集合
p[find(a)] = find(b)
d[find(a)] = distance
```

<!-- tabs:start -->

### **Python3**

<!-- 这里可写当前语言的特殊实现逻辑 -->

染色法：

```python
class Solution:
    def isBipartite(self, graph: List[List[int]]) -> bool:
        def dfs(u, c):
            color[u] = c
            for v in graph[u]:
                if not color[v]:
                    if not dfs(v, 3 - c):
                        return False
                elif color[v] == c:
                    return False
            return True

        n = len(graph)
        color = [0] * n
        for i in range(n):
            if not color[i] and not dfs(i, 1):
                return False
        return True
```

并查集：

```python
class Solution:
    def isBipartite(self, graph: List[List[int]]) -> bool:
        def find(x):
            if p[x] != x:
                p[x] = find(p[x])
            return p[x]

        p = list(range(len(graph)))
        for u, g in enumerate(graph):
            for v in g:
                if find(u) == find(v):
                    return False
                p[find(v)] = find(g[0])
        return True
```

### **Java**

<!-- 这里可写当前语言的特殊实现逻辑 -->

染色法：

```java
class Solution {
    private int[] color;
    private int[][] g;

    public boolean isBipartite(int[][] graph) {
        int n = graph.length;
        color = new int[n];
        g = graph;
        for (int i = 0; i < n; ++i) {
            if (color[i] == 0 && !dfs(i, 1)) {
                return false;
            }
        }
        return true;
    }

    private boolean dfs(int u, int c) {
        color[u] = c;
        for (int v : g[u]) {
            if (color[v] == 0) {
                if (!dfs(v, 3 - c)) {
                    return false;
                }
            } else if (color[v] == c) {
                return false;
            }
        }
        return true;
    }
}
```

并查集：

```java
class Solution {
    private int[] p;

    public boolean isBipartite(int[][] graph) {
        int n = graph.length;
        p = new int[n];
        for (int i = 0; i < n; ++i) {
            p[i] = i;
        }
        for (int u = 0; u < n; ++u) {
            int[] g = graph[u];
            for (int v : g) {
                if (find(u) == find(v)) {
                    return false;
                }
                p[find(v)] = find(g[0]);
            }
        }
        return true;
    }

    private int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }
}
```

### **C++**

染色法：

```cpp
class Solution {
public:
    bool isBipartite(vector<vector<int>>& graph) {
        int n = graph.size();
        vector<int> color(n);
        for (int i = 0; i < n; ++i)
            if (!color[i] && !dfs(i, 1, color, graph))
                return false;
        return true;
    }

    bool dfs(int u, int c, vector<int>& color, vector<vector<int>>& g) {
        color[u] = c;
        for (int& v : g[u]) {
            if (!color[v]) {
                if (!dfs(v, 3 - c, color, g)) return false;
            } else if (color[v] == c)
                return false;
        }
        return true;
    }
};
```

并查集：

```cpp
class Solution {
public:
    vector<int> p;

    bool isBipartite(vector<vector<int>>& graph) {
        int n = graph.size();
        p.resize(n);
        for (int i = 0; i < n; ++i) p[i] = i;
        for (int u = 0; u < n; ++u) {
            auto& g = graph[u];
            for (int v : g) {
                if (find(u) == find(v)) return 0;
                p[find(v)] = find(g[0]);
            }
        }
        return 1;
    }

    int find(int x) {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }
};
```

### **Rust**

染色法：

```rust
impl Solution {
    #[allow(dead_code)]
    pub fn is_bipartite(graph: Vec<Vec<i32>>) -> bool {
        let mut graph = graph;
        let n = graph.len();
        let mut color_vec: Vec<usize> = vec![0; n];
        for i in 0..n {
            if color_vec[i] == 0 && !Self::traverse(i, 1, &mut color_vec, &mut graph) {
                return false;
            }
        }
        true
    }

    #[allow(dead_code)]
    fn traverse(
        v: usize,
        color: usize,
        color_vec: &mut Vec<usize>,
        graph: &mut Vec<Vec<i32>>
    ) -> bool {
        color_vec[v] = color;
        for n in graph[v].clone() {
            if color_vec[n as usize] == 0 {
                // This node hasn't been colored
                if !Self::traverse(n as usize, 3 - color, color_vec, graph) {
                    return false;
                }
            } else if color_vec[n as usize] == color {
                // The color is the same
                return false;
            }
        }
        true
    }
}
```

并查集：

```rust
impl Solution {
    #[allow(dead_code)]
    pub fn is_bipartite(graph: Vec<Vec<i32>>) -> bool {
        let n = graph.len();
        let mut disjoint_set: Vec<usize> = vec![0; n];
        // Initialize the disjoint set
        for i in 0..n {
            disjoint_set[i] = i;
        }

        // Traverse the graph
        for i in 0..n {
            if graph[i].is_empty() {
                continue;
            }
            let first = graph[i][0] as usize;
            for v in &graph[i] {
                let v = *v as usize;
                let i_p = Self::find(i, &mut disjoint_set);
                let v_p = Self::find(v, &mut disjoint_set);
                if i_p == v_p {
                    return false;
                }
                // Otherwise, union the node
                Self::union(first, v, &mut disjoint_set);
            }
        }

        true
    }

    #[allow(dead_code)]
    fn find(x: usize, d_set: &mut Vec<usize>) -> usize {
        if d_set[x] != x {
            d_set[x] = Self::find(d_set[x], d_set);
        }
        d_set[x]
    }

    #[allow(dead_code)]
    fn union(x: usize, y: usize, d_set: &mut Vec<usize>) {
        let p_x = Self::find(x, d_set);
        let p_y = Self::find(y, d_set);
        d_set[p_x] = p_y;
    }
}
```

### **Go**

染色法：

```go
func isBipartite(graph [][]int) bool {
	n := len(graph)
	color := make([]int, n)
	var dfs func(u, c int) bool
	dfs = func(u, c int) bool {
		color[u] = c
		for _, v := range graph[u] {
			if color[v] == 0 {
				if !dfs(v, 3-c) {
					return false
				}
			} else if color[v] == c {
				return false
			}
		}
		return true
	}
	for i := range graph {
		if color[i] == 0 && !dfs(i, 1) {
			return false
		}
	}
	return true
}
```

并查集：

```go
func isBipartite(graph [][]int) bool {
	n := len(graph)
	p := make([]int, n)
	for i := range p {
		p[i] = i
	}
	var find func(x int) int
	find = func(x int) int {
		if p[x] != x {
			p[x] = find(p[x])
		}
		return p[x]
	}
	for u, g := range graph {
		for _, v := range g {
			if find(u) == find(v) {
				return false
			}
			p[find(v)] = find(g[0])
		}
	}
	return true
}
```

### **TypeScript**

染色法：

```ts
function isBipartite(graph: number[][]): boolean {
    const n = graph.length;
    let valid = true;
    // 0 未遍历， 1 红色标记， 2 绿色标记
    let colors = new Array(n).fill(0);
    function dfs(idx: number, color: number, graph: number[][]) {
        colors[idx] = color;
        const nextColor = 3 - color;
        for (let j of graph[idx]) {
            if (!colors[j]) {
                dfs(j, nextColor, graph);
                if (!valid) return;
            } else if (colors[j] != nextColor) {
                valid = false;
                return;
            }
        }
    }

    for (let i = 0; i < n && valid; i++) {
        if (!colors[i]) {
            dfs(i, 1, graph);
        }
    }
    return valid;
}
```

并查集：

```ts
function isBipartite(graph: number[][]): boolean {
    const n = graph.length;
    let p = new Array(n);
    for (let i = 0; i < n; ++i) {
        p[i] = i;
    }
    function find(x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }
    for (let u = 0; u < n; ++u) {
        for (let v of graph[u]) {
            if (find(u) == find(v)) {
                return false;
            }
            p[find(v)] = find(graph[u][0]);
        }
    }
    return true;
}
```

### **...**

```

```

<!-- tabs:end -->
