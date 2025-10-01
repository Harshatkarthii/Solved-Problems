# Solved-Problems

## LeetCode Problem 

## 76 - Minimum Window Substring

**Problem Statement:**

Given two strings `s` and `t` of lengths `m` and `n` respectively, return the minimum window of `s` such that **every character in `t` (including duplicates) is included in the window**.  

If there is no such substring, return the empty string `""`.  

The testcases will be generated such that the answer is unique.

**Example:**

```text
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
```
**Code:**
```text
class Solution {
    public String minWindow(String s, String t) {
        if(s.length()<t.length()) return "";

        Map<Character,Integer> need = new HashMap<>();
        for(char c: t.toCharArray()){
            need.put(c,need.getOrDefault(c,0)+1);
        }

        int have = 0 , needl = need.size();
        int left = 0 , min_len = Integer.MAX_VALUE;
        int start = 0;

        Map<Character,Integer> window = new HashMap<>();
        for(int right = 0; right<s.length() ; right++){
            char c = s.charAt(right);
            window.put(c,window.getOrDefault(c,0)+1);

            if(need.containsKey(c) && need.get(c).intValue()==window.get(c).intValue()){
                have++;
            }

            while(have==needl){
                if(right-left+1<min_len){
                    min_len = right - left +1;
                    start = left;
                }
                char l =s.charAt(left);
                window.put(l,window.get(l)-1);
                if(need.containsKey(l) && window.get(l)<need.get(l)){
                    have --;
                }
                left++;
            }
        }
        return min_len == Integer.MAX_VALUE ? "" : s.substring(start,start+min_len);
    }
}
```
---

## 88 - Merge Sorted Array


**Problem Statment**

You are given two integer arrays `nums1` and `nums2`, sorted in non-decreasing order, and two integers `m` and `n`, representing the number of elements in `nums1` and `nums2` respectively.

Merge `nums1` and `nums2` into a single array sorted in non-decreasing order.

The final sorted array should not be returned by the function, but instead be stored inside the array `nums1`. To accommodate this, nums1 has a length of `m + n`, where the first m elements denote the elements that should be merged, and the last `n` elements are set to 0 and should be ignored. `nums2` has a length of `n`.

**Example:**

```text
Example 1:

Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]
Explanation: The arrays we are merging are [1,2,3] and [2,5,6].
The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.

Example 2:

Input: nums1 = [1], m = 1, nums2 = [], n = 0
Output: [1]
Explanation: The arrays we are merging are [1] and [].
The result of the merge is [1].

Example 3:

Input: nums1 = [0], m = 0, nums2 = [1], n = 1
Output: [1]
Explanation: The arrays we are merging are [] and [1].
The result of the merge is [1].
Note that because m = 0, there are no elements in nums1. The 0 is only there to ensure the merge result can fit in nums1.

```


```text
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int midx = m - 1;
        int nidx = n - 1;
        int right = m + n - 1;

        while (nidx >= 0) {
            if (midx >= 0 && nums1[midx] > nums2[nidx]) {
                nums1[right] = nums1[midx];
                midx--;
            } else {
                nums1[right] = nums2[nidx];
                nidx--;
            }
            right--;
        }        
    }
}
```
---
## 239 - Sliding Window Maximum

**Problem Statement**

You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.

*Return the max sliding window*.

**Example:**

```text
Example 1:

Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
Output: [3,3,5,5,6,7]
Explanation: 
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7

Example 2:

Input: nums = [1], k = 1
Output: [1]


```
**Code**

```text
import java.util.*;

class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if (nums == null || k <= 0) return new int[0];
        int n = nums.length;
        int[] res = new int[n - k + 1];
        Deque<Integer> dq = new LinkedList<>();
        int idx = 0;
        for (int i = 0; i < n; i++) {
            while (!dq.isEmpty() && dq.peekFirst() <= i - k) {
                dq.pollFirst();
            }
            while (!dq.isEmpty() && nums[dq.peekLast()] < nums[i]) {
                dq.pollLast();
            }
            dq.offerLast(i);
            if (i >= k - 1) {
                res[idx++] = nums[dq.peekFirst()];
            }
        }
        return res;
    }
}

```
---
## 424 - Longest Repeating Character Replacement

**Problem Statment**

You are given a string `s` and an integer `k`. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most `k` times.

Return the length of the longest substring containing the same letter you can get after performing the above operations.

**Example:**

```text:

Example 1:

Input: s = "ABAB", k = 2
Output: 4
Explanation: Replace the two 'A's with two 'B's or vice versa.

Example 2:

Input: s = "AABABBA", k = 1
Output: 4
Explanation: Replace the one 'A' in the middle with 'B' and form "AABBBBA".
The substring "BBBB" has the longest repeating letters, which is 4.
There may exists other ways to achieve this answer too.

```
**Code:**

