# Git常用命令与操作指南

下面是我整理的常用 Git 命令清单。几个专用名词的译名如下。

```Git
Workspace：工作区
Index / Stage：暂存区
Repository：仓库区（或本地仓库）
Remote：远程仓库
```

#### 一、新建代码库

```ruby
# 在当前目录新建一个Git代码库
$ git init
# 新建一个目录，将其初始化为Git代码库
$ git init [project-name]
# 下载一个项目和它的整个代码历史
$ git clone [url]
```