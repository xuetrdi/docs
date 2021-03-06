=============
Sphinx使用入门
=============
* Sphinx介绍
* Sphinx安装
* Sphinx文档项目创建及配置说明
* reStructuredText常用基本语法
* Sphinx文档结构化三阶段
* Sphinx自动化生成Python文档
* 常用编辑器上集成Sphinx以及自动补全
-----------------------------

1. Sphinx介绍
-------------

  Sphinx是一块文档编辑工具,编辑纯文本文档.官方网站http://sphinx-doc.org。
  Sphinx是用Python编写的工具,用来构建多个以reStructuredText语法编写的文本文件,将它们转换为HTML或者PDF等格式,Sphinx可以将树的各个元素分割成多个文件进行管理。
  Sphinx捆绑了Tex排版系统等扩展,另外也支持单独安装第三方开发的扩展.这些扩展包括流程图,序列图,等图标的植入扩展,UML画图,乐谱绘图,HTML模版变更.也可以自己开发扩展插件。

2. Sphinx安装
-------------

.. code-block:: shell

    pip install sphinx

3. Sphinx文档项目创建及配置说明
-------------------------------

* 创建Sphinx项目

  切换目录到你想到创建项目的目录下,执行
  .. code-block:: shell
  
      sphinx-quickstart -q -p Test -a xuetr_di -v 1.0 test

  参数含义:

  Test : 开发项目的项目名称

  -p   : 要为那个项目创建文档项目(devlop project Test)

  `-a`   : 作者

  `-v`   : 版本

  test : 创建次文档项目的项目名称

  [注] 具体参数可以查询文档 sphinx-quickstart --help

* Sphinx项目的配置文件

  执行以上创建命令后会在当前目录下创建test目录及以下目录

  配置文件为conf.py

  比如配置中文:
  language = 'zh_CN'

  激活sphinx扩展插件配置
  
  `extensions = ['sphinx.ext.pngmath', 'sphinx.ext.todo', 'sphinx.ext.autodoc',]`

  `todo_include_todos = True`

4. reStructuredText常用基本语法
-------------------------------

+ 目录树指令toctree指令
+ 单个文档结构
  
  - 标题
  - 段落
  - 无序列表:可以使用+，-，\*中任意一个表示一个无序列表的一行这个三个字符和内容之间需要有一个空格
  - 有序列表:使用数字，字符,罗马数字等后跟点，然后空格，然后是内容
  - 字段列表:如
  
  :Authors: xuetrdi,
  :version: 1.0
  :Dedication: docs
  
  - 命令选项列表:快速构建命令行参数文档
  - 引用块，如下：
  
  content below is literal Blocks::
  
  > this is context
  
  - 表格
  - 水平线
  

5. Sphinx文档结构化三阶段
-------------------------

* 单个文档结构

* 多个文件目录结构

* 连接无直接父子关系结构的网络

* 组织文档结构

Sphinx能将所有文档文件组合到一个树结构中,这样,所有文件都被排成了一个序列,并以让人能从上至下阅读的格式进行输出.

该定义需在文档当中`.. toctree::`指令描述.

只要有了toctree这个主干,文档就能够被分割成多个文件之后仍保持其结构.

**链接**可以让我们更容易在文档种找到想找的信息.

只要按照一定的规则给文档加入关键字或到其它章节的跳转,就能实现灵活的网状结构.**脚注**,**交叉引用**,**术语集**,**索引**等就是此类网状结构.

需要统一的术语集,使用:term:`术语`的形式将该术语写下来.需要创建数据集以及术语说明,否则make时会提示,最好放在最后写,不会影响进度.

还可以用:doc:`../sub/index`这样的形式指定引用页的相对路径,在Sphinx在make时会自动将该页面的标题和链接填充到这里.

组织文档结构
------------

* reStructedtext文档指令的用法结构::

+ 指令
+ 参数
+ 选项
+ 空行
+ 内容

* 跳转式

  * toctree指令
  


其中执行是双点号空格开头，然后是指令，然后是双冒号，双冒号后面可以跟参数，参数下面是选项，选项用冒号包裹，前面的冒号需要跟指令对齐，后面的冒号后面是选项的值，然后是空行，再然后是具体的内容，如下

.. toctree::
   :maxdepth: 2
   :caption: 目录:
   
   pre
   first
   second
   
[注]其中前言，第一章，第章这些对应的位置和文件。使用sphinx-quickstart生成project后，会在项目文档下的source目录下生成index.rst文件，现在我们在与index.rst同一目录下创建pre.rst,first.rst,second.rst,然后就会形成内部链接，make html后在index.rst,pre.rst,first.rst,second.rst都会生成对应的html文件，在index.html，显示pre.rst,first.rst,second.rst中的Header部分，比如分别:

pre.rst

\=========

前言

\=========

first.rst

\=========

第一章

\=========

second.rst

\=========

第二章

\=========

在在index.html中就会显示出

目录：

前言

第一章

第二章
  
  * doc指令
  
  这个指令会链接成功并且可以正确打开，但是在make htm时会报警告。指令用法如:doc:`第一章`
* 嵌入式

include指令，如.. include:: 第一章.rst

嵌套就是不会跳转，会把第一章.rst直接嵌套显示在index.html中

Sphinx自动化生成Python文档
--------------------------

* 生成的文档必要条件：

1. 在conf.py中有autodoc的扩展
2. 在conf.py中有解注释sys.path,并且设置好路径，一般可以默认

* 生成文档的过程

sphinx工程建立后，在其工程的跟目录下执行sphinx-apidoc -o，会把py文件生成对应的rst文件，每个py文件是一个模块，每个模块对应的一个rst文件，最终make html执行后，这些rst文件会生成对应的html文件。

* 生成一份好文档的核心

其核心是在Python文件的docstring上，也就是三引号里面的内容。编写带有api形式的docstring等等是关键。所以是学习怎么写Python的模块文档注释，这个决定了最终生成的是什么api文档。一旦写好了py文件的文档注释，然后执行sphinx-apidoc生成rst，然后执行make html生成html，也就是最终的显示效果。另外的一个问题就是如何链接这些rst文件，让最终的结果能自动跳转，能正常显示的问题，也就是文档链接。

[重要]其实一份的好的文档一个是docstring的书写，另外就是rst文件的组织结构问题。



