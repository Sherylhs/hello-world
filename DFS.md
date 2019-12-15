DFS : 
给一个不包含'0'和'1'的数字字符串，每个数字代表一个字母，请返回其所有可能的字母组合。

下图的手机按键图，就表示了每个数字可以代表的字母。

Link: https://leetcode.com/problems/letter-combinations-of-a-phone-number/


```{java}
class Solution {
    public List<String> letterCombinations(String digits) {
        ArrayList<String> result = new ArrayList();
        if(digits == null || digits.equals("")){
            return result;
        }
        
        Map<Character, char[]> map = new HashMap<>();
        map.put('2', new char[] {'a','b','c'});
        map.put('3', new char[] {'d','e','f'});
        map.put('4', new char[] {'g','h','i'});
        map.put('5', new char[] {'j','k','l'});
        map.put('6', new char[] {'m','n','o'});
        map.put('7', new char[] {'p','q','r','s'});
        map.put('8', new char[] {'t','u','v'});
        map.put('9', new char[] {'w','x','y','z'});
        
        StringBuilder sb = new StringBuilder();
        helper(digits, sb, map, result);
        return result;
        
    }
    
    private void helper(String digits, StringBuilder sb, Map<Character, 
                        char[]> map, ArrayList<String> result){
        if(sb.length() == digits.length()){
            result.add(sb.toString());
            return;
        }
        
        for(char c : map.get(digits.charAt(sb.length()))){ 
        //get the 2 in "23"                                              
        //for(c: {'a','b','c'})  | for(c: {'d','e','f'})
            sb.append(c);
            helper(digits, sb, map, result);
            sb.deleteCharAt(sb.length()-1);
        }
        
        
        
    }
}
```
