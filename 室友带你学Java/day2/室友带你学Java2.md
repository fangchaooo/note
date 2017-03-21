

## 基础算法回顾

```Java
/**
 * Created by fc on 2017/3/21.
 */
public class Fibonacci {
    public static void main(String[] args) {
        int a = 40;
        System.out.println(Fib1(a));

    }
    /*
    斐波那契数列指的是这样一个数列 :
    1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233，377，610，987，1597，2584，4181，6765，10946，17711，28657，46368........
    这个数列从第3项开始，每一项都等于前两项之和。
     */

    //这样我们很容易写出递归版本,算法复杂度O(2^N)
    public static int Fib(int n){
        if(n==0 || n==1)
            return n;
        return Fib(n-1) +Fib(n-2);
    }

    //但在Java8之前还是不支持尾递归优化，所以当N大了之后，消耗内存太多。因此非递归可以缓解，但下面的算法仍然不是最佳。
    public static int Fib1(int n ){
        int i = 1, j =1;
        int x = 1;
        for(; n > 2; n--){
            x = i+j;
            i = j;
            j = x;
        }
        return x;
    }
}

```






#  选择排序

选择排序（Selection sort）是一种简单直观的排序算法。它的工作原理如下。**首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。以此类推，直到所有元素均排序完毕**。

选择排序的主要优点与数据移动有关。如果某个元素位于正确的最终位置上，则它不会被移动。选择排序每次交换一对元素，它们当中至少有一个将被移到其最终位置上，因此对n个元素的表进行排序总共进行至多n-1次交换。在所有的完全依靠交换去移动元素的排序方法中，选择排序属于非常好的一种。

- 时间复杂度	О(n²)

- 最优时间复杂度	О(n²)

- 平均时间复杂度	О(n²)

- 空间复杂度	О(n) total, O(1) auxiliary


![Select_sort](https://github.com/fangchaooo/note/blob/master/%E5%AE%A4%E5%8F%8B%E5%B8%A6%E4%BD%A0%E5%AD%A6Java/day2/Selection-Sort-Animation.gif)



```java
public static void sort(int[] arr){
        int i,j,min,temp,len=arr.length;//数组长度
        for (i = 0; i < len; i++){
          //将arr[i]和arr[i+1...len]中最小的数交换
            min = i;//先赋值arr最小数为第一个数，既下标为0的数arr[0]
            //遍历i后的所有数,如果找到大于min的数，则把其下标记录为最小数
            for (j = i+1; j<len;j++){
                if(arr[min]>arr[j])
                    min = j;
                //然后让查找后的最小值和当前位置上的数进行交换，当然你也发现，如果当前位置的就是最小数时还是要和自己进行交换。
                temp = arr[min];
                arr[min] = arr[i];
                arr[i] = temp;
            }
        }
```
