# Windows 原生tree命令 
```
TREE [drive:][path] [/F][/A]
/F 显示每个文件夹中文件的名称
/A 使用ASCII 字符，而不使用扩展字符
示例：
D:\website
$ tree > text.md  // 在website目录下生成text.md 树文件
```

# 基于node的tree-cli 和treer 

这两个包都提供了不同的命令，可以配置不同的参数，按照自身的需求生成所需的文件