
### Leetcode 140
### 140. Word Break II

DFS的思路就能解决这个题目，但是这道题精髓在于memorization, 刚开始没有写memorization的优化部分，所以上传的时候一直都是time-out的。

memorization 这部分实在是不熟悉，还希望多收集一些题目，加以整理。

```java
public class Solution {
    public List<String> wordBreak(String s, Set<String> wordDict) {
        ArrayList<String> result = new ArrayList<String>();
        HashMap<String, ArrayList<String>> map = new HashMap<String, ArrayList<String>>();
        if (s == null || s.length() == 0) {
            return result;
        }

        return dfs(s, wordDict, map);
    }
    
    public ArrayList<String> dfs (String s, Set<String> wordDict, HashMap<String, ArrayList<String>> map) {
        if (map.containsKey(s)) {
            return map.get(s);
        }
        ArrayList<String> res = new ArrayList<String>();
        if (s.equals("")) {
            res.add("");
            return res;
        }
        
        for (String one : wordDict) {
            if (s.startsWith(one)) {
                ArrayList<String> subList = dfs(s.substring(one.length()), wordDict, map);
                for (String subSoln : subList) {
                    res.add(one + (subSoln.isEmpty() ? "" : " ") + subSoln);
                }
            }
        }
        map.put(s, res);
        return res;
    }
}
```
