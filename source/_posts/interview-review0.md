---
title: 【面试复盘】依图-算法&一面
swiper_index: 1
top_group_index: 1
background: '#fff'
date: 2023-12-23 17:38:44
updated:
tags: [面试, 算法]
categories:
keywords:
description:
top_img:
comments:
cover: https://s2.loli.net/2024/01/02/FhG7ixONVqDuQpj.png
toc:
toc_number:
toc_style_simple:
copyright:
copyright_author:
copyright_author_href:
copyright_url:
copyright_info:
mathjax:
katex:
aplayer:
highlight_shrink:
aside:
---
## 算法

1.牛牛正在买书，每本书都有名字和价格，牛牛想把书按照价格升序排序

**输入描述**
> 第一行输入一个正整数 n ，表示书的数量。后续每行都输入一个字符串 str 和一个正整数 p 表示书价格。
>

**输出描述**
> 把书名按照价格升序输出。
>

> 输入
>> 3
>> The Nowcoder 100
>> Abook 20
>> BBook 300
>
> 输出
>> Abook
>> TheNowcoder
>> BBook

```Java
import java.util.Scanner;
import java.util.List;
import java.util.ArrayList;
import java.util.Comparator;

class Book {
    String name;
    int price;

    public Book(String name, int price) {
        this.name = name;
        this.price = price;
    }
}

public class BookSorter {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int numberOfBooks = scanner.nextInt();
        scanner.nextLine(); // Read the rest of the line after the integer
        
        List<Book> books = new ArrayList<>();
        
        for (int i = 0; i < numberOfBooks; i++) {
            String bookInfo = scanner.nextLine();
            String[] parts = bookInfo.split(" ");
            String name = parts[0];
            int price = Integer.parseInt(parts[parts.length - 1]);
            books.add(new Book(name, price));
        }
        
        books.sort(Comparator.comparingInt(book -> book.price));
        
        for (Book book : books) {
            System.out.println(book.name);
        }
        
        scanner.close();
    }
}
```

2.牛牛在研究他自己独创的平衡数，平衡数的定义是：将一个数分成左右两部分，分别成为两个新的数。
左右部分必须满足以下两点：
1.左边和右边至少存在一位
2.左边的数每一位相乘如果等于右边的数每一位相乘，则这个数称为平衡数
例如：1221这个数，分成12和21的话，1*2=2*1，则称1221为平衡数，再例如：1236这个数，可以分成123和1*2*3=6，所以1236也是平衡数。而1234无论怎样分也不满足平衡数

**输入描述**
> 输入一个正整数（int范围内）。

**输出描述**
> 如果该数是平衡数，输出 "YES", 否则输出 "NO"。 
>

> 输入
>> 1221
>
> 输出
>> YES
>
> 输入
>> 1236
>
> 输出
>> YES
>
> 输入
>> 1234
>
> 输出
>> NO
```Java
import java.util.Scanner;

public class BalancedNumber {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String input = scanner.next();
        scanner.close();

        if (isBalancedNumber(input)) {
            System.out.println("YES");
        } else {
            System.out.println("NO");
        }
    }

    private static boolean isBalancedNumber(String number) {
        for (int i = 1; i < number.length(); i++) {
            String leftPart = number.substring(0, i);
            String rightPart = number.substring(i);

            if (multiplyDigits(leftPart) == multiplyDigits(rightPart)) {
                return true;
            }
        }
        return false;
    }

    private static int multiplyDigits(String part) {
        int product = 1;
        for (char digit : part.toCharArray()) {
            product *= digit - '0';
        }
        return product;
    }
}
```

3.有n个学生站成一排，每个学生有一个能力值a<sub>i</sub>，牛牛想从这n个学生中按照顺序选取k名学生，要求相邻两个学生的位置的差不超过d。牛牛想要使得这k个学生的能力值乘积最大，你能返回最大的乘积吗？

**输入描述**
> 每个输入包含 1 个测试用例。每个测试数据的第一行包含一个整数 n (1 <= n <= 50)，表示学生的个数，接下来的一行，包含 n 个整数，按顺序表示每个学生的能力值 a<sub>i</sub>（-50 <= a<sub>i</sub> <= 50）。接下来的一行包含两个整数，k 和 d (1 <= k <= 10, 1 <= d <= 50)。
>

**输出描述**
> 输出一行表示最大的乘积。
>

**示例1**
> 输入
>> 3 2 3
>> 7 4 7
>
> 输出
>> 49

```Java
import java.util.*;
 
public class Main{
    
    public static void main(String[] args){
        
        //获取学生个数
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        
        int[] arr = new int[n + 1];
        //获取能力值
        for(int i = 1;i <= n; i++) {
            arr[i] = scanner.nextInt();
        }
        //获取k值
        int k = scanner.nextInt();
        //获取d值
        int d = scanner.nextInt();
        //创建最大值和最小值两个辅助数组，long数组存放数值，范围更大
        long[][] f = new long[n + 1][k + 1]; 
        long[][] g = new long[n + 1][k + 1];
        //初始化两个数组，即K=1的情况
        for(int i = 1; i <= n; i++) {
            f[i][1] = arr[i];
            g[i][1] = arr[i];
        }
        //从k = 2开始填充（遍历每一行）
        for(int curK = 2; curK <= k; curK++) {
            for(int curN = curK; curN <= n; curN++) {  //遍历每一列
                long tempMax = Long.MIN_VALUE;         //临时最大、最小值变量
                long tempMin = Long.MAX_VALUE;
                //根据left的两个边界条件进行遍历
                for(int left = Math.max(curK - 1, curN - d); left <= curN - 1; left++){
                    //根据所得的递推公式更新最小值 最大值
                    if(tempMax < Math.max(f[left][curK - 1] * arr[curN], g[left][curK - 1] * arr[curN])) {
                        tempMax = Math.max(f[left][curK - 1] * arr[curN], g[left][curK - 1] * arr[curN]);
                    }
                    
                    if(tempMin > Math.min(f[left][curK - 1] * arr[curN], g[left][curK - 1] * arr[curN])) {
                        tempMin = Math.min(f[left][curK - 1] * arr[curN], g[left][curK - 1] * arr[curN]);
                    }
                }
                //更新最大值
                f[curN][curK] = tempMax;
                //更新最小值
                g[curN][curK] = tempMin;
            }
        }
        //确定了K值，要得到最大值，则遍历第K列，即搜索f[curN~n][K]  （curN >= k）
        long maxResult = Long.MIN_VALUE;
        for(int curN = k; curN <= n; curN++) {
            if(f[curN][k] > maxResult)
                maxResult = f[curN][k];
        }
        System.out.println(maxResult);
    }
}
```

