### 斐波那契数列 PHP实现（递归 + 非递归)

```mysql
斐波那契数列：
1 1 2 3 5 8 13 21 34 55 …

概念：
前两个值都为1，该数列从第三位开始，每一位都是当前位前两位的和
规律公式为：
Fn = F(n-1) + F(n+1)
F：指当前这个数列
n：指数列的下标
```

```mysql

非递归写法：
function f($n){
    if($n === 1 || $n == 0) return 1;
    return f($n-1) + f($n-2);
}

for($i = 0;$i < 20;$i++){
    echo f($i)."\n\r";
}
```

```mysql
递归写法：

function rec($n){
    if($n==0 || $n==1)  return 1;
    return rec($n-1)+rec($n-2);
}

function arr($n){
    $num = [];
    for ($i=0; $i <$n ; $i++) {
        $num[$i] = rec($i);
    }
    print_r($num);
}
arr($n = 35);
```

作者：UFO

原文：https://github.com/lidawei-ufo/MYSQL/blob/master/array/PHP%E5%AE%9E%E7%8E%B0%E6%96%90%E6%B3%A2%E9%82%A3%E5%A5%91%E6%95%B0%E5%88%97.md

版权声明：本文为博主原创文章，转载请附上博文链接！

