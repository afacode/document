### 类型强制转换
> string强制转换为数字

可以用*1来转化为数字(实际上是调用.valueOf方法)
然后使用Number.isNaN来判断是否为NaN，或者使用 a !== a 来判断是否为NaN，因为 NaN !== NaN
```js
'32' * 1            // 32
'ds' * 1            // NaN
null * 1            // 0
undefined * 1    // NaN
1  * { valueOf: ()=>'3' }        // 3

// 常用：也可以使用+来转化字符串为数字

+ '123'            // 123
+ 'ds'               // NaN
+ ''                    // 0
+ null              // 0
+ undefined    // NaN
+ { valueOf: ()=>'3' }    // 3
```
> 使用Boolean过滤数组中的所有假值
```js
const compact = arr => arr.filter(Boolean)
compact([0, 1, false, 2, '', 3, 'a', 'e' * 23, NaN, 's', 34]) // [ 1, 2, 3, 'a', 's', 34 ]
```
> 取整 | 0
```js
1.3 | 0         // 1
-1.9 | 0        // -1
```
> 判断奇偶数 & 1
```js
const num=3;
!!(num & 1)                    // true
!!(num % 2)                    // true
```



































