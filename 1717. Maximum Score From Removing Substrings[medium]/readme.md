# [1717. Maximum Score From Removing Substrings](https://leetcode.com/problems/maximum-score-from-removing-substrings/description/)

## 一、题目描述
>You are given a string `s` and two integers `x` and `y`. You can perform two types of operations any number of times.
>
>- Remove substring `"ab"` and gain `a` points.
>  - For example, when removing `"ab"` from `"cabxbae"` it becomes `"cxbae"`.
>- Remove substring `"ba"` and gain `y` points.
>  - For example, when removing `"ba"` from `"cabxbae"` it becomes `"cabxe"`.
>
>Return *the maximum points you can gain after applying the above operations on* `s`.

## 二、用到的容器

- **解法一(maijiko):**

    ``` c++
    None
    ```

## 三、提交结果

- **解法一:**

    ![解法一](pics/solution_1_pass.png)

## 四、解题思路

- **解法一:**

    优先处理收益最大的组合
    
    step 1. 比较 `x`  和 `y`, 优先处理收益最大的组合
    
    step 2. 遍历字符串
    
    ​	当遇到优先字符 `a` 时，增加 `aCnt`
    
    ​	当遇到次优先字符 `b` 时
    
    ​		如果有 `a` 可用，则计分 `x`, 并减少 `aCnt`
    
    ​		否则，增加 `bCnt` 以备后续使用
    
    step 3. 返回最大得分 

## 五、代码实现

- **解法一:**

    ``` c++
    class Solution {
    public:
        int maximumGain(string s, int x, int y) {
            char a = 'a';
            char b = 'b';
            if (x < y) {
                std::swap(a, b);
                std::swap(x, y);
            }
    
            int aCnt = 0;
            int bCnt = 0;
            int maxGain = 0;
            for (auto& c : s) {
                if (c == a) {
                    aCnt++;
                } else if (c == b) {
                    if (aCnt > 0) {
                        maxGain += x;
                        aCnt--;
                    } else {
                        bCnt++;
                    }
                } else {
                    maxGain += std::min(aCnt, bCnt) * y;
                    aCnt = 0;
                    bCnt = 0;
                }
            }
            maxGain += std::min(aCnt, bCnt) * y;
    
            return maxGain;
        }
    };
    ```