FROM mathworks/matlab-deep-learning:r2021b AS needReplacing
USER root
# delete the offical install matlab
RUN rm -rf /opt/matlab && \
    rm -rf /tmp/*

FROM scratch
COPY --from=needReplacing / /

# ADD FULL MATLAB R2021b, auto extract local tar/gzip/bzip2/xz file
ADD --chown=matlab:matlab Matlab-R2021b-bin.tar /
# restore defaultpath: del plotly_matlab & imatlab from Matlab-R2021b-bin.tar
RUN sed -i '/imatlab/d' /opt/matlab/R2021b/toolbox/local/pathdef.m && \
    sed -i '/plotly_matlab/d' /opt/matlab/R2021b/toolbox/local/pathdef.m

# ADD some new dependencies packages from https://github.com/mathworks-ref-arch/container-images
RUN RUN wget https://raw.githubusercontent.com/dennischancs/matlab-deep-learning-full/main/matlab-deep-learning-full/base-dependencies.txt && \
    python3 -m pip install matlab-proxy*.whl && \
    sed -i 's/\/.*ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list && \
    apt-get update && apt-get install --no-install-recommends -y `cat /tmp/base-dependencies.txt` \
    && apt-get clean && rm -rf /var/lib/apt/lists/* && rm -rf /tmp/*
# resolve any license manager issues
RUN ln -s /lib64/ld-linux-x86-64.so.2 /lib64/ld-lsb-x86-64.so.3

ENV DEBIAN_FRONTEND=noninteractive TZ=Asia/Shanghai

#---recovery the ENV variables
# NV_CUDA
ENV NVARCH=x86_64
ENV NVIDIA_REQUIRE_CUDA=cuda>=11.1 brand=tesla,driver>=418,driver<419 brand=tesla,driver>=440,driver<441 driver>=450
ENV NV_CUDA_CUDART_VERSION=11.1.74-1
ENV NV_CUDA_COMPAT_PACKAGE=cuda-compat-11-1
ENV NV_ML_REPO_ENABLED=1
ENV NV_ML_REPO_URL=https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu2004/x86_64
ENV CUDA_VERSION=11.1.1
ENV NVIDIA_VISIBLE_DEVICES=all
ENV NVIDIA_DRIVER_CAPABILITIES=compute,utility

# PATH && LD_LIBRARY_PATH
ENV PATH=/usr/local/nvidia/bin:/usr/local/cuda/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
ENV LD_LIBRARY_PATH=/usr/local/cuda/lib64:/usr/local/nvidia/lib:/usr/local/nvidia/lib64

USER matlab

ENV PATH=/usr/local/nvidia/bin:/usr/local/cuda/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
ENV LD_LIBRARY_PATH=/usr/local/cuda/lib64:/usr/local/nvidia/lib:/usr/local/nvidia/lib64

EXPOSE 5901 6080
ENV MHLM_CONTEXT=MATLAB_DL_DOCKERHUB
ENTRYPOINT ["/bin/run.sh"]