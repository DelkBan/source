---
title: 填充：setfill和setw方法
date: 2018-09-25 19:54:11
tags:
- C++
- 输出格式
---

setfill()和setw()一起配合使用，可以在输出的内容中进行填充。

setfill括号内填用来填充的字符，setw用来设置总字符数。

setw后面紧跟的内容有效。

setw是设置输出宽度，默认用空格填充。<!--more-->

` cout << setfill('*') << setw(7) << 'A' << 'B' << endl;`

将会输出`*****AB`

代码改为 

` cout << setfill('*') << setw(7) << 'A' << 'B' << << 'C'<< endl;`

将会输出`****ABC`

如果在中间加上`left`，则setw后面的内容先左对齐，然后用setfill的内容补齐。

` cout << setfill('*') << left << setw(7) << 'A' << 'B' << endl;`

输出 `A*****B`

` cout << 'A' << setfill('*') << left << setw(7) << 'B' << endl;

输出`AB*****`

**注意：需 `#include <iomanip>`**