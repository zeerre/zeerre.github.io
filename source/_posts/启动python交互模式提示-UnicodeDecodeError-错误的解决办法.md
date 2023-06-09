---
title: 启动python交互模式提示'UnicodeDecodeError'错误的解决办法
date: 2021-11-11 20:16:17
tags:
    - Python
    - 交互模式
    - 编码
categories:
    - BUG 修理铺
---

今天启动 python 交互模式的时候，突然有一大段代码提示信息，似乎和以前有很大区别，仔细一看:

```
    (base) C:\WINDOWS\system32>python
    Python 3.8.12 (default, Oct 12 2021, 03:01:40) [MSC v.1916 64 bit (AMD64)] :: Anaconda, Inc. on win32
    Type "help", "copyright", "credits" or "license" for more information.
    Failed calling sys.__interactivehook__
    Traceback (most recent call last):
    File "C:\Anaconda3\lib\site.py", line 440, in register_readline
        readline.read_history_file(history)
    File "C:\Anaconda3\lib\site-packages\pyreadline\rlmain.py", line 165, in read_history_file
        self.mode._history.read_history_file(filename)
    File "C:\Anaconda3\lib\site-packages\pyreadline\lineeditor\history.py", line 82, in read_history_file
        for line in open(filename, 'r'):
    UnicodeDecodeError: 'gbk' codec can't decode byte 0x89 in position 26: illegal multibyte sequence
```
***UnicodeDecodeError***, Unicode 编码错误。

<!-- more -->

由上面的提示的信息：

```
    ...
    File "C:\Anaconda3\lib\site-packages\pyreadline\lineeditor\history.py", line 82, in read_history_file
        for line in open(filename, 'r'):
    ...
```

利用脚本编辑器打开 **C:\Anaconda3\lib\site-packages\pyreadline\lineeditor\history.py** 文件，找到82行所在的代码片段：

``` 
    def read_history_file(self, filename=None): 
        '''Load a readline history file.'''
        if filename is None:
            filename = self.history_filename
        try:
            for line in open(filename, 'r'):
                self.add_history(lineobj.ReadLineTextBuffer(ensure_unicode(line.rstrip())))
        except IOError:
            self.history = []
            self.history_cursor = 0    

```

将

```
    for line in open(filename,'r'):
```

中 `open(filename,'r')` 修改为： `open(filename,'r',encoding='utf-8')`。

修改后的代码片段为：

```
    def read_history_file(self, filename=None): 
        '''Load a readline history file.'''
        if filename is None:
            filename = self.history_filename
        try:
            for line in open(filename, 'r',encoding='utf-8'):
                self.add_history(lineobj.ReadLineTextBuffer(ensure_unicode(line.rstrip())))
        except IOError:
            self.history = []
            self.history_cursor = 0    

```

然后就没有然后了，正常显示如初了。遇到这个问题的伙伴们，去试试吧。



