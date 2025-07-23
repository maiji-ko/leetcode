# [1695. Maximum Erasure Value](https://leetcode.com/problems/maximum-erasure-value/description/)

## 一、题目描述
>You are given an array of positive integers `nums` and want to erase a subarray containing **unique elements**. The **score** you get by erasing the subarray is equal to the **sum** of its elements.
>
>Return *the **maximum score** you can get by erasing **exactly one** subarray.*
>
>An array `b` is called to be a subarray of `a` if it forms a contiguous subsequence of `a`, that is, if it is equal to `a[l],a[l+1],...,a[r]` for some `(l,r)`.

## 二、用到的容器

- **解法一(maijiko):**

    ``` c++
    std::unordered_map
    ```

## 三、提交结果

- **解法一:**

    ![解法一](pics/solution_1_pass.png)

## 四、解题思路

- **解法一:**

    采用滑动窗口，窗口内维持 **不包含重复字符** 的字串
    
    step 1. 定义 `numCount` 用来存储当前窗口中所有字符出现的频率
    
    step 2. 若当前字符频率大于 `1` 则将窗口左端收缩，直到满足当前窗口维持条件

## 五、代码实现

- **解法一:**

    ``` c++
    class Solution {
    public:
        int maximumUniqueSubarray(vector<int>& nums) {
            int left = 0;
            int currentSum = 0;
            int maxSum = 0;
            std::unordered_map<int, int> numCount;
            for (int right = 0; right < nums.size(); right++) {
                numCount[nums[right]]++;
                if (numCount.count(nums[right])) {
                    while (numCount[nums[right]] == 2) {
                        currentSum -= nums[left];
                        numCount[nums[left]]--;
                        left++;
                    }
                }
                currentSum += nums[right];
                maxSum = std::max(maxSum, currentSum);
            }
    
            return maxSum;
        }
    };
    ```