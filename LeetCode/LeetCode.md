<!-- GFM-TOC -->
* [1. 把数组中的 0 移到末尾](#1-把数组中的-0-移到末尾)
<!-- * [2. 改变矩阵维度](#2-改变矩阵维度)
* [3. 找出数组中最长的连续 1](#3-找出数组中最长的连续-1)
* [4. 有序矩阵查找](#4-有序矩阵查找)
* [5. 有序矩阵的 Kth Element](#5-有序矩阵的-kth-element)
* [6. 一个数组元素在 [1, n] 之间，其中一个数被替换为另一个数，找出重复的数和丢失的数](#6-一个数组元素在-[1,-n]-之间，其中一个数被替换为另一个数，找出重复的数和丢失的数)
* [7. 找出数组中重复的数，数组值在 [1, n] 之间](#7-找出数组中重复的数，数组值在-[1,-n]-之间)
* [8. 数组相邻差值的个数](#8-数组相邻差值的个数)
* [9. 数组的度](#9-数组的度)
* [10. 对角元素相等的矩阵](#10-对角元素相等的矩阵)
* [11. 嵌套数组](#11-嵌套数组)
* [12. 分隔数组](#12-分隔数组) -->
<!-- GFM-TOC -->

# 1. 把数组中的 0 移到末尾

283\. Move Zeroes (Easy)

[Leetcode](https://leetcode.com/problems/move-zeroes/description/)

```html
For example, given nums = [0, 1, 0, 3, 12], after calling your function, nums should be [1, 3, 12, 0, 0].
```

## 解法1：
```c++
void moveZeroes(vector<int>& nums) {
    int count{0};
    int p{0};
    for (int idx = 0; idx < nums.size(); idx++) {
        if (nums[idx] == 0) {
            count++;
        }
        else {
            nums[p] = nums[idx];
            p++;
        }
    }
    for (int idx=p; idx < nums.size(); idx++)
        nums[idx] = 0;
}
```
**分析：**
1. 一个变量count统计非0的个数，另一个变量p记录移动后非0数字的实际索引位置
2. 一趟遍历之后，非0数字移到左端，往p的位置填0

## 解法2：
```c++
void moveZeroes(vector<int>& nums) {
    int idx{0};
    for (auto num:nums) {
        if (num != 0) {
            nums[idx++] = num;
        }
    }
    while(idx < nums.size()) {
        nums[idx++] = 0;
    }
}
```
**分析：**
1. 使用一个变量idx来代替解法1中的计数和索引的作用，最终idx的值表示第一个0的位置

## 解法3：
```c++
void moveZeroes(vector<int>& nums) {
    int len = 0;
    for (int i = 0; i < nums.size(); ++i) {
        if (nums[i] != 0) {
            if (nums[len] != nums[i]) {
                swap(nums[len], nums[i]);
            }
            ++len;
        }
    }
}
```
**分析：**
1. 使用一个变量len表示非0数字的长度
2. 遍历数组，当遇上非自己元素的非0数字时候，交换len和游标i所对应的数字。一次遍历完成移动和填充0操作