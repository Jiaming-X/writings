# Leetcode 126




错误的答案：以为DFS就能直接做出来，但是时间复杂度实在是高得没法接受，也没法让leetcode接受。
```java
public class Solution {
    public List<List<String>> findLadders(String beginWord, String endWord, Set<String> wordList) {
        ArrayList<List<String>> result = new ArrayList<List<String>>();
        ArrayList<String> curr = new ArrayList<String>();
        HashSet<String> visit = new HashSet<String>();
        int[] minLen = new int[1];
        minLen[0] = Integer.MAX_VALUE;
        curr.add(beginWord);
        visit.add(beginWord);
        
        dfs(result, curr, visit, endWord, wordList, minLen);
        
        int size = result.size();
        for (int i = size - 1; i >= 0; i--) {
            if (result.get(i).size() > minLen[0]) {
                result.remove(i);
            }
        }
        
        return result;
    }
    
    public void dfs (ArrayList<List<String>> result, ArrayList<String> curr, HashSet<String> visit, String endWord, Set<String> wordList, int[] minLen) {
        String prev = curr.get(curr.size() - 1);
        if (prev.equals(endWord)) {
            minLen[0] = Math.min(minLen[0], curr.size());
            result.add(new ArrayList<String>(curr));
            return;
        } else if (curr.size() > minLen[0]) {
            return;
        }
        
        char[] prevStr = prev.toCharArray();
        
        for (int i = 0; i < prevStr.length; i++) {
            char charCopy = prevStr[i];
            for (int j = 0; j < 26; j++) {
                prevStr[i] = (char)('a' + j);
                String nextWord = new String(prevStr);
                if (charCopy != prevStr[i] && !visit.contains(nextWord) && wordList.contains(nextWord)) {
                    curr.add(nextWord);
                    visit.add(nextWord);
                    dfs(result, curr, visit, endWord, wordList, minLen);
                    visit.remove(nextWord);
                    curr.remove(curr.size() - 1);
                }
                prevStr[i] = charCopy;
            }
        }
    }
}
```


稍微好一点的先用BFS做将路基找出来。