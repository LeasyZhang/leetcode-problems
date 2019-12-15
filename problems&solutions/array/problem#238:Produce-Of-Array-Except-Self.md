#### 问题地址
[LeetCode#238](https://leetcode.com/problems/product-of-array-except-self/)
#### 问题描述

给一个数组A[i],返回数组B[i],使得B[i] = A[0] * A[1] * .... * A[i-1] * A[i+1] * ... *A[n].

> Given an array nums of n integers where n > 1,  return an array output such that output[i] is equal to the product of all the elements of nums except nums[i].

> Please solve it without division and in O(n).

#### 范例

```JavaScript
Input:  [1,2,3,4]
Output: [24,12,8,6]
```

#### 提醒
- 不允许用除法解决问题
- 时间复杂度要求O(n)

#### 解题思路
这道题有两个思路
##### 构造辅助数组
定义两个数组left和right. left[i] = A[0] * A[1] * ... * A[i-1]; right[i] = A[i+1] * A[i+2] * ... * A[n-1].
这样子B[i] = left[i] * right[i].根据这个定义left[0] = 1, right[n-1] = 1.

##### 代码
```Java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        if(nums == null || nums.length == 0) {
            return nums;
        }
        if(nums.length == 1) {
            return new int[]{1};
        }
        
        int[] L = new int[nums.length];
        int[] R = new int[nums.length];
        
        L[0] = 1;
        L[1] = nums[0];
        R[nums.length - 1] = 1;
        R[nums.length - 2] = nums[nums.length - 1];
        
        for(int i = 2; i < nums.length; i++) {
            L[i] = nums[i-1] * L[i-1];
        }
        
        for(int i = nums.length - 3; i >= 0; i--) {
            R[i] = nums[i+1] * R[i+1];
        }
        int[] ans = new int[nums.length];
        for(int i = 0; i < nums.length; i++) {
            ans[i] = L[i] * R[i];
        }
        
        return ans;
    }
}
```
##### 迭代
在第一个解法里面，可以发现对于left[i],存在left[i] = left[i-1] * A[i-1], right[i] = right[i+1] * A[i+1].
可以不需要辅助数组，对B[i]进行迭代累积.先计算左边的乘积,再计算右边的乘积.
##### 代码
```Java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        if(nums == null || nums.length == 0) {
            return nums;
        }
        int[] ans = new int[nums.length];
        
        ans[0] = 1;
        for(int i = 1; i < nums.length; i++) {
            ans[i] = ans[i-1] * nums[i-1];
        }
        
        int temp = 1;
        for(int i = nums.length - 2; i >= 0; i--) {
            temp *= nums[i+1];
            ans[i] *= temp;
        }
        
        return ans;
    }
}
```