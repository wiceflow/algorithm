# 冒泡排序
## 简易版本
我们知道，冒泡排序是将数一个一个的比较，用升序来举例
定义一个数字数组 `int[] arr = {9,8,7,6,5}`  当我们要用冒泡排序对它进行升序处理的时候，就是将其元素与另外的元素一个一个进行比较，把大的一方一遍遍的往后挪。
第一趟: 找出最大的数  9

	第一次  8  9  7  6  5   9与8交换
	第二次  8  7  9  6  5   9与7交换
	第三次  8  7  6  9  5   9与6交换
	第四次  8  7  6  5  9   9与5交换
第二趟:找出8

	第一次  7  8  6  5  9   8与7交换
	第二次  7  6  8  5  9   8与6交换
	第三次  7  6  5  8  9   8与5交换
	第四次  7  6  5  8  9   8与9交换
第三趟:找出7

	第一次  6  7  5  8  9   7与6交换
	第二次  6  5  7  8  9   7与5交换
	第三次  6  5  7  8  9   7与8交换
	第四次  6  5  7  8  9   8与9交换
第四趟:找出6

	第一次  5  6  7  8  9   6与5交换
	第二次  5  6  7  8  9   6与7交换
	第三次  5  6  7  8  9   7与8交换
	第四次  5  6  7  8  9   8与9交换

这样就完成了一次冒泡。那么总共会进行 “数组的长度减去1次的趟数与数组数组长度减去1次的次数进行对比”
代码如下:
```java
public class bubbleSort01 {
    public static void main(String[] args) {
        int[] arr = {9,8,7,5,6};
        // 总共有n+-1趟  每一趟都是n-1次
        for (int i=0;i<arr.length-1;i++){
            for (int j=0;j<arr.length-1;j++){
                System.out.print("第" + (i+1) + "趟  " + "第" + (j+1) + "次");
                if (arr[j]>arr[j+1]){
                    int temp = arr[j];
                    arr[j] = arr[j+1];
                    arr[j+1] = temp;
                }
                System.out.println(Arrays.toString(arr));
            }
        }
        System.out.println(Arrays.toString(arr));
    }
}
```
我们可以看出上述代码中嵌套了两个for循环，总共进行了 `4*4=16` 次对比，不难发现，其实有一些对比是没有意义的。
当我们第一趟将数字`9`移到了最后之后，已经可以确认了这是最大的一个数，而在第二趟里面，数字`8`最后还是和数字`9`进行了一次比较，这显然就是没有意义的，还浪费了时间。同样当数字`8`放到了第二大的位置时候，进行第三趟对比的时候，又浪费了两次与数字`8` 数字`9`进行对比的时间。

## 简化版冒泡
在上面的基础上，我们要简化对比次数，当一些已经被确定下来的结果，就没必要去对比了
<font color=blue>注意：这里的例子全部都是升序排序，其他类型的排序可能有所不同</font>
```java
public static void sortSec(int[] arr){
        // 总共有n+-1趟  每一趟都是n-1次
        for (int i = 0; i < arr.length - 1; i++) {
            // 当越来越大的数往后面填充的时候，则与最大的数进行对比就是多余的
            // 这里进行了减i操作，确保每一趟循环都不会出现累赘对比
            for (int j = 0; j < arr.length-1-i; j++) {
                System.out.print("第" + (i + 1) + "趟  " + "第" + (j + 1) + "次");
                if (arr[j] > arr[j + 1]) {
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                }
                System.out.println(Arrays.toString(arr));
            }
        }
        System.out.println(Arrays.toString(arr));
    }
```


## 最终版冒泡
在实际生活中还会出现另外一种情况，就是数组本身就部分有序，需要冒泡排序的位置不多，一旦那些位置被排完后，数组可能就已经是一个升序数组，这时候如果再继续进行一遍遍的循环排序，那就是浪费时间了。
我们可以在循环中加入判定条件来检测数组是否已经达到了有序状态
eg：`int[] arr = {1,2,9,4,5}`

```java
public static void sortThree(int[] arr){
     // 判断是否退出循环标识
     boolean isBreak = true;
     // 总共有n+-1趟  每一趟都是n-1次
     for (int i = 0; i < arr.length - 1; i++) {
         // 每次进行循环前都将其初始化
         isBreak = true;
         // 当越来越大的数往后面填充的时候，则与最大的数进行对比就是多余的
         for (int j = 0; j < arr.length-1-i; j++) {
             System.out.print("第" + (i + 1) + "趟  " + "第" + (j + 1) + "次");
             if (arr[j] > arr[j + 1]) {
                 int temp = arr[j];
                 arr[j] = arr[j + 1];
                 arr[j + 1] = temp;
                 // 若进行了排序则设为false
                 isBreak = false;
             }
             System.out.println(Arrays.toString(arr));
         }
         // 一旦第二个循环中没有将标识符改变赋值，那就可以认为数组已经是有序的，这时候直接退出循环。
         if (!isBreak){
             break;
         }
     }
     System.out.println(Arrays.toString(arr));
 }
```
