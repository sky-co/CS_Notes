<!-- GFM-TOC -->
* [1. 把数组中的 0 移到末尾](#1-把数组中的-0-移到末尾)
* [2. 改变矩阵维度](#2-改变矩阵维度)
* [3. 找出数组中最长的连续 1](#3-找出数组中最长的连续-1)
* [4. 有序矩阵查找](#4-有序矩阵查找)
* [5. 有序矩阵的 Kth Element](#5-有序矩阵的-kth-element)
<!--* [6. 一个数组元素在 [1, n] 之间，其中一个数被替换为另一个数，找出重复的数和丢失的数](#6-一个数组元素在-[1,-n]-之间，其中一个数被替换为另一个数，找出重复的数和丢失的数)
* [7. 找出数组中重复的数，数组值在 [1, n] 之间](#7-找出数组中重复的数，数组值在-[1,-n]-之间)
* [8. 数组相邻差值的个数](#8-数组相邻差值的个数)
* [9. 数组的度](#9-数组的度)
* [10. 对角元素相等的矩阵](#10-对角元素相等的矩阵)
* [11. 嵌套数组](#11-嵌套数组)
* [12. 分隔数组](#12-分隔数组) -->
<!-- GFM-TOC -->

# 1. 把数组中的 0 移到末尾

283\. Move Zeroes (Easy)

[LeetCode](https://leetcode.com/problems/move-zeroes/description/)

```html
For example, given nums = [0, 1, 0, 3, 12], after calling your function, nums should be [1, 3, 12, 0, 0].
```

## 解法1

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

## 解法2

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

## 解法3

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

# 2. 改变矩阵维度

566\. Reshape the Matrix (Easy)

[Leetcode](https://leetcode.com/problems/reshape-the-matrix/description/) 

```html
Input:
nums =
[[1,2],
 [3,4]]
r = 1, c = 4

Output:
[[1,2,3,4]]

Explanation:
The row-traversing of nums is [1,2,3,4]. The new reshaped matrix is a 1 * 4 matrix, fill it row by row by using the previous list.
```

## 解法1

```c++
vector<vector<int>> matrixReshape(vector<vector<int>>& nums, int r, int c) {
  int m = nums.size(), n = nums[0].size();
  if (m * n != r * c) {
      return nums;
  }

  // Use extra space to convert orginal vector into 1-dimension
  vector<int> vecTmp;
  for (int i = 0; i < m; i++) {
      for (int j = 0; j < n; j++) {
          vecTmp.push_back(nums[i][j]);
      }
  }
  
  // In the end, index == r * c
  int index = 0;
  vector<vector<int>> res(r, vector<int>(c, 0));
  for (int i = 0; i < r; i++) {
      for (int j = 0; j < c; j++) {
          res[i][j] = vecTmp[index++];
      }
  }

  return res;
}
```

**分析：**

1. 遍历数组nums，存入临时一维数组vecTmp
2. 挨个取出临时一维数组vecTmp的元素填入数组v

## 解法2

```c++
vector<vector<int>> matrixReshape(vector<vector<int>>& nums, int r, int c) {
  int m = nums.size(), n = nums[0].size();
  if (m * n != r * c) {
      return nums;
  }

  int rowIdx{0};
  int colIdx{0};
  vector<vector <int> > v(r ,vector<int>(c, 0));
  for (int i = 0; i < r; i++) {
      for (int j = 0; j < c;) {
          if (colIdx != n) {
              v[i][j++] = nums[rowIdx][colIdx++];
          }
          else {
              rowIdx++;
              colIdx = 0;
          }
      }
  }
  return v;
}
```

**分析：**

1. 如果数组的元素个数小于r和c的乘积，说明r和c不符合，返回原数组
2. 预先分配一个r*c大小的数组v，填充0
3. 遍历数组nums，挨个取出元素填入数组v（通过移动column的索引，按行遍历nums）

## 解法3

```c++
vector<vector<int>> matrixReshape(vector<vector<int>>& nums, int r, int c) {
  int m = nums.size(), n = nums[0].size();
  if (m * n != r * c) {
      return nums;
  }

  int idx{0};
  vector<vector <int> > v(r ,vector<int>(c, 0));
  for (int i = 0; i < r; i++) {
      for (int j = 0; j < c; j++) {
          v[i][j] = nums[idx / n][idx % n];
          idx++;
      }
  }
  return v;
}
```

**分析：**

1. 遍历数组v，挨个取出数组nums的元素填入数组v（通过idx将数组nums转化为一维数组，行=idx/n, 列=idx%n）

## 解法4

```c++
vector<vector<int>> matrixReshape(vector<vector<int>>& nums, int r, int c) {
  int m = nums.size(), n = nums[0].size();
  if (m * n != r * c) {
      return nums;
  }

  vector<vector<int>> res(r, vector<int>(c, 0));
  for (int i = 0; i < m; i++) {
      for (int j = 0; j < n; j++) {
          int k = i * n + j;
          res[k / c][k % c] = nums[i][j];
      }
  }

  return res;
}
```

**分析：**

1. 遍历数组nums，挨个取出元素填入数组v（通过k将数组res转化为一维数组，行=k/c, 列=k%c）

# 3. 找出数组中最长的连续 1

485\. Max Consecutive Ones (Easy)

[Leetcode](https://leetcode.com/problems/max-consecutive-ones/description/)

## 解法 1

```c++
int findMaxConsecutiveOnes(vector<int>& nums) {
  int cnt{0};
  int max{0};
  for (auto num:nums) {
      if (num != 0) {
          cnt++;
      }
      else {
          cnt = 0;
      }
      max = std::max(cnt, max);
  }
  return max;
}
```

**分析：**

1. 遍历数组nums，挨个取出元素判定（0-cnt重置，1-cnt递增）
2. 变量max保存连续最大的1的个数，与cnt比较取最大值

## 解法 2

```c++
int findMaxConsecutiveOnes(vector<int>& nums) {
  int cnt{0};
  int max{0};
  for (auto num:nums) {
      cnt = num == 0 ? 0 : cnt+1;
      max = std::max(cnt, max);
  }
  return max;
}
```

**分析：**

1. 解法1简化版

## 解法 3

```c++
int findMaxConsecutiveOnes(vector<int>& nums) {
  int cnt{0};
  int max{0};
  for (auto num:nums) {
      cnt = num == 0 ? 0 : cnt+1;
      max = std::max(cnt, max);
  }
  return max;
}
```

**分析：**

1. 解法1简化版

# 4. 有序矩阵查找

240\. Search a 2D Matrix II (Medium)

[LeetCode](https://leetcode.com/problems/search-a-2d-matrix-ii/description/)

```html
[
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
]
```

## 解法 1

```c++
bool searchMatrix(vector<vector<int>>& matrix, int target) {
  if (matrix.empty() || matrix[0].empty())
      return false;
  
  int m = matrix.size();
  int n = matrix[0].size();
  for (int colIdx = n - 1; colIdx >=0; colIdx--) {
      for (int rowIdx = 0; rowIdx < m; rowIdx++) {
          if (matrix[rowIdx][colIdx] > target) {
              break;
          }
          else if (matrix[rowIdx][colIdx] < target) {
              continue;
          }
          else {
              return true;
          }
      }
  }
  return false;
}
```

**分析：**

1. 矩阵有效性检查
2. 由于矩阵从左到右和从上到下递增的特性，遍历数组的起始位置从数组右上角开始，从右往左，从上到下的搜索target

## 解法 2

```c++
bool searchMatrix(vector<vector<int>>& matrix, int target) {
  if (matrix.empty() || matrix[0].empty())
      return false;
  
  int m = matrix.size();
  int n = matrix[0].size();
  int rowIdx{0};
  int colIdx{n-1};
  while (rowIdx < m && colIdx >= 0) {
      if (matrix[rowIdx][colIdx] == target) return true;
      else if (matrix[rowIdx][colIdx] > target) colIdx--;
      else rowIdx++;
  }
  return false;
}
```

**分析：**

1. 解法1简化版本
2. 剪枝：当前位置matrix[rowIdx][colIdx] > target时，不必从rowIdx=0开始往下搜索，直接从matrix[rowIdx][colIdx--]开始找

# 5. 有序矩阵的 Kth Element

378\. Kth Smallest Element in a Sorted Matrix (Medium)

[LeetCode](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/description/)

```html
matrix = [
  [ 1,  5,  9],
  [10, 11, 13],
  [12, 13, 15]
],
k = 8,

return 13.
```

## 解法 1

```c++
int kthSmallest(vector<vector<int>>& matrix, int k) {
  vector<int> v;
  for (auto arr:matrix) {
      for (auto num:arr) {
          v.emplace_back(num);
      }
  }
  sort(v.begin(), v.end());
  return v[k - 1];
}
```

**分析：**

1. 对于一个有序的递增数组，只需要从头开始找到第K小的数字就可以了（不考虑额外空间消耗的情况下）

## 解法 2

```c++
int kthSmallest(vector<vector<int>>& matrix, int k) {
  int n = matrix.size(); 
  int lo = matrix[0][0];
  int hi = matrix[n-1][n-1];
  int mid = 0;

  int num = 0;
  while (lo < hi) {
      mid = lo + (hi - lo) / 2;
      int cnt = 0;
      num++;
      cout << "num: " << num;
      for (int i = 0; i < n; i++) {
          int pos = upper_bound(matrix[i].begin(), matrix[i].end(), mid) - matrix[i].begin();
          cnt += pos;
          cout << ", pos: " << pos;
      }
      cout << endl;
      if (cnt < k) {
          lo = mid + 1;
      }
      else {
          hi = mid - 1;
      }
  }
  cout << num << endl;
  return lo;
}
```

**分析：**

1. 二分法（二分范围查找），由于左上和右下的两个数字就是矩阵的范围，取出中间数mid之后，逐行遍历元素，二分法查找满足k的数字
2. lo的值即结果

## 解法 3

```c++
int kthSmallest(vector<vector<int>>& matrix, int k) {
  priority_queue<int> heap;
  for (int i = 0; i < matrix.size(); i++)
  {
      for (int j = 0; j < matrix[0].size(); j++)
      {
          if (heap.size() >= k)
          {
              if (matrix[i][j] < heap.top())
              {
                  heap.push(matrix[i][j]);
                  heap.pop();
              }
          }
          else
          {
              heap.push(matrix[i][j]);
          }
      }
  }
  return heap.top();
}
```

**分析：**

1. 优先队列priority_queue，当队列元素超过k个，如果当前元素比队列首元素小，插入当前元素后pop掉最大的元素

## 解法 4
堆解法（待补充）

**解题参考：**
[LeetCode](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/discuss/85173) / [力扣](https://leetcode-cn.com/problems/kth-smallest-element-in-a-sorted-matrix/discuss/85173)
