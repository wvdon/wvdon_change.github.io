---
title: leetCode
date: 2019-09-27 20:42:00
---

## leetCode刷题笔记

> 每部分刷题笔记都从 
>
> - 题解 
> - 思路
> - 考察
> - 实现 
> - 优化
>
> 五个方面进行解释

刷题地址：https://leetcode-cn.com/

# 链表：

链表题目：`一个原则， 两种题型，三个注意，四个技巧`

一个原则：画图
两个考点：

- 指针的修改

  - 最典型的指针的修改
- 链表的拼接



`01 Two sum 2.16`

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]: 
        for i in nums:
            j = target - i
            start_index = nums.index(i)
            next_index = start_index + 1
            temp_nums = nums[next_index:]
            if j in temp_nums:
                return(start_index,temp_nums.index(j)+next_index)
```







## 题目顺序如下 

#### [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

> 给定 nums = [2, 7, 11, 15], target = 9
>
> 因为 nums[0] + nums[1] = 2 + 7 = 9
> 所以返回 [0, 1]

**题解**

从数组中选出和为target的两个数，输出他的下标

**思路**

- 思路一

  遍历数组，使用两个for循环，复杂度是 O(nlog(n))

- 思路二

  利用hashmap将键值存入map中，然后循环，如果存在target-nums[i]时 返回数组下标

**考察**

数组和hashmap的使用

**实现**

```java
class Solution{
    /**方法一 暴力破解 复杂度为 O(nlog(n))
     *执行用时 :51 ms , 在所有 Java 提交中击败了 39.43% 的用户
     *
     *内存消耗 :37.1 MB 在所有 Java 提交中击败了92.26% 的用户
     *
     * **/
    public int[] twoSum(int[] nums, int target) {
        int a[] = new int[2];
        for(int i=0;i<nums.length;i++){
            for(int j=i+1;j<nums.length;j++){
                int sum = nums[i]+nums[j];
                if(sum==target){
                    a[0]=i;
                    a[1]=j;
                }
            }
        }
        return a;
    }
}

```

**优化**

```java
class Solution1{
    /**方法二 参考 画解算法
     **执行用时 :3 ms , 在所有 Java 提交中击败了 99.11% 的用户
     *
     *内存消耗 :37.2 MB 在所有 Java 提交中击败了90.61% 的用户
     * @param nums
     * @param target
     * @return
     */
    public int[] twoSum(int[] nums, int target) {
        HashMap<Integer,Integer> map = new HashMap<>();
        for(int i=0;i<nums.length;i++){
            if(map.containsKey(target-nums[i])){
                return new int[]{map.get(target-nums[i]),i};
            }
            map.put(nums[i],i);
        }
        throw new IllegalArgumentException("No two sum solution");
    }
}
```

#### [2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/)

给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

> ```
> 输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
> 输出：7 -> 0 -> 8
> 原因：342 + 465 = 807
> ```

**题解**

返回一个求和的新链表

**思路**

添加一个预先指针，判断下一位是否进位

**考察**

利用链表进行加减

**实现**

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode pre = new ListNode(0);
        ListNode cur = pre;
        int carry=0;
        while(l1!=null||l2!=null){
            int x = l1==null?0:l1.val;
            int y = l2==null?0:l2.val;
            int sum = x+y+carry;
            carry = sum/10;
            sum=sum%10;
            cur.next = new ListNode(sum);
            cur=cur.next;
            if(l1!=null){
                l1=l1.next;
            }
            if(l2!=null){
                l2=l2.next;
            }

        }
        if (carry==1){
            cur.next=new ListNode(1);

        }
        return pre.next;
    }
}
```



#### [3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

示例 1:

> 输入: "abcabcbb"
> 输出: 3 
> 解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
> 示例 2:
> 输入: "bbbbb"
> 输出: 1
> 解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
> 示例 3:
> 输入: "pwwkew"
> 输出: 3
>
> > 解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
> >      请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串

题解

在所给的字符串中找到那个前后不重复且长度最长的子串

思路

将key存在hashmap中，如果不相同就++，存在相同的情况下就更新max值

考察

字符串的匹配，滑动窗口

