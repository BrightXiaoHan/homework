

# CS294深度强化学习(一) - 作业环境搭建



最近正在学习[cs294](http://rail.eecs.berkeley.edu/deeprlcourse/) 深度强化学习的课程，本系列的文章分享一些自己的心得和踩过的坑。下面进入正题，cs294共包含5个课后作业，都是使用课程上的算法在 [MuJoCo](http://www.mujoco.org/book/index.html#Examples)上训练简单智能体的任务，下面介绍一下在mac上搭建开发环境。

## 配置开发环境

### 基本配置

**从github上下载项目**

```
git clone https://github.com/berkeleydeeprlcourse/homework.git
cd homework
```

**创建虚拟环境**

首先在Anaconda上创建一个虚拟环境，这里最好使用hw1文件夹中README推荐的python3.5。

```
conda create -n cs294 python=3.5
source activate cs294
```

**安装python依赖包**

这里使用豆瓣源加快安装速度

```
pip install -i https://pypi.douban.com.simple -r requirements.txt
```

### 配置Mojoco

想要mujoco-py正常运行需要官方的许可证书，永久的证书需要付费，我们可以到官网申请免费30天试用，你也可以试用教育邮箱申请一年的免费试用证书。这里以申请30天免费试用为例。

进入官网[mojoco](https://www.roboti.us/license.html), 找到这个窗口

![mojoco注册](1.png)

根据你的操作系统选择下载程序获取你的computer id，我这里选择OSX。下载后执行下载的文件获取id

```
chmod +x getid_osx
./getid_osx
```

将打印的id复制到对话框中，并填写其他信息，官网会发送两个文件到你的邮箱中，把他们下载下来。

- mjkey.txt
- mjpro150_osx.zip

```Bash
unzip mjpro150_osx.zip
mkdir ~/.mujoco
mv mjkey.txt ~/.mujoco
mv mjpro150 ~/.mujoco
```

### 运行

运行hw1中的脚本，运行成功会看到模拟器中智能体。

```
cd /path/to/yourhomework/hw1/
bash demo.bash
```



## 踩过的几个坑

安装过程中可能会出现几个错误，这里分享出来。

### gcc 版本问题 (Mac os Mojave)

如果运行过程中报gcc相关的错误，可以试用brew安装gcc6解决

```
brew install gcc@6
```

### expert_data don't exist (Mac os Mojave)

这是个小坑，大家一看就能解决

```
cd hw1
mkdir expert_data
```

### Linux 下遇见的一些错误 （Ubuntu 16.04 cuda8）

加入环境变量到.bashrc

```
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/hanbing/.mujoco/mjpro150/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/lib/nvidia-387
```

编译时的错误

```
Traceback (most recent call last):
  File "/home/hanbing/anaconda3/envs/cs294/lib/python3.5/distutils/unixccompiler.py", line 118, in _compile
    extra_postargs)
  File "/home/hanbing/anaconda3/envs/cs294/lib/python3.5/distutils/ccompiler.py", line 909, in spawn
    spawn(cmd, dry_run=self.dry_run)
  File "/home/hanbing/anaconda3/envs/cs294/lib/python3.5/distutils/spawn.py", line 36, in spawn
    _spawn_posix(cmd, search_path, dry_run=dry_run)
  File "/home/hanbing/anaconda3/envs/cs294/lib/python3.5/distutils/spawn.py", line 159, in _spawn_posix
    % (cmd, exit_status))
distutils.errors.DistutilsExecError: command 'gcc' failed with exit status 1
```

安装依赖包

```
sudo apt-get install libglew-dev
```

再运行由报以下错误

```
ERROR: GLEW initalization error: Missing GL version
```

加入环境变量到.bashrc

```
export LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libGLEW.so
```

 