4.帅帅经常跟同学玩一个矩阵取数游戏：对于一个给定的n * m的矩阵，矩阵中的每个元素a<sub>ij</sub>均为非负整数。游戏规则如下：
1.每次取数时必须从每行各取走一个元素，共n个。m次后取完举证所有元素
2.每次取走的各个元素只能是该元素所在行的行首或行尾
3.每次取数都有一个得分值，为每行取数的得分之和，每行取数的得分=被取走的元素值 * 2<sup>i</sup>，其中i表示第i次取数（从1开始编号）
4.游戏结束总得分为m次取数得分之和
帅帅想请你帮忙写一个程序，对于任意矩阵，可以求出取数后的最大得分。

**输入描述**
> 第一行输入两个正整数 n 和 m ，表示矩阵的长宽。后续 n 行每行输入 m 个正整数，表示矩阵的元素
>

**输出描述**
> 输出最大得分
>

**示例1**
> 输入
>> 2 3
>> 1 2 3
>> 3 4 2
>
> 输出
>> 82
>
> 说明：
>> 第1次：第1行取行首元素，第2行取行尾元素，本次得分为1 * 21 + 2 * 21 = 6
>> 第2次：两行均取行首元素，本次得分为2 * 22 + 3 * 22 = 20
>> 第3次：得分为3 * 23 + 4 * 23 = 56。
>> 总得分为6 + 20 + 56 = 82
>

**示例2**
> 输入
>> 1 4
>> 4 5 0 5
>
> 输出
>> 122

**示例3**
> 输入
>> 2 10
>> 96 56 54 46 86 12 23 88 80 43
>> 16 95 18 29 30 53 88 83 64 67
>
> 输出
>> 316994

```Java
import java.util.Scanner;

public class Main {
    private static int[][] dp;
    private static int[][] a;
    private static int n, m;
    private static int row;

    private static int f(int i, int j) {
        if (dp[i][j] != 0) {
            return dp[i][j];
        }
        if (i == j) {
            return dp[i][j] = a[row][i] * m;
        } else {
            return dp[i][j] = Math.max(f(i + 1, j) + (m - j + i) * a[row][i], f(i, j - 1) + (m - j + i) * a[row][j]);
        }
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        n = scanner.nextInt();
        m = scanner.nextInt();
        a = new int[n][m];
        dp = new int[m][m];
        int ans = 0;

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                a[i][j] = scanner.nextInt();
            }
        }

        for (row = 0; row < n; row++) {
            for (int i = 0; i < m; i++) {
                for (int j = 0; j < m; j++) {
                    dp[i][j] = 0;
                }
            }
            ans += f(0, m - 1);
        }

        System.out.println(ans);
        scanner.close();
    }
}
```

## 八股

### JVM
* 调优参数？堆内存设置？为什么有时初始大小和最大设置成一样？
    * 堆内存设置（-Xms, -Xmx），初始堆内存和最大堆内存设置相同可避免频繁扩容引发的性能波动。
* 垃圾回收算法：

    * 标记-清除

    * 标记-整理

    * 复制算法

    * 分代回收
* 垃圾收集器？G1？


    * CMS (并发标记清除)

    * G1 (垃圾优先)：适合大堆内存、高吞吐量的应用场景。

### Redis
* redis 数据淘汰策略？

    * LRU (最近最少使用)

    * LFU (最少频率使用)

    * FIFO (先进先出)

    * 随机淘汰
* redis 持久化方法？
  **RDB(Redis Database)** 持久化 RDB 持久化会在指定的时间间隔内生成数据集的时间点快照。

  **AOF(Append Only File)** 持久化 AOF 持久化记录每次写操作的日志，这些写指令将被追加到 AOF 文件的末尾。Redis 重启的时候会通过 AOF 文件重构数据。
  
* redis 分布式锁？
    * 基于 setnx 和超时机制，避免死锁。
* redis 哨兵？
    * 高可用性方案，自动检测和恢复主节点故障。
* redis Cluster 哈希槽？
    * 哈希槽机制实现分布式存储。
    * 分片技术提升扩展性。
* redis 分片？
    * 哈希槽机制实现分布式存储。
    * 分片技术提升扩展性。
* 缓存穿透和缓存雪崩？解决方法？布隆过滤器？
    * 缓存穿透：布隆过滤器提前过滤不存在数据。
    * 缓存雪崩：随机化缓存过期时间，避免同时失效。

### Elasticsearch
* 倒排索引组成？
    * 词项词典
    * 倒排列表
    * 文档频率
* 分词器？中英文用什么？
    * 中文：ik分词器
    * 英文：standard分析器
* 分片？自定义路由优化？
    * 分片提升读写性能，合理设置主分片和副本数。
    * 自定义路由优化数据分布，提升查询效率。