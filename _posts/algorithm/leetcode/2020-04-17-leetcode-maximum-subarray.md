---
layout: post
title: "Maximum subarray"
comments: true
category: algorithm
description: >
  leetcode Maximum subarray 30일 Challenge 문제 (java)
---

#  Single Numb

### 문제 내용

Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

=> 이어지는 배열로 부분 합을 최대로 만들 때 그 수


- Example
~~~
Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
~~~


### 내가 찬 코드 (다시 풀어보자)
~~~java

~~~

카데인 알고리즘이라는 끝에 값을 기준으로 계산하는 방법이 있다.

### 시간복잡도가 개선된 코드
~~~java
class Solution {
    public int maxSubArray(int[] nums) {
        int endNum = 0;
        int bestNum = Integer.MIN_VALUE;
        
        for(int num : nums){
            endNum = Math.max(endNum+num,num);
            bestNum = Math.max(bestNum, endNum);
        }
        
        return bestNum;
    }
}
~~~
