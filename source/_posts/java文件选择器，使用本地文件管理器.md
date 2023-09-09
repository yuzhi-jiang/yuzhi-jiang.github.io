---
title: java文件选择器，使用本地文件管理器
categories: []
date: 2020-03-16 11:25:44
tags:
---

<!--more-->


java默认的选择器太丑了


如下选择管理器
```java
if(fileChooser.showOpenDialog(null)==JFileChooser.APPROVE_OPTION){
            pathField.setText(fileChooser.getSelectedFile().getPath());//文本框给出路径
            imgname = fileChooser.getSelectedFile().getName();//获取加密文件名
            imgpath = pathField.getText() ;
            imgpath = imgpath.substring(0,imgpath.length()-imgname.length());//获取加密的文件所在的文件夹
        }
```
增加下面代码
[参考链接如下](https://zhidao.baidu.com/question/425294497293157012.html)
https://zhidao.baidu.com/question/425294497293157012.html
```java
 if(UIManager.getLookAndFeel().isSupportedLookAndFeel()){//使用系统的文件管理器
            final String platform = UIManager.getSystemLookAndFeelClassName();
// If the current Look & Feel does not match the platform Look & Feel,
// change it so it does.
            if (!UIManager.getLookAndFeel().getName().equals(platform)) {
                try {
                    UIManager.setLookAndFeel(platform);
                } catch (Exception exception) {
                    exception.printStackTrace();
                }
            }
        }
```

总代码
```java
//选择图片路径事件
    public void cpathaction(ActionEvent arg0){
        JFileChooser fileChooser=new JFileChooser("C:/Users/Administrator/Desktop/");//默认首先搜寻路径,可根据自己的需要进行修改
        if(UIManager.getLookAndFeel().isSupportedLookAndFeel()){//使用系统的文件管理器
            final String platform = UIManager.getSystemLookAndFeelClassName();
// If the current Look & Feel does not match the platform Look & Feel,
// change it so it does.
            if (!UIManager.getLookAndFeel().getName().equals(platform)) {
                try {
                    UIManager.setLookAndFeel(platform);
                } catch (Exception exception) {
                    exception.printStackTrace();
                }
            }
        }
        if(fileChooser.showOpenDialog(null)==JFileChooser.APPROVE_OPTION){
            pathField.setText(fileChooser.getSelectedFile().getPath());//文本框给出路径
            imgname = fileChooser.getSelectedFile().getName();//获取加密文件名
            imgpath = pathField.getText() ;
            imgpath = imgpath.substring(0,imgpath.length()-	imgname.length());//获取加密的文件所在的文件夹
        }
    }
```

