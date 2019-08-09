---
title: Java-IO
date: 2019-08-05 23:39:43
tags: Java
---

使用BufferReader记得flush()刷新缓存区，BufferedWriter是缓冲输入流，意思是调用BufferedWriter的write方法时候。数据是先写入到缓冲区里，并没有直接写入到目的文件里。必须调用BufferedWriter的flush()方法。这个方法会刷新一下该缓冲流，也就是会把数据写入到目的文件里。或者你可以调用BufferedWriter的close()方法，该方法会在关闭该输入流之前先刷新一下该缓冲流。也会把数据写入到目的文件里。如果没有在里面的for()循环中添加 bw.flush(); 这句话，在if 的时候重新new  BufferedWriter(); 就把原来bw（缓冲区）中的覆盖掉了。于是就不能写进文件字符。