实现

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        HashMap<Character,Integer> map = new HashMap<>();
        int start =0;
        int max=0;
        for(int i=0;i<s.length();i++){
            if(map.containsKey(s.charAt(i))){
               start = Math.max(start,map.get(s.charAt(i)));      
            }
            max = Math.max(max,i-start+1);
            map.put(s.charAt(i),i+1);      
        }
        return max;
    }
}
```



#### [6. Z 字形变换](https://leetcode-cn.com/problems/zigzag-conversion/)

将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 "LEETCODEISHIRING" 行数为 3 时，排列如下：

L   C   I   R
E T O E S I I G
E   D   H   N
之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："LCIRETOESIIGEDHN"。

请你实现这个将字符串进行指定行数变换的函数：

string convert(string s, int numRows);

示例 1:

> 输入: s = "LEETCODEISHIRING", numRows = 3
> 输出: "LCIRETOESIIGEDHN"

示例 2:

> 输入: s = "LEETCODEISHIRING", numRows = 4
> 输出: "LDREOEIIECIHNTSG"
> 解释:
>
> L     D     R
> E   O E   I I
> E C   I H   N
> T     S     G

题解

将原先的字符串按照z字形进行转换输出

思路

可以构造n个 StringBuilder，然后定义一个 res 判断z字形的下一个元素是 进入到 str[i+1]还是 str[i-1]上

考察

字符串操作

实现

```java
class Solution {
    public String convert(String s, int numRows) {
        if(numRows<2){
            return s;
        }
        List<StringBuilder>  strBuilder = new ArrayList<StringBuilder>(); 
        for(int i = 0; i < numRows; i++) strBuilder.add(new StringBuilder());
        int res=-1;
        int j=0;
        for (int i=0;i<s.length();i++){
            strBuilder.get(j).append(s.charAt(i));
            if(j==0||j==(numRows-1)){
               res=-res;}
            j=res+j;
    }
      StringBuilder stb = new StringBuilder();
    for (int i =0;i<numRows;i++){
        stb.append(strBuilder.get(i).toString());
    }
               return stb.toString();
               
}
}
```



#### [7. 整数反转](https://leetcode-cn.com/problems/reverse-integer/)

给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

示例 1:

> 输入: 123
> 输出: 321

 示例 2:

> 输入: -123
> 输出: -321

示例 3:

> 输入: 120
> 输出: 21

注意:

假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−2^31,  2^31 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。

思路

可以利用StringBuilder的 reverse()方法将字符进行翻转。

考察

字符串的操作吧！

实现

```java
class Solution {
    public int reverse(int x) {
      if (x>2147483647||x<-2147483648||x==0)
          return 0;
        StringBuilder strBuilder = new StringBuilder();       
        if(x<0){
            x=-x;
            strBuilder.append(x);
            try{
                return -Integer.parseInt(strBuilder.reverse().toString());
            }catch(Exception e){
                return 0;
            }            
        }
        else{
            while(x%10==0)
                x/=10;
            strBuilder.append(x);
            try{
                return Integer.parseInt(strBuilder.reverse().toString());
            }catch(Exception e){
                return 0;
            }            
        }    
    }
}
```



#### [9. 回文数](https://leetcode-cn.com/problems/palindrome-number/)

题解

判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

示例 1:

> 输入: 121
> 输出: true

示例 2:

>  输入: -121
> 输出: false
> 解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。

示例 3:

> 输入: 10
> 输出: false
> 解释: 从右向左读, 为 01 。因此它不是一个回文数。

思路

直接用reverse()

需要注意的是在进行比较的时候 int转为str可以是 int+""

考察

实现

```java
class Solution {
    public boolean isPalindrome(int x) {
        StringBuilder strBuilder = new StringBuilder(x+"").reverse();
        return (x+"").equals(strBuilder.toString());
        
    }
}
```

优化

#### [14. 最长公共前缀](https://leetcode-cn.com/problems/longest-common-prefix/)

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。

示例 1:

> 输入: ["flower","flow","flight"]
> 输出: "fl"

示例 2:

> 输入: ["dog","racecar","car"]
> 输出: ""

解释: 输入不存在公共前缀。
说明:

所有输入只包含小写字母 a-z 。

来源：力扣（LeetCode）

题解

找到字符串相同的前缀

思路

直接设置一个字符串，如果字符串前缀相同，则存入，否则 跳出；

考察

字符串操作

实现

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if(strs.length==0||strs[0]==""){
            return "";
        }
        String str = strs[0];
        for(int i=1;i<strs.length;i++){
            int j=0;
            for(;j<str.length()&&j<strs[i].length();j++){
                if(str.charAt(j)!=strs[i].charAt(j))
                    break;
            }
            str = str.substring(0,j);
            if(str==""){
                return str;
            }
        }
        return str;
    }
}
```

## 加油

刷题地址：https://leetcode-cn.com/

**已解决 12/2074**

### 剑指offer 

#### 模板

**题解**

**思路**

**考察**

**实现**

**优化**

