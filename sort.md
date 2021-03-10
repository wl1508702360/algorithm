
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

# 快速排序

什么是快速排序？  

    也是一种`交换排序`，通过元素之间的比较和交换位置来达到排序的目的

冒泡排序的思想？  

    采用分治法的思想，在每一轮挑选一个基准元素，并让其他比它大的元素移动到数列的一边，比它小的元素移动到数列的另一边，从而把数列拆解成两部分。

普通写法
```php
<?php
    function quickSort($arr)
    {
        $count = count($arr);
        if ($count <= 1) {
            return $arr;
        }
        
        $left = $right = [];
        $basic = $arr[0];
        for ($i = 1; $i < $count; $i++) {
            if ($arr[$i] < $basic) {
                $left[] = $arr[$i];
            } else {
                $right[] = $arr[$i];
            }
        }

        $left = quickSort($left);
        $right = quickSort($right);
        return array_merge($left, [$basic], $right);
    }
```
双边循环法

从两边交替遍历元素
```php
<?php
    function quickSort($arr, $startIndex, $endIndex)
    {
        if ($startIndex >= $endIndex) {
            return;
        }

        // 得到基准元素的位置
        $pivotIndex = partition($arr, $startIndex, $endIndex);
        quickSort($arr, $startIndex, $pivotIndex - 1);
        quickSort($arr, $pivotIndex + 1, $endIndex);
    }

    function partition($arr, $startIndex, $endIndex)
    {
        // 取第一个位置（也可以选择随机位置）的元素作为基准元素
        $pivot = $arr[$startIndex];
        $left = $startIndex;
        $right = $endIndex;
        while ($left != $right) {
            // 控制right指针比较并向左移动
            while ($left < $right && $arr[$right] > $pivot) {
                $right++;
            }

            // 控制left指针比较并向右移动
            while ($left < $right && $arr[$left] <= $pivot) {
                $left++;
            }

            // 交换left和right指针所指向的元素
            if ($left < $right) {
                $temp = $arr[$left];
                $arr[$left] = $arr[$right];
                $arr[$right] = $temp;
            }
        }

        // pivot和指针重合点交换
        $arr[$startIndex] = $arr[$left];
        $arr[$left] = $pivot;
        return $left;
    }
```

单边循环法

从数组的一边对元素进行遍历和交换
```php
<?php
    function quickSort($arr, $startIndex, $endIndex)
    {
        if ($startIndex >= $endIndex) {
            return;
        }
        $pivotIndex = partition($arr, $sartIndex, $endIndex);
        quickSort($arr, $startIndex, $pivotIndex - 1);
        quickSort($arr, $pivotIndex + 1);
    }

    function partition($arr, $startIndex, $endIndex)
    {
        $pivot = $arr[$startIndex];
        $mark = $startIndex;
        for ($i = $startIndex + 1; $i < $endIndex; $i++) {
            // 
            if ($arr[$i] < $pivot) {
                $mark++;
                $temp = $arr[$mark];
                $arr[$mark] = $arr[$i];
                $arr[$i] = $temp;
            }
        }

        // 把pivot元素交换到mark指针所在的位置
        $arr[$startIndex] = $arr[$mark];
        $arr[$mark] = $pivot;
        return $mark;
    }
```