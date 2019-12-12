# XDL note
Note for [XDL](https://github.com/alibaba/x-deeplearning)

## Contents 列表
### 1. [XDL wiki](https://github.com/alibaba/x-deeplearning/wiki) 
### 2. GPU problems GPU相关问题
- [XDL 关于 GPU相关issues ](https://github.com/alibaba/x-deeplearning/issues?utf8=%E2%9C%93&q=GPU+) 
- [修复官方镜像GPU问题](https://github.com/alibaba/x-deeplearning/issues/116), 利用[Dockerfile_for_gpu](https://github.com/alibaba/x-deeplearning/blob/f8d031cc7374a07c6e5f1a8177ccb0e336e5b76e/xdl/docker/Dockerfile_for_gpu) 重新构建镜像，由于我的宿主机下载一些XX.deb比较慢因此手动下载了，然后放在本地缓存安装，具体查看我的Dockerfile目录，构建新的镜像命令：
docker build -t xdl:ubuntu-gpu-mxnet1.3 .