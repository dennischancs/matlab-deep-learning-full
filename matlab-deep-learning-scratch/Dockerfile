FROM mathworks/matlab-deep-learning:r2021b AS needReplacing
USER root

# ADD some new dependencies packages from https://github.com/mathworks-ref-arch/container-images
COPY base-dependencies.txt /tmp/base-dependencies.txt
RUN sed -i 's/\/.*ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list && \
    apt-get update && apt-get install --no-install-recommends -y `cat /tmp/base-dependencies.txt` \
    && apt-get clean && apt autoremove -y && rm -rf /var/lib/apt/lists/* && rm -rf /tmp/*
# resolve any license manager issues
RUN ln -s /lib64/ld-linux-x86-64.so.2 /lib64/ld-lsb-x86-64.so.3

# delete the offical install matlab
RUN rm -rf /opt/matlab && \
    rm -rf /tmp/*

FROM scratch
COPY --from=needReplacing / /