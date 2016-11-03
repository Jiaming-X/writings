### Leetcode 196
### 296. Best Meeting Point

Link: https://leetcode.com/problems/best-meeting-point/

本以为跟 317的思路一样的，但是发现完全没有必要，由于语义问题，这道题根本没有障碍，所以任何一个人的家都可以成为一个meeting point.

又因为manhattan distance中两个variable完全是independent的，所以分别找1维解的答案就好，1维度的最小化manhattan distance就是数组中的median(太久没有写数学证明了，脑子简直生锈了)。

找median
1. 用quick selection的思想，可以达到average O(n)
2. 还有种思路：用median of medians algorithm

可参考：
http://cs.stackexchange.com/questions/1914/to-find-the-median-of-an-unsorted-array

比较不错的解释，为何median为manhattan问题的解？
https://discuss.leetcode.com/topic/46212/the-theory-behind-why-the-median-works

