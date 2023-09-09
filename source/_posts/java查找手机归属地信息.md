---
title: java查找手机归属地信息
tags: java
categories: []
date: 2020-03-06 15:08:30
---

<!--more-->

有时候我们需要知道一个手机号码的信息，包括其归属地等等。

方法一：当然可以网上搜，有很多网页可以实现如：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200306145353698.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzI3MjI1,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200306145402782.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzI3MjI1,size_16,color_FFFFFF,t_70)


但是如和用程序实现以上呢
方法二
用java的httpclient
以下是接受字符串电话号码，结果-》返回字符串数组，的方法

首先如果要使用httpclient需要下载架包
使用maven项目
如何使项目转换为maven项目可以百度
[这里是我参考的一个链接](https://jingyan.baidu.com/article/c1a3101e06c44fde656debcf.html)

需要配置pom.xml文件

```cpp
 <dependency>
                <groupId>org.jsoup</groupId>
                <artifactId>jsoup</artifactId>
                <version>1.11.3</version>
            </dependency>

            <!-- https://mvnrepository.com/artifact/commons-httpclient/commons-httpclient -->
            <dependency>
                <groupId>commons-httpclient</groupId>
                <artifactId>commons-httpclient</artifactId>
                <version>3.1</version>
            </dependency>
```






以下是方法


```cpp
private static String [] test1(String ipid) throws IOException {
        String []res=new String[5];

        //1.打开浏览器
        CloseableHttpClient httpClient = HttpClients.createDefault();
        //2.声明get请求
        HttpPost httpPost = new HttpPost("http://www.ip138.com:8080/search.asp");
        //3.开源中国为了安全，防止恶意攻击，在post请求中都限制了浏览器才能访问
        //httpPost.addHeader("User-Agent","Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36");
        httpPost.addHeader("User-Agent","Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.25 Safari/537.36");
        //4.判断状态码
        // User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.25 Safari/537.36 Core/1.70.3741.400 QQBrowser/10.5.3863.400
        List<NameValuePair> parameters = new ArrayList<NameValuePair>(0);
        parameters.add(new BasicNameValuePair("mobile", ipid));
        parameters.add(new BasicNameValuePair("action", "mobile"));

        UrlEncodedFormEntity formEntity = new UrlEncodedFormEntity(parameters,"UTF-8");

        httpPost.setEntity(formEntity);

        //5.发送请求
        CloseableHttpResponse response = httpClient.execute(httpPost);

        if(response.getStatusLine().getStatusCode()==200){
            HttpEntity entity = response.getEntity();
            String string = EntityUtils.toString(entity, "gbk");
            int start=string.lastIndexOf("<TABLE");
            int end=string.lastIndexOf("</TABLE>");
            String a=string.substring(start,end+9);

            int l=a.indexOf(new String("市"));
            String city=a.substring(l-10,l+1);
            String regEx="[&nbsp]";
            city=city.replaceAll(regEx,"");
            regEx="[;]";
            city=city.replaceAll(regEx,"省 ");

            int l2=a.lastIndexOf("邮 编");
            String youbian=a.substring(l2+42,l2+48);


            int l3=a.lastIndexOf("区 号");
            int l31=l2;
            String quhao=a.substring(l3+42,l2);
            regEx="[^(0-9)]";
            quhao= Pattern.compile(regEx).matcher(quhao).replaceAll("").trim();

            int l4=a.lastIndexOf("卡");
            String klx=a.substring(l4-5,l4+1);


            res[0]="查寻的手机号为："+ipid;
            res[1]="号码归属第为： "+city;
            res[2]="卡  类  型："+klx;
            res[3]="区      号："+quhao;
            res[4]="邮      编："+youbian;
        }
        //6.关闭资源
        response.close();
        httpClient.close();
        return  res;
    }
```

以下是源代码：

```cpp


import org.apache.commons.codec.language.bm.Rule;
import org.apache.http.HttpEntity;
import org.apache.http.NameValuePair;
import org.apache.http.client.entity.UrlEncodedFormEntity;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.message.BasicNameValuePair;
import org.apache.http.util.EntityUtils;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;
import java.util.regex.Pattern;

public class Main{
    private static String [] test1(String ipid) throws IOException {
        String []res=new String[5];

        //1.打开浏览器
        CloseableHttpClient httpClient = HttpClients.createDefault();
        //2.声明get请求
        HttpPost httpPost = new HttpPost("http://www.ip138.com:8080/search.asp");
        //3.开源中国为了安全，防止恶意攻击，在post请求中都限制了浏览器才能访问
        //httpPost.addHeader("User-Agent","Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36");
        httpPost.addHeader("User-Agent","Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.25 Safari/537.36");
        //4.判断状态码
        // User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.25 Safari/537.36 Core/1.70.3741.400 QQBrowser/10.5.3863.400
        List<NameValuePair> parameters = new ArrayList<NameValuePair>(0);
        parameters.add(new BasicNameValuePair("mobile", ipid));
        parameters.add(new BasicNameValuePair("action", "mobile"));

        UrlEncodedFormEntity formEntity = new UrlEncodedFormEntity(parameters,"UTF-8");

        httpPost.setEntity(formEntity);

        //5.发送请求
        CloseableHttpResponse response = httpClient.execute(httpPost);

        if(response.getStatusLine().getStatusCode()==200){
            HttpEntity entity = response.getEntity();
            String string = EntityUtils.toString(entity, "gbk");
            int start=string.lastIndexOf("<TABLE");
            int end=string.lastIndexOf("</TABLE>");
            String a=string.substring(start,end+9);

            int l=a.indexOf(new String("市"));
            String city=a.substring(l-10,l+1);
            String regEx="[&nbsp]";
            city=city.replaceAll(regEx,"");
            regEx="[;]";
            city=city.replaceAll(regEx,"省 ");

            int l2=a.lastIndexOf("邮 编");
            String youbian=a.substring(l2+42,l2+48);


            int l3=a.lastIndexOf("区 号");
            int l31=l2;
            String quhao=a.substring(l3+42,l2);
            regEx="[^(0-9)]";
            quhao= Pattern.compile(regEx).matcher(quhao).replaceAll("").trim();

            int l4=a.lastIndexOf("卡");
            String klx=a.substring(l4-5,l4+1);


            res[0]="查寻的手机号为："+ipid;
            res[1]="号码归属第为： "+city;
            res[2]="卡  类  型："+klx;
            res[3]="区      号："+quhao;
            res[4]="邮      编："+youbian;
        }
        //6.关闭资源
        response.close();
        httpClient.close();
        return  res;
    }
    public static void main(String[] args) throws IOException {

        String ipid="";
        System.out.print("请输入你要查找的手机号：");
          Scanner scanner=new Scanner(System.in);
         ipid=scanner.nextLine();
        String []string=test1(ipid);
        int len=string.length;
        for (int i = 0; i < len; i++) {
            System.out.println(string[i]);
        }
    }
}

```
运行结果如下图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200306150629520.jpg)
