# JAVA总结

## 基础

## day2

### 算数运算符

#### 隐式转换

把取值范围小的数值，转成取值范围大的数据

![image-20230308210848612](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230308210848612.png)

bute short char 三种类型的数据在运算的时候，会直接先提升成Int然后再进行运算![image-20230308211032113](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230308211032113.png)

#### 强制转换

把取值范围大的数值，赋值给取值范围小的变量，是不允许直接赋值的。如果一定要这么做就要加入强制转换。![image-20230308211246567](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230308211246567.png)

#### 字符串拼接

单引号会将 字符 转换成对应的阿斯克码

### 自增自减运算符

先++和后++区别：![image-20230308221915605](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230308221915605.png)



### 赋值运算符

### 逻辑运算符

### 三元运算符

### 原码反码补码

#### 原码

![image-20230308222506800](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230308222506800.png)

原码的弊端：

![image-20230308222648092](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230308222648092.png)

#### 反码：

用于负数计算

![image-20230308222705255](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230308222705255.png)

#### 补码

![image-20230308223231010](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230308223231010.png)

补码是为了反码跨0而存在的，补码在反码的基础上+1

-128没有原码和反码，只有补码为10000000

![image-20230308224627790](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230308224627790.png)

#### 其他运算符

![image-20230308224734194](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230308224734194.png)



## day3

### 判断和循环

## day4

### 基础题目训练

## day5

### 数组

#### 动态初始化

```java
// 创建动态数组
int[] arr = new int[50]; 
```

#### 数组常见问题

##### 数组越界

访问超出数组长度

#### 数组的内存图

## day6

### 方法

## day8

### 对象内存图

### this关键字

this:方法调用者的地址值

## day9

### 面向对象案例练习

## day10

 ### ==号比的是什么？

基本数据类型比的是 数据值

引用数据类型比较的是 地址值

### 字符串-StringBuilder

sb.append()

sb.reserve()

sb.length()

sb.toString()

### 字符串-StringJoiner(jdk8以上)

add

length

toString

### 字符串拼接

![image-20230315131002247](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230315131002247.png)

![image-20230315131330464](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230315131330464.png)

如果有中间变量的参加，上者S1是放在串池中，S2是使用的StringBuilder来在堆中新开辟空间存放

![image-20230315132150009](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230315132150009.png)

没有中间变量的参加时，上者在编译的时候就会直接拼接成为abc，会直接放在串池中，所以S1与S2是同一地址

### STringBuilder源码分析

默认创建容量为16的空间

当需要的长度超过容量时，会根据长度创建一个新的容量空间

![image-20230315134202368](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230315134202368.png)

p108-110字符串的练习

## day11

### 集合

![image-20230315134717752](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230315134717752.png)
