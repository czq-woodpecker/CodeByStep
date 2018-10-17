## 幸运ID

题目描述：

> 小C有一张票，这张票的ID是长度为6的字符串，每个字符都是数字，他想让这个ID变成他的幸运ID，所以他就开始更改ID，每一次操作，他可以选择任意一个数字并且替换它。
> 如果这个ID的前三位数字之和等于后三位数字之和，那么这个ID就是幸运的。你帮小C求一下，最少需要操作几次，能使ID变成幸运ID

输入
> 输入只有一行，是一个长度为6的字符串。

输出
> 输出这个最小操作次数

样例输入
> 000000

样例输出
> 0


输入样例2
> 000018

输出样例2
> 1

样例解释：
> 将前三位任意一个改为9即可满足条件，操作数为1

## AC代码
```java
import java.util.Scanner;
public class Main {
    public static void main(String[] args){
        Scanner in = new Scanner(System.in);
        String input = in.nextLine();
        if (input.length() != 6) {
            System.out.println(0);
            return;
        }
        int[] arrP = new int[3];
        int[] arrN = new int[3];
        for (int i = 0; i < 3; i++) {
            arrP[i] = Integer.valueOf(input.substring(i, i + 1));
            arrN[i] = Integer.valueOf(input.substring(i + 3, i + 4));
        }
        int per = arrP[0] + arrP[1] + arrP[2];
        int next = arrN[0] + arrN[1] + arrN[2];
        if (per == next) {
            System.out.println(0);
            return;
        }
        for (int i = 0; i < 3; i++) {
            for (int j = i; j < 3; j++) {
                if (arrP[i] > arrP[j]) {
                    int temp = arrP[i];
                    arrP[i] = arrP[j];
                    arrP[j] = temp;
                }
                if (arrN[i] > arrN[j]) {
                    int temp = arrN[i];
                    arrN[i] = arrN[j];
                    arrN[j] = temp;
                }
            }
        }
        int minF = 0;
        int minS = 0;
        if (arrN[0] < arrP[0]) {
            minF = arrN[0];
            minS = Math.min(arrP[0], arrN[1]);
        } else {
            minF = arrP[0];
            minS = Math.min(arrP[1], arrN[0]);
        }
        int maxF = 0;
        int maxS = 0;
        if (arrN[2] > arrP[2]) {
            maxF = arrN[2];
            maxS = Math.max(arrN[1], arrP[2]);
        } else {
            maxF = arrP[2];
            maxS = Math.max(arrP[1], arrN[2]);
        }
        int poor = Math.abs(per - next);
        int poorF = 0;
        int poorS = 0;
        if (9 - minF > maxF) {
            poorF = 9 - minF;
            poorS = Math.max(9 - minS, maxF);
        } else {
            poorF = maxF;
            poorS = Math.max(9 - minF, maxS);
        }
        if (poor <= poorF) {
            System.out.println(1);
            return;
        }
        if (poor <= poorF + poorS) {
            System.out.println(2);
            return;
        }
        System.out.println(3);
    }
}
```