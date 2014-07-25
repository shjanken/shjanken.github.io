---
    layout: post
    title: grub相关知识
---

1. `tee` 命令的使用: `tee` 命令可以接受一个`stdin` 并将该 `stdin` 保存到一个副本中， 然后将该 `stdin` 作为 自己（`tee`命令）的 `stdout` 输出

        
        # 接受前一个命令的输出放入文件，并将该输出继续传入下一个命令的stdin
        echo 'hello world' | tee out.txt | cat -n -
        #显示 1 hello world，并且在out.txt文件中记录了'hello world'

2. 数组：
    
    1. 普通数组：
        - 定义：`array=(val1 val2 val3 ..)`
        - 获取元素：`echo ${array[$index]}`
        - 打印所有元素: `{array[*]}`
        - 打印元素个数： `echo ${#array[*]}`
    
    2. 关联数组：
        - 声明必须是显示定义： `declare -A array`
        - 赋值： `array=([index1]=val1 [index2]=val2)`
        - 获取元素： `echo ${array[index1]}
        - 打印所有元素： `echo ${!array[*]}`

3. `find` 命令的用法：
    
        find path -print

    1. 参数：
        
        - `name`: 查找的名字，可以用通用匹配符
        - `iname`: 和`name`类似，不过可以忽略大小写
        - `OR` 条件： `find . \( -name "*.txt" -o -name "*.pdf" \) -print`
        - `path` : 匹配文件名或路径名
        - `regex`: 匹配正则表达式
        - `！` 否定表达式： `find . ! -name "*.txt" -print`
        - `perm` : 匹配权限。 如 `find . -perm 644 -print`
        - `exec`: 对匹配的文件执行命令。
                
                find . -type f -name "*.c" -exec cat {} \
                // ‘{}’ 在命令中替换所有匹配文件的文件
