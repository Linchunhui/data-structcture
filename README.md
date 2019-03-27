# Data-structcture
数据结构常用算法
====

Index
---
<!-- TOC -->

- [排序](##排序)
    -[冒泡排序](#冒泡排序)  
    -[选择排序](#选择排序)  
    -[插入排序](#插入排序)  
    -[希尔排序](#希尔排序)  
    -[归并排序(递归)](#归并排序(递归))  
    -[归并排序(非递归)](#归并排序(非递归))  
    -[快速排序(递归)](#快速排序(递归))  
    -[快速排序(非递归)](#快速排序(非递归))  
    -[堆排序](#堆排序)  
    -[基数排序](#基数排序)  
 
<!-- /TOC -->
## 排序
![排序算法及复杂度](https://github.com/Linchunhui/data-structcture/blob/master/640.png)
### 冒泡排序
    def bubbleSort(self,list):      #冒泡排序  稳定  时间复杂度O(n^2)  空间复杂度O（1）
        for i in range(1, len(list)):               #每遍历一次j，则排好一个最大值
            for j in range(0, len(list) - i):
                if list[j] > list[j + 1]:           #比较相邻元素，大的放后面
                    list[j], list[j + 1] = list[j + 1], list[j]
        return list
### 选择排序
    def selectSort(self,list):      #选择排序   不稳定 时间复杂度 O(n^2)  空间复杂度O（1）
        for i in range(len(list)):
            minIndex=i                  #保存最小值索引
            for j in range(i+1,len(list)):
                if list[j]<list[minIndex]:
                    minIndex=j
            if i!=minIndex:
                list[i],list[minIndex]=list[minIndex],list[i]    #如果当前元素不是最小元素，则交换
        return list
### 插入排序
    def insertSort(self,list):      #插入排序   稳定  时间复杂度 O(n^2)  空间复杂度O（1）
        for i in range(len(list)):      #要插入的元素
            for j in range(i):           #已经排好序的前i-1个数
                if list[i]<list[j]:         #找到比他大的数，插入这个位置
                    list.insert(j,list.pop(i))      #将i出栈，插入到j位置
        return list
### 希尔排序
    def shellSort(self,list):       #希尔排序   不稳定  O（n） O(sqrt(n))
        gap=len(list)
        while gap>1:
            gap=gap//2                  #gap不断除以2
            for i in range(gap,len(list)):      #每一组的第一个
                for j in range(i%gap,i,gap):        #每组对应i的元素  每次步长gap
                    if list[j]>list[i]:                 #如果组里面前面的大就交换
                        list[i],list[j]=list[j],list[i]
        return list
### 归并排序（递归)
    def merge(self,left,right):
        list=[]
        while len(left) and len(right):
            if left[0]<=right[0]:               #添加数组头中小的元素
                list.append(left.pop(0))
            elif left[0]>right[0]:
                list.append(right.pop(0))
        if len(left)>0:
            list+=left
        if len(right)>0:
            list+=right
        return list

    def mergeSort(self,list):       #归并排序   递归   稳定  O（nlog2 (n)） O(1)
        if len(list)<=1:
            return list
        mid=len(list)//2
        left=self.mergeSort(list[:mid])
        right=self.mergeSort(list[mid:])
        return self.merge(left,right)
### 归并排序（非递归）
    def merge2(self,list,start,mid,end):
        list1=[]
        left=list[start:mid]
        right=list[mid:end]
        while len(left) and len(right):
            if left[0]<=right[0]:
                list1.append(left.pop(0))
            elif left[0]>right[0]:
                list1.append(right.pop(0))
        if len(left)>0:
            list1+=left
        if len(right)>0:
            list1+=right
        list[start:end]=list1

    def mergeSort2(self,list):          #非递归 归并排序   在原数组上修改
        i=1   #步长
        while i<len(list):
            low=0
            while low<len(list):
                mid=low+i                 #mid移动一个步长
                high=min(mid+i,len(list))     #high移动2个步长，考虑边界
                if mid<high:
                    self.merge2(list, low, mid, high)
                low+=2*i                    #low移动两个步长
            i*=2
        return list
### 快速排序（递归）
    def quickSort(self,list):               #快排  递归  不稳定  O（nlog2(n)）O（nlog2(n)）
        if len(list)<=1:
            return list
        first=list[0]
        left=[i for i in list[1:] if i<first]
        right = [i for i in list[1:] if i >=first]
        return self.quickSort(left)+[first]+self.quickSort(right)
### 快速排序（非递归）
    def partition(self,list,start,end):
        num=list[start]                 #保存第一个元素
        while start<end:
            while start<end and list[end]>=num:     #从后往前找比num小的数
                end-=1
            list[start]=list[end]                   #放到第一个位置
            while start<end and list[start]<num:       #从前往后找比num大的数
                start+=1
            list[end]=list[start]                       #放到后面位置
        list[start]=num                                 #把保存的第一个元素放到中间分隔左右
        return start                                    #返回中间元素下标

    def quickSort2(self,list):              #快排 非递归
        #用栈来保存索引
        if len(list)<2:
            return list
        stack=[]
        stack.append(len(list)-1)               #栈保存索引
        stack.append(0)
        while stack:                    #栈空终止
            l=stack.pop()
            r=stack.pop()               #出栈
            index=self.partition(list,l,r)          #分区并返回中间元素索引
            if l<index-1:                           #比较索引，循环分区
                stack.append(index-1)
                stack.append(l)
            if r>index+1:
                stack.append(r)
                stack.append(index+1)
        return list
### 堆排序
    def buildMaxHeap(self,arr):
        for i in range(len(arr)//2, -1, -1):
            self.heapify(arr, i)

    def heapify(self,arr, i):
        left = 2 * i + 1
        right = 2 * i + 2
        largest = i
        if left < arrLen and arr[left] > arr[largest]:
            largest = left
        if right < arrLen and arr[right] > arr[largest]:
            largest = right

        if largest != i:
            self.swap(arr, i, largest)
            self.heapify(arr, largest)

    def swap(self,arr, i, j):
        arr[i], arr[j] = arr[j], arr[i]

    def heapSort1(self,arr):            #堆排序    不稳定       O（nlog2n） O(1)
        global arrLen
        arrLen = len(arr)
        self.buildMaxHeap(arr)
        for i in range(len(arr) - 1, 0, -1):
            self.swap(arr, 0, i)
            arrLen -= 1
            self.heapify(arr, 0)
        return arr
### 基数排序
    def radix_sort(self,array):  #基数排序：透过键值的部份资讯，将要排序的元素分配至某些“桶”中，藉以达到排序的作用
        bucket, digit = [[]], 0     #稳定       O(d(n+r))   O(rd+n)
        while len(bucket[0]) != len(array):
            bucket = [[], [], [], [], [], [], [], [], [], []]
            for i in range(len(array)):
                num = (array[i] // 10 ** digit) % 10
                bucket[num].append(array[i])
            array.clear()
            for i in range(len(bucket)):
                array += bucket[i]
            digit += 1
        return array
