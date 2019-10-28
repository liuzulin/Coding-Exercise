# Some-coding-exercises-
## Quora
### coolFeature   
这个题要注意有18个hidden test case, 我有两个没过超时了。
输入a，b两个array， 一个query array。query有两种type， 一种是[target]查从a中取一个数， b中取一个数，求加起来等于target的情况有多少种。第二种query是[index, num], 把b中在 index位置的数字改成num，这种query不需要输出。最后输出所有第一种query的result。
coolFeature
Give three array a, b and query. This one is hard to explain. Just read the example. Input:
a = [1, 2, 3]
b = [3, 4]
query = [[1, 5], [1, 1, 1], [1, 5]] 
Output: [2, 1]
```JAVA
package test;

import java.util.*;

public class test {
	public static void main(String[] args) {
		int[] a = {1,2,3};
		int[] b = {3,4};
		int[][] queries = {{1,5},{1,1,1},{1,5}};
		Solution sol = new Solution();
		int[] res = sol.coolFeature(a, b, queries);
		System.out.print(Arrays.toString(res));
	}
}

class Solution {
	public int[] coolFeature(int[] a, int[] b, int[][] queries) {
		List<Integer> res = new ArrayList<>();
		HashMap<Integer, Integer> map = new HashMap<>();
		for (int i: a) {
			if (!map.containsKey(i)) {
				map.put(i, 0);
			}
			map.put(i, map.get(i) + 1);
		}
		for (int[] query: queries) {
			if (query.length == 2) {
				res.add(findSum(map, b, query[1]));
			} else {
				change(b, query[1], query[2]);
			}
		}
		return toArray(res);
	}
	private int findSum(HashMap<Integer, Integer> map, int[] array, int target) {
		int res = 0;
		for (int i : array) {
			if (map.containsKey(target - i)) {
				res = res + (map.get(target - i));
			}
		}
		return res;
	}
	private void change(int[] array, int index, int num) {
		array[index] = num;
	}
	private int[] toArray(List<Integer> list) {
		int[] res = new int[list.size()];
		for (int i = 0; i < list.size(); i++) {
			res[i] = list.get(i);
		}
		return res;
	}
}
```

### product-sum
很简单，一个数字，求所有位数的乘积减去所有位数的和。
```JAVA
class Solution {
	public int productSum(int num) {
		int prod = 1;
		int sum = 0;
		while (num != 0) {
			int digit = num % 10;
			num = num /10;
			prod = prod * digit;
			sum = sum + digit;
		}
		return prod - sum;
	}
}
```

