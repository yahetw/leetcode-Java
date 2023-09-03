今天的題目有兩個，都是Array類型的題目，而且會用到HashMap，

HashMap的解釋以及常用方法放在文章的最後，有需要的人可以看！

題目如下：

[217. Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)

[1. Two Sum](https://leetcode.com/problems/two-sum/)

# Contains Duplicate

問題：一串陣列中，如果有一個數字連續出現兩次，回傳True；否則回傳False。

**Example 3:**

<pre><strong>Input:</strong> nums = [1,1,1,3,3,4,3,2,4,2]
<strong>Output:</strong> true</pre>


### Sort

首先將陣列排序，並遍歷陣列一次(n-1)，如果元素和下一個元素相同，回傳True；否則False。

但會花 O(nlogn) 的時間排序。

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        // 排序 O(nlogn)
        Arrays.sort(nums); 
        for (int i = 0; i < nums.length - 1; i++) {
                if (nums[i] == nums[i+1]) {
                    return true;
                }
        }
        return false;
    }
}
```


Time Complexity: O(nlogn)，排序

Space Complexity: O(1)



### HashSet

用空間換取時間，遍歷陣列的同時將每個元素存入HashSet，

並檢查有沒有相同的元素。

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> hashset = new HashSet<>();

        for(int i = 0 ; i< nums.length;i++){
            if(!hashset.contains(nums[i])) hashset.add(nums[i]);
            else return true;
        }
        return false;
    }
}
```

Time Complexity: O(n)

Space Complexity: O(n)

### HashMap

這題實際上比較適合用HashSet解，因為不太需要HashMap鍵值對應的特性，

不過這裡也放上來，思路跟上一題類似。

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        HashMap<Integer,Integer> hashmap = new HashMap<>();
        for(int i =0;i<nums.length;i++){
            if(!hashmap.containsKey(nums[i])) hashmap.put(nums[i],i);
            else return true;
        }
        return false;
    }
}
```

Time Complexity: O(n)

Space Complexity: O(n)


# Two Sum

問題：一串陣列中，某兩個元素相加會等於target，如果有則回傳兩個元素的index；如果沒有則回傳空陣列

**Example 1:**

<pre><strong>Input:</strong> nums = [2,7,11,15], target = 9
<strong>Output:</strong> [0,1]
<strong>Explanation:</strong> Because nums[0] + nums[1] == 9, we return [0, 1].</pre>

這裡介紹兩種解法，第一種是暴力破解，會需要用到O(n^2)的時間；第二種則是HashMap，只用到O(n)的時間。

### Brute Force

最直覺的作法就是使用巢狀迴圈，一步步找出目標值，

如果找到目標值，則直接回傳。

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int n = nums.length;
        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                if (nums[i] + nums[j] == target) {
                    return new int[]{i, j};
                }
            }
        }
        return new int[]{}; // No solution found
    }
}
```

Time Complexity: O(n^2)，巢狀迴圈

Space Complexity: O(1)，只使用常數變數


### HashMap

1. 首先先建立一個HashMap，存放所有元素。
2. 透過補數(complement)，尋找每個nums[j]符合條件的值
3. 如果找到符合條件的值(不包含自己)，則回傳答案

這時如果有兩個相同的元素出現在同一個陣列的時候，怎麼辦呢!

請看圖解~




```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        //hash table solution
        HashMap<Integer,Integer> num_hash = new HashMap<>();

        for(int i=0;i<nums.length;i++){
            //在hashtable中放入資料
            num_hash.put(nums[i],i);
        }

        //找出補數並搜尋
        for(int j=0;j<nums.length;j++){
            int complement = target - nums[j];
            if(num_hash.containsKey(complement) && num_hash.get(complement) != j){
                //要回傳index, 故要找出complement的index
                return new int[] {j,num_hash.get(complement)};
            }
        }
        return new int[]{};

        //hashmap.get() 方法，取得指定key的value
    }
}
```

Time Complexity: O(n)，陣列遍歷一次

Space Complexity: O(n)，Worst Case會包含所有Array的資料


# HashMap

HashMap以每個獨立的 key 對應一個 value

1. 資料(K, V)原則上是一對一
2. key不能重複，若重複宣告，後者會取代前者
3. 資料不會排序，預設是先宣告的在前面(呼叫整個 HashMap 的話)

插入：O(1)

尋找：O(1)

刪除：O(1)

## 常用方法

// import
import java.util.HashMap;

// 宣告，以下幾種都可。看儲存的資料類型
HashMap<String, String> hashmap = new HashMap<String, String>();
HashMap<String, String> hashmap = new HashMap<>();
HashMap<String, Integer> hashmap = new HashMap<>();

// 新增元素
hashmap.put(1, "Google");
hashmap.put(2, "Runoob");
hashmap.put(3, "Taobao");

// 檢查該key是否存在(return bool)

hashmap.containsKey()

// 清空 HashMap

hashmap.clear();


參考資料:

- [[Java] HashMap資料結構簡介與用法 (hellowalk.blogspot.com)](http://hellowalk.blogspot.com/2017/10/java-hashmap.html)
- [Java HashMap | 菜鸟教程 (runoob.com)](https://www.runoob.com/java/java-hashmap.html)
