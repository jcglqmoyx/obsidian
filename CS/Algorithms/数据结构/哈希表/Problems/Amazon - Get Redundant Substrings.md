Data analysts at Amazon are building a utility to identify redundant words in advertisements.

They define a string as redundant if the length of the string, `|word| = a * V + b * C`, where `a` and `b` are given integers and `V` and `C` are the numbers of vowels and consonants in the string `word`.

Given a string `word`, and two integers, `a`, and `b`, find the number of redundant substrings of `word`.

Note: A substring is a contiguous group of 0 or more characters in a string. For example- "bcb" is a substring of "abcba", while "bba" is not.

Example 1:

**Input: ** word = "abbacc", a = -1, b = 2  
**Output:** 5   

Example 2:

**Input: ** word = "akljfs", a = -2, b = 1  
**Output:** 15

```java
import java.util.HashMap;
import java.util.Map;

/*
    Link: https://www.fastprep.io/problems/amazon-get-redundant-substrings
    Difficulty: Hard
 */
public class GetRedundantSubstrings {

    private boolean isVowel(char c) {
        return c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u';
    }

    public int getRedundantSubstrings(String word, int a, int b) {
        int n = word.length();
        /*
                From
                    a×V + b×C = V + C <= n,
                we have
                    C/V = (1-a)/(b-1)
         */

        Map<Integer, Integer> map = new HashMap<>();
        map.put(0, 1);
        int c = 0, v = 0;
        int res = 0;
        a = 1 - a;
        b = b - 1;
        for (int i = 0; i < n; i++) {
            if (isVowel(word.charAt(i))) {
                c++;
            } else {
                v++;
            }

            int diff = c * a - v * b;

            if (map.containsKey(diff)) {
                res += map.get(diff);
            }

            map.put(diff, map.getOrDefault(diff, 0) + 1);
        }
        return res;
    }
}
```