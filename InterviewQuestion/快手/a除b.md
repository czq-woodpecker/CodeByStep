## a/b

题目描述

> 求a/b的小数表现形式。如果a可以整除b则不需要小数点。如果是有限小数，则可以直接输出。如果是无限循环小数，则需要把小数循环的部分用"()"括起来。

输入

> 两个整数 a 和 b，其中  
> 0 ≤ a ≤ 1 000 000;   
> 1 ≤ b ≤ 10 000

输出

> 一个字符串，该分数的小数表现形式

输入样例 1 

```
10 1
```

输出样例 1

```
10
说明 10/1 = 10
```

输入样例 2 

```
1 2
```

输出样例 2

```
0.5
说明  1/2 = 0.5
```

输入样例 3 

```
1 3
```

输出样例 3

```
0.(3)
说明 1/3=0.333333.....
```

输入样例 4 

```
1 6
```

输出样例 4

```
0.1(6)
说明：1/6=0.1666666.....
```

输入样例 5 

```
1 7
```

输出样例 5

```
0.(142857)
说明 1/7 = 0.1428571428.......
```



## AC代码

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        int a, b;
        a = input.nextInt();
        b = input.nextInt();
        //保存
        int[] num = new int[2000];
        // 保存每次运算后的余数
        int[] digit = new int[2000];
        int a1 = a;
        //临时的余数
        int tempD = 0;
        num[0] = a / b;
        tempD = a % b;
        a = a % b * 10;
        if (a1 == 0) {
            System.out.println(0);
        } else if (a1 == b) {
            System.out.println(1);
        } else if (a1 > b && a1 % b == 0) {
            System.out.print(num[0]);
        } else {
            System.out.print(num[0] + ".");
        }
        int k = 0;
        //当余数再次出现时，说明循环节出现
        while (a != 0 && digit[tempD] == 0) {
            num[++k] = a / b;
            digit[tempD] = k;
            tempD = a % b;
            a = a % b * 10;
        }

        if (a == 0) {
            for (int i = 1; i <= k; i++)
                System.out.print(num[i]);
        } else {
            for (int i = 1; i < digit[tempD]; i++) {
                System.out.print(num[i]);
            }                                //输出循环节
            System.out.print("(");
            for (int i = digit[tempD]; i <= k && i < digit[tempD] + 50; i++) {
                System.out.print(num[i]);
            }                                //判断循环节的长度
            if (k - digit[tempD] + 1 > 50) System.out.print("...");
            System.out.println(")");
        }
    }
}
```