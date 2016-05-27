---
layout: post
title: "Java Useful Materials"
category: programming
tags: [java, code]
use_math: true
---

### Java Useful Materials
#### Hello World
```java
public class HelloWorld 
{
    public static void main(String[] args) 
    {
        // Prints "Hello, World" to the terminal window.
        System.out.println("Hello, World");
    }
}
```

1. Save the file as `HelloWorld.java`, note that the file name must be the same as the class name.
2. Compile it: `javac HelloWorld.java`
3. Run it: `java HelloWorld`, note here there is no extension for the 2nd parameter.

#### A More Complex Example
This example is from the LeetCode problem [Edit Distance](http://www.lintcode.com/en/problem/edit-distance/). See how to write it in Java.

Note: 

1. `boolean` and `int` cannot be directly cast. So we use `int x = (express) ? 0 : 1`;
2. 2D array is like `int [][] dp = new int [m][n]`;
3. If the method is not static, we must initialize a object to call it.

```java
public class hw
{
    private static int min3(int a, int b, int c)
    {
        return Math.min(a, Math.min(b, c));
    }
    public int minDistance(String word1, String word2) 
    {
        int m = word1.length(), n = word2.length();
        int [][] dp = new int [m + 1][n + 1];
        for (int i = 0; i <= n; i++)
            dp[0][i] = i;
        for (int i = 0; i <= m; i++)
            dp[i][0] = i;
        for (int i = 1; i <= m; i++)
        {
            for (int j = 1; j <= n; j++)
            {
                int flag = word1.charAt(i - 1) == word2.charAt(j - 1) ? 0 : 1;
                dp[i][j] = min3(dp[i - 1][j] + 1,
                    dp[i][j - 1] + 1,
                    dp[i - 1][j - 1] + flag);
            }
        }
        return dp[m][n];
    }

    public static void main(String[] args)
    {
        String a = "mart";
        String b = "karma";
        hw obj = new hw();
        int x = obj.minDistance(a, b);
        System.out.println(x);
    } 
}
```

#### File I/O
```java
import java.io.*;
import java.util.*;

public class hw
{
    public static void main(String[] args) throws FileNotFoundException
    {
        String filename0 = "input.txt";
        String filename1 = "output.txt";
        File file0 = new File(filename0);
        File file1 = new File(filename1);
        Scanner sc = new Scanner(file0);
        PrintWriter pr = new PrintWriter(file1);
        while (sc.hasNextLine())
        {
            String line = sc.nextLine();
            System.out.println(line);
            pr.write(line);
            pr.write('\n');
        }
        sc.close();
        pr.close();
    } 
}
```

