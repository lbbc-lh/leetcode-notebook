#### 217. Contains Duplicate
```
class Solution {

    public boolean containsDuplicate(int[] nums) {
        Set<Integer> uniques = new HashSet<>();//定义HashSet
        for (int i = 0; i < nums.length; i++) {
            if (uniques.contains(nums[i])) {
                return true;//如果HashSet有那个数，return true
            }
            uniques.add(nums[i]);//没有那个数，就往HashSet添加那个数→nums[i]
        }
        return false;
    }
}
```
* 本题把构造出的HashSet存入一个集合（Set），并且该集合中只能存储整数类型的数据，因为它被参数化为Integer
* 创建HashSet是首先需要创建一个HashSet类的对象→HashSet<E> hs = new HashSet<E>();
* 判断是否包含元素contains()
* 添加元素add()
* HashSet是Java中的一个集合类，它是基于哈希表的数据结构，用于存储一组不重复的元素。
