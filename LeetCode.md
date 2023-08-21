# LeetCode

## 2023-3-21 

#### [2. 两数相加](https://leetcode.cn/problems/add-two-numbers/)

难度中等9400

操作链表题

思路：相加取余得到填入的值，取模得到需要进位的值，这里需要的是相加后倒置，进位的值在，下一位增加即可

![image-20230321151121656](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230321151121656.png)



## 2023.3.22

#### [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

思路：这里主要是完成使用IndexOF（）查重，找出数组可以存在的最大的没有重复的串

splice函数

![image-20230322204720199](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230322204720199.png)



## 2023.3.23

#### [5. 最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/)

思路：主要使用双指针，由中间向两边查找，对比找到符合回文的最长字符串。

主要有三个问题需要注意，1.解决字符串存在奇数偶数的问题

​					2.双指针向两边走的判断条件

​					3.拿到回文符时需要注意 while多执行了一次需要+1还原

![image-20230323233255321](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230323233255321.png)

## 2023.3.25

#### [11. 盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/)

思路：利用双指针由外向内走，找出最大的容量并保存下来

![image-20230325150200507](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230325150200507.png)



#### [17. 电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)

思路：

1.DFS深度优先算法：按照从ad ae af bd ...的方式走

<img src="https://olrando.oss-cn-chengdu.aliyuncs.com/img/IMG_20230325_201801.jpg" alt="IMG_20230325_201801" style="zoom: 25%;" />

2.BFS广度优先算法



DFS:

![image-20230325201250365](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230325201250365.png)

BFS:

## 2023.3.26

#### [19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

![image-20230327004434810](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230327004434810.png)

思路：1.遍历求出链表的总长度

​	2.设置正向删除的位置

​	3.区别是否是正向第一个位置，如果是第一个位置直接返回即可

​	4.不是，则循环得到如图的元素，并将其处理跳过需要删除的元素即可

​	注意点：这里在最开始需要赋头节点，后续操作不使用给出的头节点进行操作，最后的时候返回头节点即可

## 2023.3.27

#### [21. 合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)

思路：利用递归的思想，这里对于第三个if else 中的流程还存在疑惑

![image-20230328004935921](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230328004935921.png)
