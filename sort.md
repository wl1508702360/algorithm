
# 常见的排序算法

根据排序算法的不同，主流的排序算法可以分为3类  

**1. 时间复杂度为`O(n^2)`的排序算法**
* 冒泡排序
* 选择排序
* 插入排序
* 希尔排序（性能略优于`O(n^2)`，但是又比不上O(nlogn)）

**2. 时间复杂度为`O(nlogn)`的排序算法**
* 快速排序
* 归并排序
* 堆排序
  
**3. 时间复杂度为`O(n)`的排序算法**
* 计数排序
* 桶排序
* 计数排序  

<br/> 

# 冒泡排序

什么是冒泡排序？  

    它是一种基础的`交换排序`，冒泡排序算法中的每一个元素都可以像小气泡一样，根据自身大小，一点的向着数据的一侧移动。是一种稳定排序（值相等的元素不会打乱原本的顺序）

冒泡排序的思想？  

    把相邻的元素两两比较，当一个元素大于右侧相邻元素时，交换它们的位置；当元素小于或等于右侧相邻元素时，位置不变

代码实现  
```php
<?php
    function bubbleSort($arr)
    {
        $count = count($arr);
        for ($i = 0; $i < $count; $i++) {
            for ($j = 0; $j < $count - $i - 1; $j++) {
                # 当一个元素大于右侧相邻元素是，交换他们的位置
                if ($arr[$j] > $arr[$j+1]) {
                    $temp = $arr[$j];
                    $arr[$j] = $arr[$j+1];
                    $arr[$j+1] = $temp;
                }
            }
        }
        return $arr;
    }
```

冒泡排序优化一  
<p style="text-indent: 20px">情况：有10轮排序，当在第6轮时，整个数列都是有序的了，可是排序算法仍还在排序，这个时候如何优化</p>
<p style="text-indent: 20px">方法：通过是否有序标记进行判断，如果标记是true说明就不用再进行排序了</p>  
<p style="text-indent: 20px">代码如下：</p>

```php
<?php
    function bubbleSort($arr)
    {
        $count = count($arr);
        for ($i = 0; $i < $count; $i++) {
            $isSorted = true;
            for ($j = 0; $j < $count - $i - 1; $j++) {
                if ($arr[$j] > $arr[$j+1]) {
                    $temp = $arr[$j];
                    $arr[$j] = $arr[$j+1];
                    $arr[$j+1] = $temp;
                    $isSorted = false;
                }
            }
            if ($isSorted) {
                break;
            }
        }
    }
```

冒泡排序优化二
<p style="text-indent: 20px">情况：有一数列，前半部分的元素无序，后半部分的元素按升序排序，并且后半部分元素中的最小值也大于前部分元素的最大值</p>
<p style="text-indent: 20px">方法：记录最后一次元素交换的位置，该位置即为无序数列的边界，再往后就是有序数列</p>  
<p style="text-indent: 20px">代码如下：</p>

```php
<?php
    function bubbleSort($arr)
    {
        $count = count($arr);
        $lastExchangeIndex = 0;
        $sortBorder = $count;
        for ($i = 0; $i < $count; $i++) {
            $isSorted = true;
            for ($j = 0; $j < $sortBorder; $j++) {
                if ($arr[$j] > $arr[$j+1]) {
                    $temp = $arr[$j];
                    $arr[$j] = $arr[$j+1];
                    $arr[$j+1] = $temp;
                    $lastExchangeIndex = $j;
                    $isSorted = false;
                }
            }
            $sortBorder = $lastExchangeIndex;
            if ($isSorted) {
                break;
            }
        }
        return $arr;
    }
```