### wordIsValid
输入一组words和一组valid letters，判断有多少个words是valid。
判断条件是words里的所有upper and lower letter必须在valid letters里面。
如果word里面有special character不用管。注意valid letter只有小写，
但是words里面有大写的也算valid。
比如words = [hEllo##, This^^],
valid letter = [h, e, l, 0, t, h, s];
"hello##" 就是valid，因为h，e，l，o都在valid letter 里面，
“This^^” 不valid, 因为i不在valid letter里面
```JAVA
package test;

import java.util.*;

public class test {
	public static void main(String[] args) {
		String[] words = {"Hel^Lo", "leetCode"};
		char[] valid = {'h','e','l','l','o'};
		Solution sol = new Solution();
		int res = sol.wordIsValid(words, valid);
		System.out.print(res);
	}
}

class Solution {
	public int wordIsValid(String[] words, char[] valid) {
		HashSet<Character> set = new HashSet<>();
		for (char ch: valid) {
			set.add(Character.toLowerCase(ch));
		}
		int res = 0;
		for (String word: words) {
			boolean isValid = true;
			char[] input = word.toCharArray();
			for (char ch: input) {
				if (!Character.isLetter(ch)) {
					continue;
				}
				if (!set.contains(Character.toLowerCase(ch))) {
					isValid = false;
				}
			}
			if (isValid) {
				res++;
			}
		}
		return res;
	}
}

```

### brokenKeyboard
input: a = "Hello, my dear friend!", b = ['h', 'e', 'l', 'o', 'm']
output: 1 题目是键盘坏了，只剩下b中的字母按键和所有的数字和符号案件能用，同时shift键是好的， 所以可以切换大小写。问a中的单词有几个可以用当前坏掉的键盘打出来。 
```JAVA
package test;

import java.util.*;

public class test {
	public static void main(String[] args) {
		String sentence = "Hello, my dear friend, IT!";
		char[] valid = {'h','e','l','o','m','y','i','t'};
		Solution sol = new Solution();
		int res = sol.brokenKeyboard(sentence, valid);
		System.out.print(res);
	}
}

class Solution {
	public int brokenKeyboard(String sentence, char[] valid) {
		HashSet<Character> set = new HashSet<>();
		for (char ch: valid) {
			set.add(Character.toLowerCase(ch));
		}
		int res = 0;
		String[] words = sentence.split(" ");
		for (String word: words) {
			boolean isValid = true;
			char[] input = word.toCharArray();
			for (char ch: input) {
				if (!Character.isLetter(ch)) {
					continue;
				}
				if (!set.contains(Character.toLowerCase(ch))) {
					isValid = false;
				}
			}
			if (isValid) {
				res++;
			}
		}
		return res;
	}
}
```

### compareStringWithFrequency
compare两个string，只有小写字母。 每个stirng内部可以任意换位置，所以位置不重要。每个 string内部两个letter出现的频率也可以互换，所以这题只需要两个string每个frequency出现的 次数要一样。比如“babzccc” 和 “bbazzcz” 就返回“true”，因为z和c可以互换频率。 但是 “babzcccm” 和 “bbazzczl” 就不一样，因为m在第一个里出现过，第二个里没有出现过。
```JAVA
public static boolean compareString(String s1, String s2) {
        Map<Character, Integer> map1 = new HashMap<Character, Integer>();
        for(int i=0; i<s1.length(); i++) {
            map1.put(s1.charAt(i), map1.getOrDefault(s1.charAt(i), 0) + 1);
        }

        Map<Character, Integer> map2 = new HashMap<Character, Integer>();
        for(int i=0; i<s2.length(); i++) {
            map2.put(s2.charAt(i), map2.getOrDefault(s2.charAt(i), 0) + 1);
        }
        for(char ch : map1.keySet()) {
            if(!map2.containsKey(ch)) {
                return false;
            }
        }
        for(char ch : map2.keySet()) {
            if(!map1.containsKey(ch)) {
                return false;
            }
        }
        Map<Integer, Integer> countS1 = new HashMap<Integer, Integer>();
        for(char ch : map1.keySet()) {
            int freq = map1.get(ch);
            countS1.put(freq, countS1.getOrDefault(freq, 0) + 1);
        }

        Map<Integer, Integer> countS2 = new HashMap<Integer, Integer>();
        for(char ch : map2.keySet()) {
            int freq = map2.get(ch);
            countS2.put(freq, countS2.getOrDefault(freq, 0) + 1);
        }

        if(s1.length() != s2.length()) {
            return false;
        }
        for(int freq : countS1.keySet()) {
            if(countS1.get(freq) != countS2.get(freq)) {
                return false;
            }
        }
        return true;
    }

```

### findEvenDigit
Find how many numbers have even digit in a list. Ex.Input: A = [12, 3, 5, 3456] Output: 2
```JAVA
package test;

import java.util.*;

public class test {
	public static void main(String[] args) {
		int[] nums = {1, 23, 3456, 2, 0};
		Solution sol = new Solution();
		int res = sol.findEvenDigit(nums);
		System.out.print(res);
	}
}

class Solution {
	public int findEvenDigit(int[] nums) {
		int res = 0;
		for (int i: nums) {
			String cur = String.valueOf(i);
			if (cur.length()%2 == 0) {
				res++;
			}
		}
		return res;
	}
}
```

### findMostCommonElements
Input: A = [2, 2, 3, 3, 5] Output: [2, 3] 
```JAVA
```

### maxRibbon
Given a list representing the length of ribbon, and the target number "k" parts of ribbon. we want to cut ribbon into k parts with the same size, at the same time we want the maximum size.
Ex.
Input: A = [1, 2, 3, 4, 9], k = 5
Output: 3
```JAVA
package test;

import java.util.*;

public class test {
	public static void main(String[] args) {
		int[] a = {1,2,3,4,9};
		int k = 5;
		Solution sol = new Solution();
		int res = sol.maxRibbon(a, k);
		System.out.print(res);
	}
}

class Solution {
	public int maxRibbon(int[] a, int k) {
		int right = 0;
		for (int i: a) {
			right += i;
		}
		int left = 0;
		int max = 0;
		while (left <= right) {
			int mid = (left + right)/2;
			int part = 0;
			for (int i = 0; i < a.length; i++) {
				part = part + a[i]/mid;
			}
			if (part >= k) {
				max = Math.max(max, mid);
				left = mid + 1;
			} else {
				right = mid - 1;
			}
		}
		return max;
	}
}
```

### goodTuple
Give an array and find the count of a pair number and a
single number combination in a row of this array. Target array is
a[i - 1], a, a[i + 1]
```JAVA
```

### rotateDiagonal
Don't know what it is talking about


### divideSubstring
Give a number ​n​ and digit number ​k​ find all serial substring is able to divisible n.
Input: n = 120, k = 2 Output: 2
Explain:
120 -> 12 and 20 120 % 12 == 0 (O)
```JAVA
package test;

import java.util.*;

public class test {
	public static void main(String[] args) {
		String input = "120";
		int k = 2;
		Solution sol = new Solution();
		int res = sol.divideSubstring(input, k);
		System.out.print(res);
	}
}

class Solution {
	public int divideSubstring(String input, int k) {
		int res = 0;
		int num = Integer.parseInt(input);
		HashSet<Integer> set = new HashSet<>();
		for (int i = 0; i <= input.length() - k; i++) {
			String temp = input.substring(i,i+k);
			int value = Integer.parseInt(temp);
			if (!set.contains(value) && value != 0) {
				if (num % value == 0) {
					res++;
				}
			}
			set.add(value);
		}
		return res;
	}
}
```

