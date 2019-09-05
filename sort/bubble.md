
### 优化冒泡算法 -- [生命不止折腾不息]

- 优化冒泡排序
-  @author   lidawei
- @date     2019/9/8
```mysql
 思路分析：就是像冒泡一样，每次从数组当中 冒一个最大的数出来。
 你可以这样理解：（从小到大排序）
 由底至上的把较少的气泡逐步地向上升，这样经过遍历一次后最小的气泡就会被上升到顶（下标为0）
 定义一个标识 这个flag就是判断有没有进入里面的for,不进去就代表排好了,就直接退出当次循环
 每个元素比较的次数,当前面排过 i次时,就以为这i次肯定是排好的 就跳出本次循环
```

```mysql
 解题方式    | 这儿，可能有用的    ojbk
 BubbleSort:
 @param array $container
 @return array
```

```mysql
function oo($arr){
    $count = count($arr)-1; //获取数组的长度   有多少个数组元素就最多就要排n-1次
    $flag = true;
    for($i = 0; $i < $count && $flag; $i++){
        $flag = false; //这个flag就是判断有没有进入里面的for,不进去就代表排好了,就直接退出当次循环
        for($j = $count-1; $j >= $i; $j--){
            if($arr[$j] > $arr[$j+1]){
                $temp      = $arr[$j];
                $arr[$j]   = $arr[$j+1];
                $arr[$j+1] = $temp;
                $flag      = true;
            }
        }
    }
    print_r($arr);
}
$arr = [3,4,5,7,89,2,1,5,643,23,546,77,82,32,221];
oo($arr);
```


--------------------- 
作者：UFO

原文：https://github.com/lidawei-ufo/MYSQL/blob/master/bubble.md

版权声明：本文为博主原创文章，转载请附上博文链接！
