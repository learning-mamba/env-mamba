# env-mamba
mamba在Windows的环境配置
这里复现的是两个大佬的mamba环境

https://github.com/walking-shadow/Official_Remote_Sensing_Mamba

https://github.com/MzeroMiko/VMamba

这两个环境对于我来说踩了不少的坑所以现在来简单介绍一下怎么配置好这个环境

我的电脑环境是window11 i7-14700kf 2080ti22g 

pytorch2.2+cuda12.2  

这个环境是基于py11的，我们从conda开始

```shell

conda create -n vmamba python==3.11

```

```shell

pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121

```

为了方便我们环境的搭建我在这两个大佬的仓库里面下载了这两个必要的环境


# 这里是https://github.com/state-spaces/mamba 的编译
首先我们的环境必须要有一个triton的支持
triton来源于https://github.com/jakaline-dev/Triton_win/releases/tag/3.0.0

我们下载完这个离线包之后放在本地文件夹下


```shell

pip install triton-3.0.0-cp311-cp311-win_amd64.whl

```

```shell

cd mamba

pip install -r requestment.txt

pip install .

```







# 这里是https://github.com/SeanSong-amd/causal-conv1d 的编译

