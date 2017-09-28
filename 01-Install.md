#环境说明  
OS:   mac0S Sierra   Version 10.12.6   (MacBook Pro)  
Python:   Version 2.7.13  
Tensorflow:  Version 1.3.0  (CPU)  

在以上环境中直接用pip安装tensorflow可能会有以下错误：  

```
OSError: [Errno 1] Operation not permitted: '/tmp/pip-gjo3uz-uninstall/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/numpy-1.8.0rc1-py2.7.egg-info'
```

即使使用pip的参数 --ignore-installed也不行  
uninstall numpy也不行  
用brew重装python可以解决该问题  
用brew重装python需要Xcode和Command Line Tools  
按照如下步骤即可在macOS Sierra操作系统成功安装Tensorflow 1.3.0 CPU版本  

**下载过程中，可能会有Read Timeout，重试即可**

#Xcode安装  
安装or更新Xcode，重装python需要  
通过AppStore安装  

#Apple Command Line Tools安装  
安装or更新，重装python需要  
通过命令行安装：  
`xcode-select --install`

#Xcode license接受  
接受xcode license，重装python需要  
通过命令行接受：  
`sudo xcodebuild -license accept`

#python更新  
`brew reinstall python`

#pip setuptools wheel更新  
`pip install -U pip setuptools wheel`

不做该更新会有如下错误：  

```
AttributeError: 'module' object has no attribute 'evaluate_marker'

error in setup command: Error parsing /private/tmp/pip_build_root/mock/setup.cfg: AttributeError: 'module' object has no attribute 'evaluate_marker'
```

#tensorflow安装  

`sudo pip install --upgrade --ignore-installed https://storage.googleapis.com/tensorflow/mac/cpu/tensorflow-1.3.0-py2-none-any.whl`

#tensorflow测试  
**python脚本测试**  
```python
import tensorflow as tf
hello = tf.constant('Hello, TensorFlow!')
session = tf.Session()
print session.run(hello)
```
**执行结果**  
`Hello, TensorFlow!`

**以上脚本执行过程会有如下警告**  
```
W tensorflow/core/platform/cpu_feature_guard.cc:45] The TensorFlow library wasn't compiled to use SSE4.2 instructions, but these are available on your machine and could speed up CPU computations.

W tensorflow/core/platform/cpu_feature_guard.cc:45] The TensorFlow library wasn't compiled to use AVX instructions, but these are available on your machine and could speed up CPU computations.
```
意思是本地CPU支持SSE4.2，AVX指令集可以加速Tensorflow，但我们编译时并没有指定支持  

相关讨论可以参考：  
<https://github.com/tensorflow/tensorflow/issues/7778>

去除警告办法：  
1 修改tensorflow日志级别：（默认为0，显示所有日志信息）  

```python
import os
os.environ['TF_CPP_MIN_LOG_LEVEL']='2'
import tensorflow as tf
```

2 重新编译优化版本：  
相关讨论可以参考：  
<https://github.com/tensorflow/tensorflow/issues/8037>

优化编译参考：  
<https://github.com/lakshayg/tensorflow-build>

**欢迎完整分享，禁止用于商业用途**

  

By Master, Beiming   

Email:   <beimingmaster@gmail.com>  
Wechat:  beimingmaster  

&copy;2017  