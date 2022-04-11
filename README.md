# matlab-deep-learning-full
 docker image with full version matlab-r2021b and offline version matlab-proxy


# [Usage](https://hub.docker.com/r/mathworks/matlab-deep-learning#How-to-use-this-image)

## matlab full with matlab-proxy offline version
> This matlab-proxy package supports the same subset of MATLAB features and commands as MATLAB® Online, except there is no support for Simulink® Online.

To pull the R2022a MATLAB image to your machine, execute:
`docker pull dennischancs/matlab-deep-learning-full:r2021b-matlab-proxy`

To launch the container with the -browser option, execute:

`docker run -it --rm -p 8888:8888 --shm-size=512M dennischancs/matlab-deep-learning-full:r2021b-matlab-proxy`
`docker run -it --rm -p 8888:8888 --shm-size=512M mathworks/matlab-deep-learning:r2022a -browser`

PS.: 不好用，很卡。虽然交互比noVNC好（复制粘贴方便），但在关键节点（保存文件、plot等情况下）会卡很久。可能是任务阻塞了

Executing this command will display a URL on which you can access MATLAB, for example:
http://localhost:8888/index.html

## matlab full
> support Simulink®
Run MATLAB in desktop mode and interact with it via VNC
`docker run -it --rm -p 5901:5901 -p 6080:6080 --shm-size=512M dennischancs/matlab-deep-learning-full:r2021b -vnc`

Run a bash shell inside the container
`docker run -it --rm --shm-size=512M dennischancs/matlab-deep-learning-full:r2021b -shell`

Run MATLAB desktop using X11
```bash
xhost +
docker run -it --rm -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix:ro --shm-size=512M dennischancs/matlab-deep-learning-full:r2021b
```
