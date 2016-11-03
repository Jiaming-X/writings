### Leetcode 321. 
### Create Maximum Numbmer

题目链接：https://leetcode.com/problems/create-maximum-number/

个人认为这是一道很有意思的题目，既又要有很强的整体把握，而且十分具有技巧性的一题。

**思路**：

大思路是：利用迭代去找所有的长度组合[0, k], [1, k - 1] ... [k, 0]。 其中[i, j]的意思既是从 String 1 中拿 i 个元素。从 String 2中拿 j 个元素。

然后 findMaxSubArray 利用贪心的算法，找出数组中找出i个元素，并在相对顺序不被打乱的状态下组成最大的数。

接着，merge函数再将两个局部的方案合并成长度为k的一个解。

之所以说这个题目有技巧性是因为这个larger的函数，这既在更新全球最优解的时候用于比较旧的最优解，和新的一个解。也在 merge 函数中用到了。

错误的一个版本：
```java
    // Reminder: This is a wrong merge function!
    public static int[] merge(int[] one, int[] two) {
        int m = one.length, n = two.length;
        int[] result = new int[m + n];
        
        for (int i = 0, j = 0, count = 0; i < m || j < n;) {
            if (i == m) {
                result[count++] = two[j++];
            } else if (j == n) {
                result[count++] = one[i++];
            } else if (one[i] < two[j]) {
                result[count++] = two[j++];
            } else {
                result[count++] = one[i++];
            }
        }
        return result;
    }
```
这是我旧的思路，但是是错的。下面这个edge case没法过，但两个pointer指向两个相同的数的时候，往往需要看数组后面的情况来做判断。下面的case， 0和0相等，但是我们用第二个数组里面的0， 因为后面有6，可以让我们组成一个更大的数字。如果将完整的逻辑写到merge函数里面，将会相当长，所以干脆借用larger的思想去检查哪一个更有priority。
```
Input: [2,5,6,4,4,0]
       [7,3,8,0,6,5,7,6,2]
       15
       
Output:
[7,3,8,2,5,6,4,4,0,0,6,5,7,6,2]

Expected:
[7,3,8,2,5,6,4,4,0,6,5,7,6,2,0]
```

AC solution:
```java
public class Solution {
    public int[] maxNumber(int[] nums1, int[] nums2, int k) {
        int[] result = new int[k];
        
        for (int i = 0; i <= k; i++) {
            if (i <= nums1.length && k - i <= nums2.length) {
                int[] combined = merge(findMaxSubArray(nums1, i), findMaxSubArray(nums2, k - i));
                if (larger(combined, 0, result, 0)) {
                    result = combined;
                }
            }
        }
        return result;
    }
    
    public boolean larger(int[] one, int i, int[] two, int j) {
        while (i < one.length && j < two.length && one[i] == two[j]) {
            i++;
            j++;
        }
        if (i < one.length && j < two.length && one[i] != two[j]) {
            return one[i] > two[j];
        }
        return j == two.length;
    }
    
    public int[] findMaxSubArray (int[] array, int k) {
        if (k == 0) {
            return new int[0];
        }
        int[] result = new int[k];
        int num = 0;
        
        for (int i = 0; i < array.length; i++) {
            while (num != 0 && result[num - 1] < array[i] && k - num < array.length - i) {
                num--;
            }
            if (num < k) {
                result[num++] = array[i];
            }
        }
        return result; 
    }
    
    public int[] merge(int[] one, int[] two) {
        int m = one.length, n = two.length;
        int[] result = new int[m + n];
        
        for (int i = 0, j = 0, count = 0; i < m || j < n;) {
            if (larger(one, i, two, j)) {
                result[count++] = one[i++];
            } else {
                result[count++] = two[j++];
            }
        }
        return result;
    }
}
```
