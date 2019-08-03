#### 问题地址
[LeetCode#76](https://leetcode.com/problems/minimum-window-substring/)
#### 问题描述

给定两个字符串S和T，找出S中包含T的最小子串,要求时间复杂度是O(n).

> Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n).

#### 范例

```JavaScript
Input: S = "ADOBECODEBANC", T = "ABC"
Output: "BANC"
```

#### 提醒
- 如果没有这种字符串使得S包含T中所有的字符，返回空字符串"".
- 测试数据可以保证，如果存在这种子字符串，这种字符串有且只有一个.

#### 解题思路

- 先把t中字母出现次数用一个array统计好
- 遍历s 每当找到t中出现的字母时 就在array中更新(对应下标-1)
- 当所有t中字母都找到后 得到一个区间
- 然后去除该区间的第一个字母 再次在s中寻找新的含有t的区间
- 每次找到一个更短的区间 就更新 用substr形式输出

#### 代码
```Java
class Solution {
    public String minWindow(String s, String t) {
        
        int[] letters = new int[128];
        for(int i = 0; i < t.length(); i++) {
            int asciiValue = t.charAt(i);
            letters[asciiValue]++;
        }
        
        int count = 0;
        int minLength = Integer.MAX_VALUE;
        String ans = "";
        int left = 0;
        
        for(int i = 0; i < s.length(); i++) {
            int ascii = s.charAt(i);
            letters[ascii]--;
            if(letters[ascii] >= 0) {
                count++;
            }
            
            while(count == t.length()) {
                
                if(minLength > (i - left)){
                    minLength = i - left + 1;
                    ans = s.substring(left, i+1);
                }
                
                int leftAscii = s.charAt(left);
                letters[leftAscii] ++;
                if(letters[leftAscii] > 0) {
                    count--;
                }
                left++;
            }
        }
        
        return ans;
    }
}
```