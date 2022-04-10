#----------
# FROM node:lts-alpine3.15 AS matlab-proxy-build
# USER root
# RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories && \
#     apk update && apk add git gcc musl-dev python3-dev python3 py3-pip  && \
#     cd /tmp && git clone https://github.com/mathworks/matlab-proxy && \
#     cd matlab-proxy && \
#     # change serviceWorker.unregister() to serviceWorker.register(), let app to work offline and load faster
#     sed -i 's#// import \* as serviceWorker#import \* as serviceWorker#g' gui/src/index.js  && \
#     echo 'serviceWorker.register();' >> gui/src/index.js  && \
#     python3 -m pip install .
#---------

FROM dennischancs/matlab-full:r2021b
USER root

#---add [matlab-proxy](https://github.com/mathworks/matlab-proxy/blob/main/examples/Dockerfile)
# Install dependencies for matlab-proxy
RUN sed -i 's/\/.*ubuntu.com/mirrors.aliyun.com/g' /etc/apt/sources.list && \
    apt-get update && apt-get install --no-install-recommends -y \
    && apt-get install --no-install-recommends -y \
    python3 \
    python3-pip \
    xvfb \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# install matlab-proxy
# online version
#RUN python3 -m pip install matlab-proxy
# offline version
# COPY --from=matlab-proxy-build /usr/bin/matlab-proxy-app /usr/local/bin/
# COPY --from=matlab-proxy-build /usr/lib/python3.9/site-packages/matlab_proxy-0.2.7-py3.9.egg-info /usr/local/lib/python3.8/dist-packages/
# COPY --from=matlab-proxy-build /usr/lib/python3.9/site-packages/matlab_proxy /usr/local/lib/python3.8/dist-packages/
RUN wget && \
    python3 -m pip install matlab_proxy*.whl


# Set environment variables used by integration
# MATLAB should then become accessible on http://localhost:8888/matlab/index.html
ENV MWI_APP_PORT=8888
ENV MWI_BASE_URL="/matlab"

USER matlab
ENTRYPOINT ["matlab-proxy-app"]