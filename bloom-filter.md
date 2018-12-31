# 如何判断一个元素是否存在于亿级数据中？
> 副标题：布隆过滤（Bloom Filter）

算法总结如下：
 1. 给定一个预设置长度的字节数组，
 2. 初始化亿级数据，对存入的元素做k次不同的hash运算，得到k个不同的hash值，设置数组对应的以hash值为偏移量的值为1；
 3. 判断一个元素是否存在与第二步的初始化数据中；即以相同的方式，计算该元素的hash值，并且分别判断数组上对应的值是否为1；
 4. 如果全为1，则说明这个元素可能存在；如果不全为1，则说明这个元素一定不存在；
 
 在有限长度的数组中大量的数据，即使是再完美的hash算法也会有冲突，因此，两个不同的元素，运算的hash值结果可能相同；
 
> 该算法只能判断数据是否一定不存在，不能判断一定存在，会有一定的误判；

自己实现一个布隆过滤，参考自：[crossoverJie  纯洁的微笑](https://mp.weixin.qq.com/s/GRQp4nK1vualrC--8SHxEg)
```java
public class BloomFilters {
    private int arraySize;
    private int[] array;

    public BloomFilters(int arraySize) {
        this.arraySize = arraySize;
        this.array = new int[arraySize];
    }

    public void add(String key) {
        int first = hashcode_1(key);
        int second = hashcode_2(key);
        int third = hashcode_3(key);
        
        array[first % arraySize] = 1;
        array[second % arraySize] = 1;
        array[third % arraySize] = 1;
    }

    
    public  boolean check(String key) {
        int first = hashcode_1(key);
        int firstIndex = array[first % arraySize];
        if (firstIndex == 0) {
            return false;
        }
        

        int second = hashcode_2(key);
        int secondIndex = array[second % arraySize];
        if (secondIndex == 0) {
            return false;
        }
        
        int third = hashcode_3(key);
        int thirdIndex = array[third % arraySize];
        if (thirdIndex == 0) {
            return false;
        }
        
        return true;
    }

    private int hashcode_1(String key) {
        // 省略
    }

    private int hashcode_2(String key) {
        // 省略
    }

    private int hashcode_3(String key) {
        // 省略
    }
}
```

### Guava中的BloomFilter
> [Guava Wiki - BloomFilter](https://github.com/google/guava/wiki/HashingExplained#bloomfilter)
> [Baeldung - Bloom Filter in Java using Guava](https://www.baeldung.com/guava-bloom-filter)

```java
BloomFilter<Person> friends = BloomFilter.create(personFunnel, 500, 0.01);
for (Person friend : friendsList) {
  friends.put(friend);
}
// much later
if (friends.mightContain(dude)) {
  // the probability that dude reached this place if he isn't a friend is 1%
  // we might, for example, start asynchronously loading things for dude while we do a more expensive exact check
}
```
