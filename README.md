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

cd mamba\mamba-main

pip install -r requestment.txt

pip install .

```



# 这里是https://github.com/SeanSong-amd/causal-conv1d 的编译


这里也是借鉴了大佬的库

```shell

cd causal-conv1d\causal-conv1d-main

pip install .
```

目前是完成了https://github.com/walking-shadow/Official_Remote_Sensing_Mamba 的踩坑

检查环境中是否有mamba-ssm和causal-con1d

然后就可以使用这个mamba的模型啦

# 接下来是https://github.com/MzeroMiko/VMamba 踩的坑

首先是kernals编译不起来

这个的问题出现在C++的编译器

我们首先要有C++的编译器，如果你有了visual studio这里就可以跳过了

参考https://github.com/MzeroMiko/VMamba/issues/95

是static_switch.h的constexpr出现了非常量的错误

我们手动添加一个static在constexpr前面

![image](https://github.com/learning-mamba/env-mamba/assets/66856290/ea4ae859-9fb6-4e6a-9c2a-11ce88acac8a)

然后在以下的文件里添加

```shell

#ifndef M_LOG2E
#define M_LOG2E 1.4426950408889634074
#endif

```
在这
kernels/selective_scan/csrc/selective_scan/cus/selective_scan_bwd_kernel.cuh
kernels/selective_scan/csrc/selective_scan/cus/selective_scan_fwd_kernel.cuh
kernels/selective_scan/csrc/selective_scan/cusndstate/selective_scan_bwd_kernel_ndstate.cuh
kernels/selective_scan/csrc/selective_scan/cusndstate/selective_scan_fwd_kernel_ndstate.cuh
kernels/selective_scan/csrc/selective_scan/cusoflex/selective_scan_bwd_kernel_oflex.cuh
kernels/selective_scan/csrc/selective_scan/cusoflex/selective_scan_fwd_kernel_oflex.cuh

像这样
![image](https://github.com/learning-mamba/env-mamba/assets/66856290/f8f23011-9043-4529-ab0d-a20cb6dde64f)


如果你前面的mamba没有安装的话也是运行不了这个项目的！！！

