缓存数据都存储在HashTable里

输入任意数据 - 散列表 - 数字（int） （DNS -> IP）

|并   &交   -差

## 分治

将一个规模为N的问题分解为K个规模较小的子问题，这些子问题==相互独立且与原问题性质相同==。求出子问题的解，就可得到原问题的解。

```html
<!--给表达式加括号-->
Input: "2-1-1".

((2-1)-1) = 0
(2-(1-1)) = 2

Output : [0, 2]
```

```Java
public List<Integer> diffWaysToCompute(String input) {
    List<Integer> ways = new ArrayList<>();
    for (int i = 0; i < input.length(); i++) {
        char c = input.charAt(i);
        if (c == '+' || c == '-' || c == '*') {
            List<Integer> left = diffWaysToCompute(input.substring(0, i));
            List<Integer> right = diffWaysToCompute(input.substring(i + 1));
            for (int l : left) {
                for (int r : right) {
                    switch (c) {
                        case '+':
                            ways.add(l + r);
                            break;
                        case '-':
                            ways.add(l - r);
                            break;
                        case '*':
                            ways.add(l * r);
                            break;
                    }
                }
            }
        }
    }
    if (ways.size() == 0) {
        ways.add(Integer.valueOf(input));
    }
    return ways;
}

```



## DP

https://www.pdai.tech/md/algorithm/alg-core-dynamic.html

多解异值 -> 最优解

解决子问题->大问题 (动态规划保存了子问题的解)



## NP

每步都选择局部最优解，近似贪婪

## KNN

特征匹配



## Dijstra

最便宜的加权路线

有向无环图

## DFS



## BFS

