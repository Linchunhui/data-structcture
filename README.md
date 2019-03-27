# Data-structcture
数据结构常用算法
## 排序
![排序算法及复杂度](https://github.com/Linchunhui/data-structcture/blob/master/640.png)
### 冒泡排序
        def bubbleSort(list):      #冒泡排序  稳定  时间复杂度O(n^2)  空间复杂度O（1）
        for i in range(1, len(list)):               #每遍历一次j，则排好一个最大值
            for j in range(0, len(list) - i):
                if list[j] > list[j + 1]:           #比较相邻元素，大的放后面
                    list[j], list[j + 1] = list[j + 1], list[j]
        return list
### 选择排序
### 插入排序
### 希尔排序
### 归并排序（递归以及非递归）
### 快速排序（递归以及非递归）
### 堆排序
### 基数排序
