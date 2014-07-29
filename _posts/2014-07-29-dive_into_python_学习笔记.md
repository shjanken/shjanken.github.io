---
    layout: post
    title: "dive into python 学习笔记"
---

惊异好用函数系列
---

- 输出用户的家目录

    ```python
    import os.path
    os.path.expanduser("~")
    ```

- 使用通配符列出目录

    ```python
    import glob
    glob.glob("/tmp/*.mp3")
    //列出所有的MP3文件
    ```


- 获取对象的方法的引用
        
    ```python
    li = ["larry","Curly"]
    li.pop //获取 li的pop方法的引用. 注意!并不调用该方法
    getattr(li,"pop") //同上
    getattr(li,"append")("Moe") //调用li.append("Moe")
    ````
