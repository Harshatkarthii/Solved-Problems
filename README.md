# Solved-Problems

## LeetCode Problem 76 - Minimum Window Substring

**Problem Statement:**

Given two strings `s` and `t` of lengths `m` and `n` respectively, return the minimum window of `s` such that **every character in `t` (including duplicates) is included in the window**.  

If there is no such substring, return the empty string `""`.  

The testcases will be generated such that the answer is unique.

**Example:**

```text
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
```
```Code
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
