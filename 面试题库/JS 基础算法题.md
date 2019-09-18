https://www.cnblogs.com/mr-wuxiansheng/p/6534910.html
https://www.jianshu.com/p/f0b69781e556


1、不需要借助第三个临时变量，实现两个变量的交换.
```
function swap(a,b){
    b = b - a;
    a = a + b;
    b = a - b;

    return [a,b]
}
swap(10,20);
swap(20,10);
```

2、确保字符串的每个单词首字母都大写，其余部分小写.
```
function titleCase(str){
    var lstr = str.toLowerCase().split(' ');
    for(var i = 0; i< lstr.length; i++){
        lstr[i] = lstr[i][0].toUpperCase() + lstr[i].substring(1,lstr[i].length);
    }
    var res = lstr.join(' ');
    return res;
}

titleCase("good night");
```

3、找出正整数 数组的最大差值.
```
function getMaxPro(arr){
    var min = arr[0];
    var max = 0;

    for(var i=0;i<arr.lenght;i++){
        var current = arr[i];
        min = Math.min(min,current);
        var res = current - min;
        max = Math.max(max,res);
    }

    return max;
}
```

4、清除字符串前后的空格（兼容所有浏览器）
```
function trim(str){
    if(str & typeof str === "string"){
        return str.replace(/(^s*)|(s*)$/g,'');  // 去除前后空白符号.
    }
}
```

5、去掉一组整型数组中重复的值.





