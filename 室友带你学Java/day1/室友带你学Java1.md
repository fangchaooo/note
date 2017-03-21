#  复习数组最基本使用

```java
//注释就不写了 想象你的英语
    public static int findMaxNum(int []a){
        int max = a[0];
        for (int i =0;i<a.length;i++){
            if(a[i]>max) max = a[i];
        }
        return max;
    }
    public static double arrayAverage(int []a){
        int N = a.length;
        int sum = 0;
        for (int i = 0; i < N; i++){
            sum+=a[i];
        }
        double average = sum/N;
        return average;
    }

    public static int[] reserveArray(int []a){
        int N = a.length;
        for (int i = 0; i < N/2; i++){
            int temp = a[i];
            a[i] = a[N-1-i];
            a[N-i-1] = temp;
        }
        return a;

```


好了，我们开始第一个简单算法：**二分查找**

# **二分查找**又称折半查找

![二分查找](C:\Users\fc\Documents\GitHub\note\室友带你学Java\day1\捕获.PNG)


算法实现：

```java
/**
 * Created by fc on 2017/3/20.
 */
public class BinarySearch {
    public static int rank(int key, int[]a){
        int low = 0;
        int high = a.length-1;
        while (low<=high){
        //找到中间值
            int mid = (high+low)/2;
            // 如果查找值小于中间值，就从头一半中查找
            if(key<a[mid])
                high = mid-1;
            //大于中间值，就从后一半开始查找
            else if(key>a[mid])
                low = mid+1;
            //找到查找值位置
            else
                return mid;
        }
        return -1;
    }
    //递归版本原理是一样的，使用递归更能纯粹的表达二分查找的思想
    public static int rank2(int[] a,int high, int low, int key){
        if (low>high)
            return -1;
        int mid = low+(high-low)/2;
        if (key<a[mid])
            return rank2(a,mid-1,low,key);//带入的就是前一半
        else if (key>a[mid])
            return rank2(a,high,mid+1,key);//带入的就是后一半
        else
            return mid;
    }

    public static void main(String[] args) {
        //.............
    }
}

```
相信你现在觉得能写出二分查找了，但是一定要自己动手写一个，不用偷懒，就几分钟而已。


顺便秀把`python`

```python
def binary_search(arr,start,end,hkey):
	if start > end:
		return -1
	mid = start + (end - start) / 2
	if arr[mid] > hkey:
		return binary_search(arr, start, mid - 1, hkey)
	if arr[mid] < hkey:
		return binary_search(arr, mid + 1, end, hkey)
	return mid
```
