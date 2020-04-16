---
layout: post
title: "Single Number"
comments: true
category: algorithm
description: >
  leetcode Single Number 30일 Challenge 문제 (java)
---

#  Single Numb

### 문제 내용

Given a non-empty array of integers, every element appears twice except for one. Find that single one.

=> 한 수를 제외한 모든 수가 2번씩 배열에 들어있다.


- Example
~~~
Input: [2,2,1]
Output: 1
~~~
-Example 2
~~~
Input: [4,1,2,1,2]
Output: 4
~~~


### 내가 찬 코드
~~~java
class Solution {
    public int singleNumber(int[] nums) {
            List<Integer> list = new ArrayList<>();

    for (int i = 0; i < nums.length; i++) {
      if (!list.contains(nums[i])) {
        list.add(nums[i]);
      } else {
        list.remove(new Integer(nums[i]));
      }
    }
        return list.get(0);
    }
}
~~~

공간을 list나 map으로 할당 안하고 푸는 방법이 있다.

바로 XOR을 이용하는 방법이다.

### 공간복잡도가 개선된 코드
~~~java
class Solution {
    public int singleNumber(int[] nums) {
    int x = 0;
    for (int i : nums) {
      x ^= i;
    }
        return x;
    }
}
~~~
