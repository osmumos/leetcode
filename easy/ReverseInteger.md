# Question
Click here to [View on LeetCode:](https://leetcode.com/problems/reverse-integer/) 
<br/><br/>
Given a 32-bit signed integer, reverse digits of an integer.

**Example 1:**
```
Input: 123
Output: 321
```
**Example 2:**
```
Input: -123
Output: -321
```
**Example 3:**
```
Input: 120
Output: 21
```
**Note:**
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231,  231 − 1]. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

<br/><br/>
# My Solution
## Thought Process
## Code Snippet
```java
class Solution {
    public int reverse(int x) {
        try{
            String intString = String.format("%d", x);
            char[] charArray = intString.toCharArray();
            if(x < 0){
                StringBuilder reversed = new StringBuilder("-");
                for (int i=charArray.length-1; i >= 1; i--){
                    if(!(i==charArray.length-1 && charArray[i] == '0')){
                        reversed.append(charArray[i]);
                    }
                }

                return Integer.parseInt(reversed.toString());
            }
            else {
                StringBuilder reversed = new StringBuilder();
                for (int i=charArray.length-1; i >= 0; i--){
                    if(!(i==charArray.length-1 && charArray[i] == '0')){
                        reversed.append(charArray[i]);
                    }
                }
                return Integer.parseInt(reversed.toString());
            }
        }
        catch(Exception ex){
            return 0;
        }
    }
}
```

## My Comments
<br/><br/>

# LeetCode Solution
## Approach 1: Maths for Pop and Push

**Algorithm**

Reversing an integer can be done similarly to reversing a string.

We want to repeatedly "pop" the last digit off of xx and "push" it to the back of the \text{rev}rev. In the end, \text{rev}rev will be the reverse of the xx.

To "pop" and "push" digits without the help of some auxiliary stack/array, we can use math.
```java
//pop operation:
pop = x % 10;
x /= 10;

//push operation:
temp = rev * 10 + pop;
rev = temp;
```
However, this approach is dangerous, because the statement \text{temp} = \text{rev} \cdot 10 + \text{pop}temp=rev⋅10+pop can cause overflow.

Luckily, it is easy to check beforehand whether or this statement would cause an overflow.

### Code
```java
class Solution {
    public int reverse(int x) {
        int rev = 0;
        while (x != 0) {
            int pop = x % 10;
            x /= 10;
            if (rev > Integer.MAX_VALUE/10 || (rev == Integer.MAX_VALUE / 10 && pop > 7)) return 0;
            if (rev < Integer.MIN_VALUE/10 || (rev == Integer.MIN_VALUE / 10 && pop < -8)) return 0;
            rev = rev * 10 + pop;
        }
        return rev;
    }
}
```

### Complexity Analysis
* Time Complexity: O(\log(x))O(log(x)). There are roughly \log_{10}(x)log 
10
​	
 (x) digits in xx.
* Space Complexity: O(1)O(1).

## Approach 2: Two-Pass Hash Table

### Code

