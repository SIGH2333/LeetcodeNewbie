# Day1 Array

## Problem List 

知识点: #数组 #双指针 #左右开闭区间

Selected by 代码随想录

1. No.704 https://leetcode.com/problems/binary-search/704 
2. No.35 https://leetcode.com/problems/search-insert-position/
3. No.34 https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/
4. No.27 https://leetcode.com/problems/remove-element/

## No. 704 Binary Search

我的最终答案, 有问题! 

```java
class Solution {
    public int search(int[] nums, int target) { 
        // 多余
        if (nums.length == 1 && target == nums[0]) {
            return 0;
        }
        int left = 0;
        int right = nums.length - 1;
        while (left <= right) {
            // 有问题, 推荐 left + ((right - left)) >>1) 防止溢出
            int mid = (left + right) >> 1;  
            // 逻辑判断顺序也有问题, 应该先判断是否相等.
            if (nums[mid] < target) {
                left = mid;  // 有问题, left = mid + 1
            } else if (nums[mid] > target) {
                right = mid;  // 有问题, left = mid - 1
            } else {
                return mid;
            }
        }
        return -1;
    }
}
```

正确解法 (左闭右闭区间) :

```java
class Solution {
    public int search(int[] nums, int target) {
        // 避免当 target 小于nums[0] nums[nums.length - 1]时多次循环运算
        if (target < nums[0] || target > nums[nums.length - 1]) {
            return -1;
        }
        int left = 0, right = nums.length - 1;
        while (left <= right) {
            int mid = left + ((right - left) >> 1);
            if (nums[mid] == target)
                return mid;
            else if (nums[mid] < target)
                left = mid + 1;
            else if (nums[mid] > target)
                right = mid - 1;
        }
        return -1;
    }
}
```

为什么左闭右闭的时候, left = mid + 1, right = mid - 1呢, 因为此时的mid我们已经知道不等于target了!

## No. 27 移除元素

我的最终答案, 受到提示说使用双指针, 我想到了左右指针..

我的思路是, 左边遇到需要删除的val, 就把右边合适的值给过去, 如果右边也是要删除的, 就往前挪到不需要删除的,再替换掉左边要删除的.

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int left = 0;
        int right = nums.length - 1;
        while(left <= right) {
            if (nums[left] != val) {
                left++;
            } else if (nums[left] == val) {
                if (nums[right] != val) {
                    nums[left] = nums[right];
                    right--;
                } else if (nums[right] == val) {
                    right--;
                }
            }
        }
        return left;
    }
}
```

代码随想录提供的标准解法包含两种指针方法 

快慢指针的思路需要我学习和理解

双指针法（快慢指针法）： **通过一个快指针和慢指针在一个for循环下完成两个for循环的工作。**

定义快慢指针

- 快指针：寻找新数组的元素 ，新数组就是不含有目标元素的数组
- 慢指针：指向更新 新数组下标的位置

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        // 快慢指针
        int slowIndex = 0;
        for (int fastIndex = 0; fastIndex < nums.length; fastIndex++) {
            if (nums[fastIndex] != val) {
                nums[slowIndex] = nums[fastIndex];
                slowIndex++;
            }
        }
        return slowIndex;
    }
}
```

```java
//相向双指针法
class Solution {
    public int removeElement(int[] nums, int val) {
        int left = 0;
        int right = nums.length - 1;
        while(right >= 0 && nums[right] == val) right--; //将right移到从右数第一个值不为val的位置
        while(left <= right) {
            if(nums[left] == val) { //left位置的元素需要移除
                //将right位置的元素移到left（覆盖），right位置移除
                nums[left] = nums[right];
                right--;
            }
            left++;
            while(right >= 0 && nums[right] == val) right--;
        }
        return left;
    }
}
```

