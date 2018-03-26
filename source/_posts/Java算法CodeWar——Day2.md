---
title: Java算法CodeWar——Day2
date: 2018-03-18 12:07:42
tags: CodeWar
categories: 算法
---

# Code War

- code war honor 27  
- uestc position 58
 
### quesetion 1

Mr. Scrooge has a sum of money 'P' that wants to invest, and he wants to know how many years 'Y' this sum has to be kept in the bank in order for this sum of money to amount to 'D'.
The sum is kept for 'Y' years in the bank where interest 'I' is paid yearly, and the new sum is re-invested yearly after paying tax 'T'
Note that the principal is not taxed but only the year's accrued interest

      Example:
      Let P be the Principal = 1000.00      
        Let I be the Interest Rate = 0.05      
        Let T be the Tax Rate = 0.18      
        Let D be the Desired Sum = 1100.00
      
      
      After 1st Year -->
        P = 1041.00
      After 2nd Year -->
        P = 1083.86
      After 3rd Year -->
        P = 1128.30
Thus Mr. Scrooge has to wait for 3 years for the initial pricipal to ammount to the desired sum.
Your task is to complete the method provided and return the number of years 'Y' as a whole in order for Mr. Scrooge to get the desired sum.
Assumptions : Assume that Desired Principal 'D' is always greater than the initial principal, however it is best to take into consideration that if the Desired Principal 'D' is equal to Principal 'P' this should return 0 Years.

### translation 1

Scrooge将本金（principal）存入银行，每年的利息和交税计算公式如下：result = princial * （ 1 + interest）- princial*interest*tax
给定目标金额、本金、利率、税率，求出需要几年可以达到这个金额。如果目标金额和本金相等，返回0；

```Java
public class Money {

    public static int calculateYears(double principal, double interest,  double tax, double desired) {
        int years = 0;

        while( principal < desired)
        {
            years++;
            principal = principal * ( 1 + interest ) - principal*interest*tax;
        }
        return years;
    }
}
```

### question 2

Return the number (count) of vowels in the given string.
We will consider a, e, i, o, and u as vowels for this Kata.
The input string will only consist of lower case letters and/or spaces.

### translation 2

返回字符串中的元音字母，元音字母为a、e、i、o、u，输入参数只包含小写字母 空格 和“/”（斜杠）

```Java
public class Vowels {

    public static int getCount(String str) {
        int j = 0;
        char[] key = str.toCharArray();
        for(int i = 0; i < key.length; i++)
        {
            if(key[i] == 'a' | key[i] =='e'| key[i] == 'i' | key[i] == 'o' | key[i] == 'u')
            {
                j++;
            }
        }
        return j;
    }
}
```

### question 3

Part of Series 1/3: This kata is part of a series on the Morse code. After you solve this kata, you may move to the next one.
In this kata you have to write a simple Morse code decoder. While the Morse code is now mostly superceded by voice and digital data communication channels, it still has its use in some applications around the world.
The Morse code encodes every character as a sequence of "dots" and "dashes". For example, the letter A is coded as ·−, letter Q is coded as −−·−, and digit 1 is coded as ·−−−. The Morse code is case-insensitive, traditionally capital letters are used. When the message is written in Morse code, a single space is used to separate the character codes and 3 spaces are used to separate words. For example, the message HEY JUDE in Morse code is ···· · −·−−   ·−−− ··− −·· ·.
NOTE: Extra spaces before or after the code have no meaning and should be ignored.
In addition to letters, digits and some punctuation, there are some special service codes, the most notorious of those is the international distress signal SOS (that was first issued by Titanic), that is coded as ···−−−···. These special codes are treated as single special characters, and usually are transmitted as separate words.
Your task is to implement a function decodeMorse(morseCode), that would take the morse code as input and return a decoded human-readable string.
For example:

    MorseCodeDecoder.decode(".... . -.--   .--- ..- -.. .")
    //should return "HEY JUDE"
The Morse code table is preloaded for you as a dictionary, feel free to use it. In CoffeeScript, C++, JavaScript, PHP, Python, Ruby and TypeScript, the table can be accessed like this: MORSE_CODE['.--'], in Java it is MorseCode.get('.--'), in C# it is MorseCode.Get('.--'), in Haskell the codes are in a Map String String and can be accessed like this: morseCodes ! ".--", in Elixir it is morse_codes variable.
All the test strings would contain valid Morse code, so you may skip checking for errors and exceptions. In C#, tests will fail if the solution code throws an exception, please keep that in mind. This is mostly because otherwise the engine would simply ignore the tests, resulting in a "valid" solution.
Good luck!
After you complete this kata, you may try yourself at Decode the Morse code, advanced.

### translation 3

制作一个摩斯码的解码器
摩斯码用'.'和'-'来表示信息，不区分大小写，一个空格分割字符，三个空格分隔单词，你的任务
是实现一个函数`decodeMorse(morseCode)` 传入摩斯码，返回英文字符串。摩斯码表我们已经预先提供给你了，Java使用方法： `MorseCode.get('.--')`

```Java
/*
    pass the BaseTest but not the ComplexTest 
*/
public class MorseCodeDecoder {
    public static String decode(String morseCode) {
      String s = morseCode.replaceAll("   ", "#").replaceAll(" ", "|");
        char[] key = s.toCharArray();
        String result = "";
        String temp = "";
        String str = "";
        for(int i = 0; i < key.length; i++)
        {
            if(key[i] == '|')
            {
              str += MorseCode.get(temp);
               temp = "";
            }
            else if(key[i] == '#')
            {
               str += MorseCode.get(temp);
                result += str + " ";
                str = "";
                temp = "";
            }
            else{
                temp += key[i];
            }

        }
        str += MorseCode.get(temp);
        result += str;
        return result;
    }
}
```

1. `public String replaceAll(@NotNull String regex, @NotNull String replacement)`将字符串按照正则表达式用参数`replacement`进行替换


### question 4

Implement a method that accepts 3 integer values a, b, c. The method should return true if a triangle can be built with the sides of given length and false in any other case.
(In this case, all triangles must have surface greater than 0 to be accepted).

### translation 4

实现一个方法，输入三角形的三个边长度，如果能构成三角形，则返回true，否则返回false，注：输入值均大于0；

```Java
public class TriangleTester {

    public static boolean isTriangle(int a, int b, int c){
        if( a+b > c & a+c > b & b+c > a )
        {
            return true;
        }
        return false;
    }
}
```

[对应代码链接，欢迎fork和star](https://github.com/lizheng3401/CodeWar)