```text
class Solution {
    public int characterReplacement(String s, int k) {
        
        int[] c = new int[26];
        int lon = 0 , left = 0 ,maxcount = 0;

        for(int right=0 ; right<s.length() ; right++){

            c[s.charAt(right)-'A']++;
            maxcount = Math.max(maxcount,c[s.charAt(right)-'A']);

            while((right-left+1) - maxcount>k){
                c[s.charAt(left)-'A']--;
                left++;
            }

            lon = Math.max(lon,right - left + 1);
        }

        return lon;
    }
}
```
---

## 438 -  Find All Anagrams in a String

**Problem Statment**

Given two strings `s` and `p`, return an array of all the start indices of `p's` [Anagram](# "An anagram is a word or phrase formed by rearranging the letters of different word of phrase, using all the original letters exactly once.")
 in `s`. You may return the answer in **any order**.

**Example:**

```text
Example 1:

Input: s = "cbaebabacd", p = "abc"
Output: [0,6]
Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".

Example 2:

Input: s = "abab", p = "ab"
Output: [0,1,2]
Explanation:
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".

```

**Code:**

```text
import java.util.*;
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        int[] count1 = new arr[26];
        int[] count2 = new arr[26];
        List<integer> l = new ArrayList<>();
        for(char c : toCharArray()){
            count1[c - 'a']++;
        }
        for(int i=0;i<s.length)
    }
}
```
---

## 611 - Valid Triangle Number

**Problem Statemnt**

Given an integer array `nums`, return the number of triplets chosen from the array that can make triangles if we take them as side lengths of a triangle.

**Example:**

```text
Example 1:

Input: nums = [2,2,3,4]
Output: 3
Explanation: Valid combinations are: 
2,3,4 (using the first 2)
2,3,4 (using the second 2)
2,2,3

Example 2:

Input: nums = [4,2,3,4]
Output: 4

```
**Code**

```text
class Solution {
    public int triangleNumber(int[] nums) {
        Arrays.sort(nums);
        int count = 0;
        for(int i = nums.length-1;i>=2;i--){
            int left = 0 , right = i-1;
            while(left<right){
                if(nums[left]+nums[right]>nums[i]){
                    count+=right-left;
                    right--;
                }
                else{
                    left++;
                }
            }
        }
        return count;
    }
}
```
---

## 812 - Largest Triangle Area

**Problem Statement:**

Given an array of points on the **X-Y** plane `points` where `points[i] = [xi, yi]` , return the area of the largest triangle that can be formed by any three different points. Answers within `10^-5` of the actual answer will be accepted.

**Example:**


```text

Example 1:
Input: points = [[0,0],[0,1],[1,0],[0,2],[2,0]]
Output: 2.00000
Explanation: The five points are shown in the above figure. The red triangle is the largest.

Example 2:

Input: points = [[1,0],[0,0],[0,1]]
Output: 0.50000
```

**Code:**
```text
class Solution {
    public double largestTriangleArea(int[][] points) {
        double max = 0;
        int n = points.length;

        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                for (int k = j + 1; k < n; k++) {
                    double area = 0.5 * Math.abs(
                        points[i][0] * (points[j][1] - points[k][1]) +
                        points[j][0] * (points[k][1] - points[i][1]) +
                        points[k][0] * (points[i][1] - points[j][1])
                    );
                    if (area > max) max = area;
                }
            }
        }
        return max;
    }
}

```

---

## 904 - Fruit into Baskets

**Problem Statment**

You are visiting a farm that has a single row of fruit trees arranged from left to right. The trees are represented by an integer array `fruits` where `fruits[i]` is the type of fruit the `ith` tree produces.

You want to collect as much `fruit` as possible. However, the owner has some strict rules that you must follow:

You only have two baskets, and each basket can only hold a single type of fruit. There is no limit on the amount of fruit each basket can hold.

Starting from any tree of your choice, you must pick exactly one fruit from every tree (including the start tree) while moving to the right. The picked fruits must fit in one of your baskets.

Once you reach a tree with fruit that cannot fit in your baskets, you must stop.

Given the integer array `fruits`, return the maximum number of fruits you can pick.

**Example:**

```text
Example 1:

Input: fruits = [1,2,1]
Output: 3
Explanation: We can pick from all 3 trees.

Example 2:

Input: fruits = [0,1,2,2]
Output: 3
Explanation: We can pick from trees [1,2,2].
If we had started at the first tree, we would only pick from trees [0,1].

Example 3:

Input: fruits = [1,2,3,2,2]
Output: 4
Explanation: We can pick from trees [2,3,2,2].
If we had started at the first tree, we would only pick from trees [1,2].

```

**Code:**

```text

import java.util.*;
class Solution {
    public int totalFruit(int[] fruits) {
        int start = 0;
        int maxm = 0;
        Map<Integer,Integer> map = new HashMap<>();
        for(int end = 0 ; end<fruits.length ; end++){
            int currentc = map.getOrDefault(fruits[end],0);
            map.put(fruits[end],currentc+1);
            while(map.size()>2){
                int fc = map.get(fruits[start]);
                if(fc==1){
                    map.remove(fruits[start]);
                }
                else{
                    map.put(fruits[start],fc-1);
                }
                start++;
            }
            maxm = Math.max(maxm,end - start + 1);
        }
        return maxm;
    }
}
```
---
## 1004 - Max Consecutive One III

**Problem Statement**

Given a binary array `nums` and an integer `k`, return the maximum number of consecutive `1's` in the array if you can flip at most `k` `0's`.

**Example:**

```text
Example 1:

Input: nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2
Output: 6
Explanation: [1,1,1,0,0,1,1,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

Example 2:

Input: nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3
Output: 10
Explanation: [0,0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

```

**Code:**

```text
class Solution {
    public int longestOnes(int[] nums, int k) {
        int left = 0 , maxl = 0 , zerocount = 0;
        for(int right = 0;right<nums.length;right++){
            if(nums[right]==0){
                zerocount++;
            }
            while(zerocount>k){
                if(nums[left]==0){
                    zerocount--;
                }
                left++;
            }
            maxl = Math.max(maxl,right-left+1);
        }
        return maxl;
    }
}
```
---
## 1423 - Maximum Points You Can Obtain from Cards

**Problem Statement**

There are several cards arranged in a row, and each card has an associated number of points. The points are given in the integer array `cardPoints`.

In one step, you can take one card from the beginning or from the end of the row. You have to take exactly `k` cards.

Your score is the sum of the points of the cards you have taken.

Given the integer array `cardPoints` and the integer `k`, return the maximum score you can obtain.

**Example:**

```text
Example 1:

Input: cardPoints = [1,2,3,4,5,6,1], k = 3
Output: 12
Explanation: After the first step, your score will always be 1. However, choosing the rightmost card first will maximize your total score. The optimal strategy is to take the three cards on the right, giving a final score of 1 + 6 + 5 = 12.

Example 2:

Input: cardPoints = [2,2,2], k = 2
Output: 4
Explanation: Regardless of which two cards you take, your score will always be 4.

Example 3:

Input: cardPoints = [9,7,7,9,7,7,9], k = 7
Output: 55
Explanation: You have to take all the cards. Your score is the sum of points of all cards.

```

**Code:**
```text
class Solution {
    public int maxScore(int[] cardPoints, int k) {
        int sum = 0;

        for(int i = 0 ; i<k ; i++) sum += cardPoints[i];
        
        int sum2 = sum;

        for(int i=k-1 , j=cardPoints.length-1 ; i>=0 ; i-- , j--){
            sum += cardPoints[j]-cardPoints[i];
            sum2 = Math.max(sum2 , sum); 
        }

        return sum2;
    }
}
```
---

## 1493 - Logest SubArray of 1'S After Deleting One Element

**Problem Statement**

Given a binary array `nums`, you should delete one element from it.

Return the size of the longest non-empty subarray containing only `1's` in the resulting array. Return `0` if there is no such subarray.

**Example:**

```text
Example 1:

Input: nums = [1,1,0,1]
Output: 3
Explanation: After deleting the number in position 2, [1,1,1] contains 3 numbers with value of 1's.

Example 2:

Input: nums = [0,1,1,1,0,1,1,0,1]
Output: 5
Explanation: After deleting the number in position 4, [0,1,1,1,1,1,0,1] longest subarray with value of 1's is [1,1,1,1,1].

Example 3:

Input: nums = [1,1,1]
Output: 2
Explanation: You must delete one element.

```

**Code:**

```text
class Solution {
    public int longestSubarray(int[] nums) {
        int left = 0 ,maxl = 0 , zerocount = 0;
        for(int right=0;right<nums.length;right++){
            if(nums[right]==0){
                zerocount++;
            }
            while(zerocount > 1){
                if(nums[left]==0){
                    zerocount--;
                }
                left++;
            }
            maxl = Math.max(maxl,right-left);
        }
        return maxl;
    }
}
```
