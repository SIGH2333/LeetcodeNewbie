# Day 2 Array

## Problem List

知识点: #数组 #双指针 #二分搜索 #滑动窗口

Selected by 代码随想录

1. No. 977 https://leetcode.com/problems/squares-of-a-sorted-array/
2. No. 209 https://leetcode.com/problems/minimum-size-subarray-sum/
3. No. 59 https://leetcode.com/problems/spiral-matrix-ii/

## No. 977 Squares of a Sorted Array

第一个思路是: 先平方 再排序

发现问题:

1. 排序算法记得不牢, 本来想用 $O(n)=nlogn$ 的算法, 结果用了插入排序, 可以用 快排
2. 没用到双指针

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        // First calculate the squares
        for (int i = 0; i < nums.length; i++) {
           nums[i] = nums[i] * nums[i]; 
        }
        // Sort
        for (int i = 0; i < nums.length - 1; i++) {
            
            for (int j = i + 1; j > 0; j--) {
                if (nums[j] < nums[j-1]) {
                    int temp = nums[j-1];
                    nums[j-1] = nums[j];
                    nums[j] = temp;
                } else {
                    break;
                }
            }
        }
        return nums;
    }
}
```

代码随想录的解法: 利用双指针思想

> 考虑双指针法: i指向起始位置, j指向终止位置
>
> 定义一个新数组result, 和A数组一样的大小, 让k指向result数组终止位置
>
> 如果 `A[i] * A[i] < A[j] * A[j]` 那么 `result[k--] = A[j] * A[j]`
>
> 如果 `A[i] * A[i] > A[j] * A[j]` 那么 `result[k--] = A[i] * A[i]`

意思总结过来就是, 最大的在两边, 那么我们就由大到小的为新数组输入元素, 对应过来就是i和j在原数组的两边找最大的

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int i = 0;
        int j = nums.length - 1;
        int k = j;
        int[] result = new int[nums.length];
        while(k >= 0) {
            if(nums[i] * nums[i] > nums[j] * nums[j]) {
                result[k--] = nums[i] * nums[i];
                i++;
            } else {
                result[k--] = nums[j] * nums[j];
                j--;
            }
        }
        return result;
    }
}
```

## No. 209 Minimum Size Subarray Sum

思路是先排序, 后从后往前相加比大小, 但思路错了, 因为要求Subarray, 意味着只能从连续的元素相加.

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        Arrays.sort(nums);
        int sum = 0;
        int count = 0;
        for (int i = nums.length - 1; i >= 0; i--) {
            sum += nums[i];
            count++;
            if (sum >= target) {
                return count;
            }
        }
        return 0;
    }
}
```

正确的解法1: 

暴力, 两层循环, 每次从一个下标开始连续求和比大小, 找到最小的满足target的序列长度

#### 正确的解法2 : 滑动窗口

啥是滑动窗口?

> 所谓滑动窗口, 就是 不断地调节子序列的起始位置和终止位置 (类似双指针) , 从而得出我们想要的结果

在暴力解法中，是一个for循环滑动窗口的起始位置，一个for循环为滑动窗口的终止位置，用两个for循环 完成了一个不断搜索区间的过程。 **那么滑动窗口如何用 一个for循环 来完成呢?**

如果用一个for循环来表示 滑动窗口的起始位置, 会落入暴力解的困境, 所以需要用 滑动窗口的终止位置 来作为循环的索引

**对于本题:**

- 窗口就是 满足其和 ≥ s 的长度最小的 连续 子数组。
- 窗口的起始位置如何移动：如果当前窗口的值大于s了，窗口就要向前移动了（也就是该缩小了）
- 窗口的结束位置如何移动：窗口的结束位置就是遍历数组的指针，也就是for循环里的索引

#### 滑动窗口的例子

打个比方 [1, 2, 3, 4, 5, 6], target = 6

1. left 停留在0, sum = 0, result = MAX

2. 外层循坏right开始走 1 -> 2 -> 3 -> 4 -> 5 -> 6 .....

   3. sum += nums[right]

   4. 当sum >= target, 找到了第一个符合条件的子数组 (如果遍历完没找到自然就是没有)
   5. 我们要找最短的满足条件的子数组, 所以我们要试着削减长度
      1. 首先记录当前长度, **在while内要比较, 而不是单纯记录, 而是要记录见过的最短的长度**
      2. 从left往后挪, 如果符合条件就继续, 如果sum小于target就跳出循环, right再往后挪

3. 最后判断, 如果result还是初始值就代表没有找到符合条件的子数组, 不是就代表是结果.

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int left = 0;
        int sum = 0;
        int result = Integer.MAX_VALUE;
        for (int right = 0; right < nums.length; right++) {
            sum += nums[right];
            while (sum >= target) {
                // result = right - left + 1;
                result = Math.min(result, right - left + 1);
                sum -= nums[left++];
            }
        }
        return result == Integer.MAX_VALUE ? 0 : result;

    }
}
```

## No. 59 Spiral Matrix II

嗯...看着头大, 下次再写
