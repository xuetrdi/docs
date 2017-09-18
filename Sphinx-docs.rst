=============
Sphinx使用入门
=============
* Sphinx介绍
* Sphinx安装
* Sphinx文档项目创建及配置说明
* reStructuredText常用基本语法
* Sphinx文档项目管理
* 常用编辑器上集成Sphinx以及自动补全
-----------------------------

1. Sphinx介绍
------
  Sphinx是一块文档编辑工具,编辑纯文本文档.官方网站http://sphinx-doc.org.
  Sphinx是用Python编写的工具,用来构建多个以reStructuredText语法编写的文本文件,将它们转换为HTML或者PDF等格式,Sphinx可以将树的各个元素分割成多个文件进行管理.
  Sphinx捆绑了Tex排版系统等扩展,另外也支持单独安装第三方开发的扩展.这些扩展包括流程图,序列图,等图标的植入扩展,UML画图,乐谱绘图,HTML模版变更.
也可以自己开发扩展插件.

2. Sphinx安装
------
 pip install sphinx

3. Sphinx文档项目创建及配置说明
------

* 创建Sphinx项目

切换目录到你想到创建项目的目录下,执行
sphinx-quickstart -q -p Test -a xuetr_di -v 1.0 test
参数含义:
Test : 开发项目的项目名称
-p   : 要为那个项目创建文档项目(devlop project Test)
-a   : 作者
-v   : 版本
test : 创建次文档项目的项目名称
[注] 具体参数可以查询文档 sphinx-quickstart --help

* Sphinx项目的配置文件

执行以上创建命令后会在当前目录下创建test目录及以下目录
配置文件为conf.py
比如配置中文:
language = 'zh_CN'
激活sphinx扩展插件配置
extensions = ['sphinx.ext.pngmath', 'sphinx.ext.todo', 'sphinx.ext.autodoc',]
todo_include_todos = True

4. reStructuredText常用基本语法
--------

5. Sphinx文档项目管理
--------


