# XDL note
Note for [XDL](https://github.com/alibaba/x-deeplearning)

## Contents 列表
### 1. [XDL wiki](https://github.com/alibaba/x-deeplearning/wiki) 
### 2. GPU problems GPU相关问题
- [XDL 关于 GPU相关issues ](https://github.com/alibaba/x-deeplearning/issues?utf8=%E2%9C%93&q=GPU+) 
- [修复官方镜像GPU问题](https://github.com/alibaba/x-deeplearning/issues/116), 利用[Dockerfile_for_gpu](https://github.com/alibaba/x-deeplearning/blob/f8d031cc7374a07c6e5f1a8177ccb0e336e5b76e/xdl/docker/Dockerfile_for_gpu) 重新构建镜像，具体查看我的Dockerfile目录(我的xx.deb是提前下载的)，构建新的镜像命令：
docker build -t xdl:ubuntu-gpu-mxnet1.3 .
- apt-get 下载太慢且不能更改软件源 如NVIDIA，可以把下载到的相关xx.deb拷贝到目录
/var/cache/apt/archives  apt-get 能搜索到xx.deb https://askubuntu.com/questions/288631/apt-get-retries-download-endlessly
- **The method driver /usr/lib/apt/methods/https could not be found **  把https 软件源换成 HTTP的软件源还是出现上面的问题，可能因为是/etc/apt/sources.list.d/有https打头的软件源。删除即可。
https://unix.stackexchange.com/questions/263801/apt-get-fails-the-method-driver-usr-lib-apt-methods-https-could-not-be-found