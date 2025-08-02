# \[题号.题目标题\]\(题目链接\)

## 一、题目描述
>Given a string `s`, find the length of the **longest** **substring** without duplicate characters.

## 二、用到的容器

- **解法一(maijiko):**

    ``` c++
    std::array
    ```

## 三、提交结果

- **解法一:**

    ![解法一](pics/solution_1_pass.png)

## 四、解题思路

- **解法一:**

    采用 **滑动窗口** 的思想，通过判断窗口内是否有重复的字符串，来扩大与缩小窗口的范围
    
    step 1. 定义数组 `charCount` 数组来统计 `left` 与 `right` 范围内字符出现的次数
    
    step 2. 判断当前窗口右边界的字符是否出现过( `charCount[s[right]]` > 1)
    
    ​	如果存在，则搜小左边界，直到对于字符的次数不大于1
    
    ​	如果不存在，则右边界继续遍历并更新保存最大长度变量 `maxLength`
    
    step 3. 遍历结束后，`maxLength` 中保存的就是最大无重复字串长度

## 五、代码实现

- **解法一:**

    ``` c++
    class Solution {
    public:
        int lengthOfLongestSubstring(string s) {
            static constexpr int arraySize = 128;
            std::array<int, arraySize> charCount{ 0 };
    
            int left = 0;
            int maxLength = 0;
            for (int right = 0; right < s.length(); right++) {
                auto currentChar = s[right];
                charCount[currentChar]++;
                if (charCount[currentChar] > 1) {
                    while (charCount[currentChar] > 1) {
                        charCount[s[left]]--;
                        left++;
                    }
                }
                maxLength = std::max(maxLength, right - left + 1);
            }
    
            return maxLength;
        }
    };
    ```
