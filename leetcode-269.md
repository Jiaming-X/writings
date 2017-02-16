### Leetcode 321.

### Create Maximum Numbmer

题目链接：[https://leetcode.com/problems/create-maximum-number/](https://leetcode.com/problems/alien-dictionary/)

```java
public class Solution {
    Map<Character,Set<Character>> graph = new HashMap<>();
    Map<Character,Integer> indegreeMap = new HashMap<>();

    public boolean buildGraph(String[] words) {
        //注意:不做这步的话之后找indegree为0的起点就找不到了,但不能在loop里做,因为像["w","wr"]会因为后者长度更短而还没看到r就停了使indegree为空
        for(String w:words){
            for(char c:w.toCharArray()){
                if(!indegreeMap.containsKey(c)){
                    indegreeMap.put(c,0);
                }
            }
        }

        for (int i = 0; i < words.length-1; i++) {
            String cur = words[i];
            String next = words[i+1];

            if (cur.length() > next.length() && cur.substring(0, next.length()).equals(next)) {
                return false;
            }

            // find first different char then add edge
            //由于有["wrtkj","wrt"]即前面相同的短的反而在后面的情况,使得这个order无法确定需要return 空
            //所以在forj完成后判断来排除
            int limit = Math.min(cur.length(),next.length());
            int j = 0;
            for (; j <limit; j++) {
                char c1 = cur.charAt(j);
                char c2 = next.charAt(j);
                if(c1 == c2) continue;
                // add edge from c1 -> c2
                Set<Character> adjSet = graph.get(c1);

                if(adjSet == null){
                    adjSet = new HashSet<>();
                    graph.put(c1,adjSet);
                }
                //注意:对重复的边不能叠加indegree
                if(!adjSet.contains(c2)){
                    adjSet.add(c2);
                    indegreeMap.put(c2,indegreeMap.get(c2)+1);
                }
                break; //很重要,不然对wrf, er来说j会超出er的范围
            }
            // if(cur.length() > next.length() && j == limit) return false;
        }
        return true;
    }    

    public String alienOrder(String[] words) {
        if(words.length == 0) return "";
        if(words.length == 1) return words[0];
        if (!buildGraph(words)) {
            return "";
        }

        // topo sort (BFS)
        StringBuilder result = new StringBuilder();
        Queue<Character> queue = new LinkedList<>();
        // find node with indegree==0 as start
        for(Map.Entry<Character,Integer> entry:indegreeMap.entrySet()){
            if(entry.getValue() == 0){
                queue.add(entry.getKey());
                result.append(entry.getKey());
            }
        }

        while (!queue.isEmpty()){
            char c = queue.poll();
            if(graph.containsKey(c)){
                Set<Character> set = graph.get(c);
                for(char neighbour: set){
                    int newInDegree = indegreeMap.get(neighbour)-1;
                    indegreeMap.put(neighbour,newInDegree);
                    if(newInDegree == 0){
                        queue.add(neighbour);
                        result.append(neighbour);
                    }
                }
            }
        }

        //注意:["ri","xz","qxf","jhsguaw","dztqrbwbm","dhdqfb","jdv","fcgfsilnb","ooby"]应出""
        // 因为规则错了导致cycle，看j
        if(result.length() != indegreeMap.size()) return "";

        return result.toString();
    }
}
```



