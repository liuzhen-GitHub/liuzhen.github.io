---
title: JAVA加密解密之凯撒加密算法
tags: [凯撒加密,java,算法]
categories: [加密算法,java]
---
凯撒加密算法简介

凯撒加密(Caesar cipher)是一种简单的消息编码方式：它根据字母表将消息中的每个字母移动常量位k。举个例子如果k等于3，则在编码后的消息中，每个字母都会向前移动3位：a会被替换为d；b会被替换成e；依此类推。字母表末尾将回卷到字母表开头。于是，w会被替换为z，x会被替换为a。

凯撒加密算法实现

``` java
package com.liuzhen;


import org.junit.Test;

/**
 * 凯撒加密
 * Created by liuzhen on 2017/9/25.
 */
public class CaesarUtils {

    public static String encrypt(String str, int k) {
        StringBuilder result = new StringBuilder();
        for (char c : str.toCharArray()) {
            // 如果字符串中的某个字符是小写字母
            if (c >= 'a' && c <= 'z') {
                c += k % 26; // 移动key%26位
                if (c < 'a')
                    c += 26; // 向左超界
                if (c > 'z')
                    c -= 26; // 向右超界
            }
            // 如果字符串中的某个字符是大写字母
            else if (c >= 'A' && c <= 'Z') {
                c += k % 26; // 移动key%26位
                if (c < 'A')
                    c += 26;// 同上
                if (c > 'Z')
                    c -= 26;// 同上
            }
            result.append(c);
        }
        return result.toString();
    }

    public static String decrypt(String str, int k) {
        // 取相反数
        k = 0 - k;
        return encrypt(str, k);
    }

    @Test
    public void encode() {
        String data = "liuzhen";
        int k = 4;
        String result = CaesarUtils.encrypt(data, k);
        System.out.println("加密后：" + result);
        System.out.println("解密后：" + CaesarUtils.decrypt(result, k));
    }
}
```

测试结果：
加密后：pmydlir
解密后：liuzhen