**插入排序**，直接插入排序（Straight Insertion Srot)

### 基本思想
将一个记录插入到已排序好的有序表中，从而得到一个新的，记录数增1的有序表。即：先将序列的第一个记录看成是一个有序的子序列，然后从第二个记录逐个进行插入，直至整个序列有序为止。

**要点：设立哨兵，作为临时存储和判断数组边界之用。**

直接插入排序实例：

![Straight Insertion Sort Sample Image](http://my.csdn.net/uploads/201207/17/1342520948_8667.jpg)

如果碰到一个和插入元素相等的，那么把插入元素放在相等元素的后面。所以，相等元素的前后顺序没有改变，即有序序列中的相等元素顺序与其在元序列中的顺序是相同的。所以插入排序是稳定的。

### 算法实现

````java
	int[] unsortedArray = {10, 1, 5, 8, 49, 34, 100, 4, 1, 0, 90, 9, 80, 5};
	
    int[] straightInsertionSort(final int[] array) {
        int[] opArray = Arrays.copyOf(array, array.length);

        // 第一个元素作为有序表的开始, 比较逻辑从原无序表的第二个元素开始.
        for (int i = 1; i < opArray.length; i++) {
            // 如果当前遍历处的元素大于或等于有序表的最后一个元素, 会直接加入有序表.
            // 因为这个元素肯定会排在有序表的末尾.
            // 如下逻辑是处理遍历处元素小于有序表最后一位的情况.
            if (opArray[i] < opArray[i - 1]) {
                int temp = opArray[i]; // 临时保存遍历位元素

                // 用j保存遍历位在有序表中的下标.
                // 这个地方直接等于i,并不会有问题, 由if条件得知, j一定会小于i.
                int j = i;
                while (j > 0 && temp < opArray[j - 1]) {
                    // 如果前一个元素比标记位元素大, 则后移.
                    opArray[j] = opArray[j - 1];
                    j--;
                }
                opArray[j] = temp; // j代表有序表中标记位元素的下标, 直接插入有序表即可.
            }
        }
        return opArray;
    }
````
执行插入排序之后的结果是：

````
Original:[10, 1, 5, 8, 49, 34, 100, 4, 1, 0, 90, 9, 80, 5]
Sorted:[0, 1, 1, 4, 5, 5, 8, 9, 10, 34, 49, 80, 90, 100]
````