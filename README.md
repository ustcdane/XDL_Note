# XDL note
Note for [XDL](https://github.com/alibaba/x-deeplearning)

## Contents 列表
### 1. 想要跑XDL还是需要认真看下官方wiki [XDL wiki](https://github.com/alibaba/x-deeplearning/wiki) 
### 2. GPU problems GPU相关问题
- [XDL 关于 GPU相关issues ](https://github.com/alibaba/x-deeplearning/issues?utf8=%E2%9C%93&q=GPU+) 
- [修复官方镜像GPU问题](https://github.com/alibaba/x-deeplearning/issues/116), 利用[Dockerfile_for_gpu](https://github.com/alibaba/x-deeplearning/blob/f8d031cc7374a07c6e5f1a8177ccb0e336e5b76e/xdl/docker/Dockerfile_for_gpu) 重新构建镜像，具体查看我的Dockerfile目录(我的xx.deb是提前下载的)，构建新的镜像命令：
docker build -t xdl:ubuntu-gpu-mxnet1.3 .
- apt-get 下载太慢且不能更改软件源 如NVIDIA，可以把下载到的相关xx.deb拷贝到目录
/var/cache/apt/archives  apt-get 能搜索到xx.deb  [ref](https://askubuntu.com/questions/288631/apt-get-retries-download-endlessly)
- **The method driver /usr/lib/apt/methods/https could not be found**  把https 软件源换成 HTTP的软件源还是出现上面的问题，可能因为是/etc/apt/sources.list.d/有https打头的软件源。删除即可。[ref](
https://unix.stackexchange.com/questions/263801/apt-get-fails-the-method-driver-usr-lib-apt-methods-https-could-not-be-found)
### 3. 出core了, 如何调试？ 
- 可以安装python版gdb sudo apt-get install gdb python-dbg, 安装完毕后执行gdb python 然后运行 run train.py --run_mode=local --config=config.train.json
运行一会后出core，输出命令 core-file core.xxx(xxx是你刚才出的core的编码), 输入bt回车,查看栈信息。[ref](https://github.com/alibaba/x-deeplearning/issues/59#issuecomment-454681359)

### 4. 线上线下打分对齐问题？ 
- [issues/313](https://github.com/alibaba/x-deeplearning/issues/313)
- [issues/299](https://github.com/alibaba/x-deeplearning/issues/299)