#----------
# FROM node:lts-alpine3.15 AS matlab-proxy-build
# USER root
# RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories && \
#     apk update && apk add git gcc musl-dev python3-dev python3 py3-pip  && \
#     cd /tmp && git clone https://github.com/mathworks/matlab-proxy && \
#     cd matlab-proxy && \
#     # change serviceWorker.unregister() to serviceWorker.register(), let app to work offline and load faster
#     sed -i 's#// import \* as serviceWorker#import \* as serviceWorker#g' gui/src/index.js  && \
#     echo 'serviceWorker.register();' >> gui/src/index.js  && \
#     python3 -m pip install .
#---------

FROM dennischancs/matlab-deep-learning-full:r2021b
USER root

#---add [matlab-proxy](https://github.com/mathworks/matlab-proxy/blob/main/examples/Dockerfile)
# online version
#RUN python3 -m pip install matlab-proxy
# offline version
# COPY --from=matlab-proxy-build /usr/bin/matlab-proxy-app /usr/local/bin/
# COPY --from=matlab-proxy-build /usr/lib/python3.9/site-packages/matlab_proxy-0.2.7-py3.9.egg-info /usr/local/lib/python3.8/dist-packages/
# COPY --from=matlab-proxy-build /usr/lib/python3.9/site-packages/matlab_proxy /usr/local/lib/python3.8/dist-packages/
RUN wget https://raw.githubusercontent.com/dennischancs/matlab-deep-learning-full/main/matlab-proxy/matlab-proxy-0.2.7-py3-none-any.whl && \
    python3 -m pip install matlab-proxy*.whl && \
    rm -rf matlab-proxy*.whl


# Set environment variables used by integration
# MATLAB should then become accessible on http://localhost:8888/matlab/index.html
ENV MWI_APP_PORT=8888
ENV MWI_BASE_URL="/matlab"

USER matlab
ENTRYPOINT ["matlab-proxy-app"]