---
title: 【面试复盘】依图-算法&一面
swiper_index: 1
top_group_index: 1
background: '#fff'
date: 2023-12-23 17:38:44
updated:
tags:
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
> 输入
>> 3 2 3
>> 7 4 7
>
> 输出
>> 49