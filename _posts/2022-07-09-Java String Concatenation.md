---
layout: single
title: Java String Concatenation
tags:
  - java
categories:
  - notes
header:
  overlay_filter: "0.5"
  overlay_color: "#000"
  teaser: /assets/2018/04/java2.png
  overlay_image: /assets/2018/04/java2.png
excerpt: Change a web service endpoint.
created: 2022-07-09T22:17:58+07:00
modified: 2022-07-09T22:17:58+07:00
---

> An implementation may choose to perform conversion and concatenation in one step to avoid creating and then discarding an intermediate String object. To increase the performance of repeated string concatenation, a Java compiler may use the StringBuffer class or a similar technique to reduce the number of intermediate String objects that are created by evaluation of an expression.

https://docs.oracle.com/javase/specs/jls/se8/html/jls-15.html#jls-15.18.1

It's advised to use StringBuffer or StringBuilder to concatenate String objects. However, the specification said that a "+" operator is rewritten by the Java Compiler into StringBuilder. 

https://stackoverflow.com/questions/4648607/stringbuilder-stringbuffer-vs-operator
