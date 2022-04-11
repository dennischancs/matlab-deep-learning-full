FROM dennischancs/matlab-deep-learning-scratch:r2021b
USER root

# ADD FULL MATLAB R2021b, auto extract local tar/gzip/bzip2/xz file
ADD --chown=matlab:matlab Matlab-R2021b-bin.tar /
# restore defaultpath: del plotly_matlab & imatlab from Matlab-R2021b-bin.tar
RUN sed -i '/imatlab/d' /opt/matlab/R2021b/toolbox/local/pathdef.m && \
    sed -i '/plotly_matlab/d' /opt/matlab/R2021b/toolbox/local/pathdef.m

#---recovery the ENV variables
ENV DEBIAN_FRONTEND=noninteractive TZ=Asia/Shanghai
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