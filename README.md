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
## 438 -  Find All Anagrams in a String

**Problem Statment**

Given two strings `s` and `p`, return an array of all the start indices of `p's` angrams in `s`. You may return the answer in **any order**.

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
