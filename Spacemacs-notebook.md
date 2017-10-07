# Emacs/Spacemacs 中编写notebook
**要点如下:**

- 操作环境
- 依赖系统环境
- 依赖Python环境
- 设置jupyter
- 在Spacemacs中安装notebook layer
- 使用Spacemacs中的note-book

## 搭建环境

1.操作环境

- OS：Ubuntu16.04 LST

- Python：Python3.5.2

2.安装依赖库和工具

sudo apt-get install g++

sudo apt-get install libfreetype6-dev

sudo apt-get install gfortran

sudo apt-get install libopenblas-dev liblapack-dev

sudo apt-get apt-get install libpng12-0 libpng12-dev

3.安装Python依赖包

- 科学计算包

sudo pip3 install numpy

sudo pip3 install pandas

sudo pip3 install matplotlib

sudo pip3 install scipy

sudo pip3 install seaborn

sudo pip3 install scikit-learn

- 安装jupyter notebook

sudo pip3 install urllib3

sudo pip3 install ipython

sudo pip3 install jupyter

4.设置jupyter

在命令行运行，注意，如果你没有安装上面的库可能执行报错
python3 -c "from notebook.auth import passwd;print(passwd())"

然后输入你想设置的一个密码，这个密码用于后面登录用，一定要记得
复制sha1:fe4a5785aa9b:d11adbaaf789fa560e8b637d69cdd0f386ce4712

cd .jupyter

vi jupyter_notebook_config.py 或者gedit jupyter_notebook_config.py

在这个文件中编辑如下内容：（后面的那串密码就是上面生成的）

c.NotebookApp.password = "sha1:83c7dddb4465:46739089b03d417962d4a14246a28efa584bc892"

编辑好后退出保存。

5.spacemacs安装ipython-notebook 

这个可以看github上的spacemacs ipython-notebook layer
具体如下：
在你的～/.spacemacs或者~/.spacemacs.d/init.el中 dotspacemacs-configuration-layers中增加ipython-notebook，保存退出。重启spacemacs.
如果是Emacs依赖的插件是ein插件，安装方法M-x package-install,RET, ein,RET

以上是所需环境。

## 如何在Spacemacs中使用notebook

1.在命令行启动jupyter

jupyter notebook

2.在spacemacs中登录

M-x notebook-login RET

提示密码输入，然后输入上面设置的密码，RET后会提示已经登录使用

3.在Spacemacs中输入快捷键，SPC a i n，RET，默认8888端口,回车就可以登录成功了

4.如果在Emacs中，第三步为M-x，然后输入ein:notebooklist-open，RET，看到端口RET就成功了

## 推荐教程

关于Spacemacs的教程，子龙闪人的[Spacemacs教程](https://github.com/emacs-china/Spacemacs-rocks)
