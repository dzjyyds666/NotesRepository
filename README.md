# 算法笔记

摘自:[leetcode最强刷题指南](https://blog.csdn.net/sinat_16643223/article/details/114152438)

## 一、了解算法复杂度

- 时间复杂度:[算法时间复杂度详解](https://mp.weixin.qq.com/s?__biz=MzUxNjY5NTYxNA==&mid=2247485980&idx=1&sn=88a44d270b1361611996281446c4f7b6&scene=21#wechat_redirect)

- 空间复杂度:表示一个算法在运行过程中临时占用存储空间大小的量度

## 二、基本数据结构

### 1、数组

数组是存放在连续空间上的相同数据类型数据的集合

> 要点:
>
> - 数组的下标都是从0开始
> - 数组在内存中是一片连续的地址空间
> - 二维数组是一个线性数组,数组中存放的是每一行数据的首地址

#### 算法详解:二分法

> 例题:35.[搜索插入位置](https://leetcode.cn/problems/search-insert-position/)
>
> 给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。
>
> 请必须使用时间复杂度为 `O(log n)` 的算法。
>
> **示例 1:**
>
> ```
> 输入: nums = [1,3,5,6], target = 5
> 输出: 2
> ```
>
> **示例 2:**
>
> ```
> 输入: nums = [1,3,5,6], target = 2
> 输出: 1
> ```
>
> **示例 3:**
>
> ```
> 输入: nums = [1,3,5,6], target = 7
> 输出: 4
> ```

思路:可以使用两种办法,暴力搜索和二分查找法,使用二分查找法需要注意选择的区间问题,如果选择的是**左闭右闭**,则**while(left <= right)**,如果选择的是**左闭右开**,则**while(left < right)**.

> - 暴力破解

```java
class Solution {
  	public int searchInsert(int nums[],int target){
      int length = nums.length;
     	for(int i = 0;i < length;i++){
        // 从0开始遍历数组,数组元素小于目标元素,直接跳过,数组元素大于或等于目标元素,则直接返回数组元素的下标
        if(target <= nums[i]){
          	return i;
        }
      }
      // 目标元素大于数组中的所有元素,则要插入数组后面
      return length + 1;
    }
}
```

> - 二分查找第一种(左闭右闭)

```java
class Solution {
  public int searchInsert(int nums[],int target){
    int length = nums.length;
    int left = 0;
    int right = length - 1; // 左闭右闭
    while(left <= right){
      int middle = left + (right - left) / 2;
      if(target > nums[middle]){
        // 目标元素在右区间
        left = middle + 1;
      }else if(target < nums[middle]){
        // 目标在左区间
        right = middle - 1;
      }else if(target == nums[middle]){
        return middle;
      }
    }
    // 分别处理如下四种情况
    // 目标值在数组所有元素之前  [0, -1]
    // 目标值等于数组中某一个元素  return middle;
    // 目标值插入数组中的位置 [left, right]，return  right + 1
    // 目标值在数组所有元素之后的情况 [left, right]， return right + 1
    return right + 1;
  }
}
```

> - 二分查找第二种(左闭右开)

```java
class Solution {
  public int searchInsert(int nums[],int target){
    int length = nums.length;
    int left = 0;
    int right = length; // 左闭右开
    while(left < right){
      int middle = left + (right - left) / 2;
      if(target > nums[middle]){
        // 目标元素在右区间
        left = middle + 1;
      }else if(target < nums[middle]){
        // 目标在左区间
        right = middle ;
      }else if(target == nums[middle]){
        return middle ;
      }
    }
    // 分别处理如下四种情况
    // 目标值在数组所有元素之前 [0,0)
    // 目标值等于数组中某一个元素 return middle
    // 目标值插入数组中的位置 [left, right) ，return right 即可
    // 目标值在数组所有元素之后的情况 [left, right)，return right 即可
    return right ;
  }
}
```

