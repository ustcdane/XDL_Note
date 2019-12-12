# Use one of the GPU based XDL images as the parent image
FROM registry.cn-hangzhou.aliyuncs.com/xdl/xdl:ubuntu-gpu-mxnet1.3
#FROM registry.cn-hangzhou.aliyuncs.com/xdl/xdl:ubuntu-gpu-tf1.12
RUN rm -f /etc/apt/sources.list.d/*

LABEL maintainer "DanielWang <daneustc@gmail.com>"
# Remove CUDA and Nvidia related packages from the parent image to avoid conflict with the host Nvidia driver's libraries and tools.
RUN apt -y --allow-unauthenticated  remove cuda-*
RUN apt -y --allow-unauthenticated  remove nvidia-*
# Add custom china source
COPY sources.list /etc/apt/

RUN apt-get clean && apt-get update 
RUN apt-get install -y apt-transport-https && apt-get install vim

RUN rm /etc/apt/sources.list
COPY aliyun.sources.list /etc/apt/sources.list

#RUN apt-get install -y apt-transport-https
# Install CUDA computing and machine learning components. Refer to the official Nvidia Dockerfile: https://gitlab.com/nvidia/cuda/blob/ubuntu16.04/9.0/base/Dockerfile

RUN apt-get clean && apt-get update && apt-get install -y --allow-unauthenticated  --no-install-recommends ca-certificates apt-transport-https gnupg-curl && \
    rm -rf /var/lib/apt/lists/* && \
    NVIDIA_GPGKEY_SUM=d1be581509378368edeec8c1eb2958702feedf3bc3d17011adbf24efacce4ab5 && \
    NVIDIA_GPGKEY_FPR=ae09fe4bbd223a84b2ccfce3f60f4b3d7fa2af80 && \
    apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub && \
    apt-key adv --export --no-emit-version -a $NVIDIA_GPGKEY_FPR | tail -n +5 > cudasign.pub && \
    echo "$NVIDIA_GPGKEY_SUM  cudasign.pub" | sha256sum -c --strict - && rm cudasign.pub && \
    echo "deb https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64 /" > /etc/apt/sources.list.d/cuda.list && \
    echo "deb https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1604/x86_64 /" > /etc/apt/sources.list.d/nvidia-ml.list

COPY ./*.deb /var/cache/apt/archives/
#COPY ./*.deb /tmp/
#RUN dpkg -i /tmp/cuda-cublas-9-0_9.0.176-1_amd64.deb
#RUN dpkg -i /tmp/cuda-core-9-0_9.0.176-1_amd64.deb
#RUN dpkg -i /tmp/cuda-cufft-9-0_9.0.176-1_amd64.deb
#RUN dpkg -i /tmp/cuda-curand-9-0_9.0.176-1_amd64.deb
#RUN dpkg -i /tmp/cuda-cusolver-9-0_9.0.176-1_amd64.deb
#RUN dpkg -i /tmp/cuda-cusparse-9-0_9.0.176-1_amd64.deb

ENV CUDA_VERSION 9.0.176
ENV CUDA_PKG_VERSION 9-0=$CUDA_VERSION-1
RUN apt-get update && apt-get install -y --allow-unauthenticated  --no-install-recommends \
    cuda-cudart-$CUDA_PKG_VERSION cuda-cublas-$CUDA_PKG_VERSION cuda-core-$CUDA_PKG_VERSION cuda-cufft-$CUDA_PKG_VERSION cuda-curand-$CUDA_PKG_VERSION cuda-cusolver-$CUDA_PKG_VERSION cuda-cusparse-$CUDA_PKG_VERSION cuda-libraries-$CUDA_PKG_VERSION libcudnn7=7.0.5.15-1+cuda9.0 libcudnn7-dev=7.0.5.15-1+cuda9.0 libnccl2=2.1.4-1+cuda9.0 libnccl-dev=2.1.4-1+cuda9.0 && \
    ln -s cuda-9.0 /usr/local/cuda && \
    rm -rf /var/lib/apt/lists/*

# nvidia-docker 1.0
LABEL com.nvidia.volumes.needed="nvidia_driver"
LABEL com.nvidia.cuda.version="${CUDA_VERSION}"

RUN echo "/usr/local/nvidia/lib" >> /etc/ld.so.conf.d/nvidia.conf && \
    echo "/usr/local/nvidia/lib64" >> /etc/ld.so.conf.d/nvidia.conf

ENV PATH /usr/local/nvidia/bin:/usr/local/cuda/bin:${PATH}
ENV LD_LIBRARY_PATH /usr/local/nvidia/lib:/usr/local/nvidia/lib64

# nvidia-container-runtime
# Add these environment variables, so nvidia-docker will map the driver libraries and tools from the host to the container. 
# Refer to: https://devblogs.nvidia.com/gpu-containers-runtime
ENV NVIDIA_VISIBLE_DEVICES all
ENV NVIDIA_DRIVER_CAPABILITIES compute,utility
ENV NVIDIA_REQUIRE_CUDA "cuda>=9.0"

# Fix the bug of pip
RUN sed -i 's/pip._internal/pip/g' /usr/local/bin/pip
