---
title: Java算法CodeWar——Day1
date: 2018-03-18 12:06:04
tags: CodeWar
categories: 算法
---
# Code War

- code war honor 27  
- uestc position 58
 
### quesetion 1

Jaden Smith, the son of Will Smith, is the star of films such as The Karate Kid (2010) and After Earth (2013). Jaden is also known for some of his philosophy that he delivers via Twitter. When writing on Twitter, he is known for almost always capitalizing every word.

Your task is to convert strings to how they would be written by Jaden Smith. The strings are actual quotes from Jaden Smith, but they are not capitalized in the same way he originally typed them.

Example:

Not Jaden-Cased: "How can mirrors be real if our eyes aren't real"
Jaden-Cased:     "How Can Mirrors Be Real If Our Eyes Aren't Real"
Note that the Java version expects a return value of null for an empty string or null.

### translation 1

给一个初始的字符串，对于该字符串的每个单词的首字母转换为大写并返回，当传入字符串为空字符串(即"")或者传入参数为null时，返回null。

```Java
public class JadenCase {

  public String toJadenCase(String phrase) {
    if(phrase == null || phrase.length() == 0 )
    {
        return null;
    }

    char[] str = phrase.toCharArray();
    str[0] = Character.toUpperCase(str[0]);
    for (int i = 0; i < str.length ; i++ )
    {
        if(str[i] == ' ')
        {
            str[i+1] = Character.toUpperCase(str[i+1]);
        }
    }
    return new String(str);
  }
}
```

1. 在判断参数类型时，如果需要判断传入的参数是否为null， 则必须首先判断是否为null，再判断其他条件，反例： phrase.length() == 0 || phrase == null，在判断这个条件时，如果phrase为null，这是首先判断其长度，就会出错。
1. 字符串转化为字符串数组时，用str.CharArray();
1. 字符大小写转化：Character.toUpperCase(char c)、 Character.toLowerCase(char c)
1. 字符数组转换为字符串 new String(char c[])

### question 2

In a small town the population is p0 = 1000 at the beginning of a year. The population regularly increases by 2 percent per year and moreover 50 new inhabitants per year come to live in the town. How many years does the town need to see its population greater or equal to p = 1200 inhabitants?

    At the end of the first year there will be:
    1000 + 1000 * 0.02 + 50 => 1070 inhabitants

    At the end of the 2nd year there will be:
    1070 + 1070 * 0.02 + 50 => 1141 inhabitants (number of inhabitants is an integer)

    At the end of the 3rd year there will be:
    1141 + 1141 * 0.02 + 50 => 1213

    It will need 3 entire years.
More generally given parameters:

p0, percent, aug (inhabitants coming or leaving each year), p (population to surpass)

the function nb_year should return n number of entire years needed to get a population greater or equal to p.

aug is an integer, percent a positive or null number, p0 and p are positive integers (> 0)

Examples:
nb_year(1500, 5, 100, 5000) -> 15
nb_year(1500000, 2.5, 10000, 2000000) -> 10
Note: Don't forget to convert the percent parameter as a percentage in the body of your function: if the parameter percent is 2 you have to convert it to 0.02.

### translation 2

某个区域的人口初始为p0，每年以 百分之percent的速度增加，另外每年还会迁移过来aug个人,编程实现函数，返回几年后人口会达到p。

```Java
class Arge {
    
    public static int nbYear(int p0, double percent, int aug, int p) {
        int sum;
        percent = percent*0.01;
        for(int i = 1; ; i++ )
        {
            sum = (int)(p0 + p0 * percent + aug);
            if( sum >= p)
            {
                return i;
            }
            p0 = sum;
        }
    }
}
```

1. 强制转换 `(int)float a`;

### question 3

Write a function, persistence, that takes in a positive parameter num and returns its multiplicative persistence, which is the number of times you must multiply the digits in num until you reach a single digit.

For example:

     persistence(39) == 3 // because 3*9 = 27, 2*7 = 14, 1*4=4
                          // and 4 has only one digit
    
     persistence(999) == 4 // because 9*9*9 = 729, 7*2*9 = 126,
                           // 1*2*6 = 12, and finally 1*2 = 2
    
     persistence(4) == 0 // because 4 is already a one-digit number

### translation 3

实现一个函数persistence, 传入一个正整数参数，把参数的每个数字相乘，得到的积如果不是个位数，那么重复上述操作，继续乘，如果是个位数，那么返回计算的次数（即重复了多少次上述操作）

```Java
class Persist {
  public static int persistence(long n) {
    char str[] = (n+"").toCharArray();
        int time=0;
        while(str.length > 1)
        {
            int temp = 1;
            int[] key = new int[str.length];
            for(int i = 0; i < str.length; i++)
            {
                key[i] = Integer.parseInt(String.valueOf(str[i]));
                temp = key[i] * temp;
            }
            str = (temp+"").toCharArray();
            time++;
            if(temp < 10)
            {
                break;
            }

        }

        return time;
  }
}
```

1. 数字转化为字符串 可使用如下方式 int + "" 
2. Java中数组的创建方法: `int[] arr = new int[length];`
3. 字符转化为数字：`Integer.parseInt(String.valueOf(char c))`
即把'2'转换为数字2，而强制转换`(int)char c;`是把对应字符转化为对应的ascii码值

### question 4

Take 2 strings s1 and s2 including only letters from ato z. Return a new sorted string, the longest possible, containing distinct letters,

each taken only once - coming from s1 or s2. 

Examples:
     a = "xyaabbbccccdefww" b = "xxxxyyyyabklmopq" longest(a, b) -> "abcdefklmopqwxy"
     a = "abcdefghijklmnopqrstuvwxyz" longest(a, a) -> "abcdefghijklmnopqrstuvwxyz" 

### translation 4

给定两个字符串，统计出现的字母并按照字母表的顺序排序

```Java
public class TwoToOne {
    
    public static String longest (String s1, String s2) {
       char[] str = (s1+s2).toCharArray();
        int[] key = new int[26];
        char[] aim = new char[26];
        for(int i = 0; i < str.length; i++)
        {
            key[(int)str[i] - 97] = 1;
        }
        int i = 0;
        for(int j = 0; j < 26; j++)
        {
            if(key[j] == 1)
            {
                aim[i] = (char)(j+97);
                i++;
            }
        }
        char[] re = new char[i];
        System.arraycopy(aim, 0, re, 0,i);
        return new String(re);
    }
}
```  

1. 字符串拼接 `str1 + str2;`
2. 数组复制函数 
`arraycopy(@NotNull Object src, int srcPos,@NotNull Object dest,int destPos,int length)`
参数 src 源数组，srcPos 源数组复制起点，dest 目标数组，destPos 目标数组开始位置，复制长度，

[对应代码链接，欢迎fork和star](https://github.com/lizheng3401/CodeWar)

