# [1957. Delete Characters to Make Fancy String](https://leetcode.com/problems/delete-characters-to-make-fancy-string/description/)

## 一、题目描述
>A **fancy string** is a string where no **three** **consecutive** characters are equal.
>
>Given a string `s`, delete the **minimum** possible number of characters from `s` to make it **fancy**.
>
>Return *the final string after the deletion*. It can be shown that the answer will always be **unique**.

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

    定义两个遍历，分辨标记需要 **写入(write)** 和 **读取(read)** 的位置，`read` 遍历 字符串 `s`，当前读取的位置与当前写入位置的前 **两个** 位置字符不相同时，写入 `read` 位置的字符，否则，用 `read` 继续遍历，最后生成的新的 `s` 字符串前 `write -1` 个位置就为需要返回的字符串

## 五、代码实现

- **解法一:**

    ``` c++
    class Solution {
    public:
        string makeFancyString(string s) {
            int write = 0;
            for (int read = 0; read < s.length(); read++) {
                if (read < 2 || s[read] != s[write - 1] || s[read] != s[write - 2]) {
                    s[write++] = s[read];
                }
            }
            s.resize(write);
            return s;
        }
    };
    ```