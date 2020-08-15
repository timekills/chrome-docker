# Chrome Docker
## What is it?
A Docker image that can run Google Chrome.

## How does it work?
The Docker image includes a VNC server which provides graphical access to the
virtual display running in the container.

## How do I build and run it?
mkdir /opt/appdata/chrome

sudo chown 1000:1000 /opt/appdata/chrome

cd /opt/appdata/chrome

wget https://github.com/timekills/chrome-docker/archive/master.zip && unzip master.zip && rm -r master.zip

cd /opt/appdata/chrome/chrome-docker-master/image/ && sudo docker build -t timekills/chrome-docker .

sudo docker run -d -p 5900:5900 -e VNC_SERVER_PASSWORD=WhatEverYouWant -e JAVA_OPTS='-Xmx8000M' --user apps --privileged --volume /opt/appdata/chrome:/home/apps --name "chrome" timekills/chrome-docker

## How do I access it?
Use whatever VNC client you wish, on the default port (5900)

**Note**: The macOS VNC client will not be able to login unless you set a
password for the VNC server. The instructions for setting a VNC password can be
found below.

By default, the VNC server is started without a password. If you would like to
specify a password for the VNC server, do the following:
```
docker run -p 5900:5900 -e VNC_SERVER_PASSWORD=some-password --name chrome \
    --user apps --privileged <image-name>
```

Once the container is running, you can VNC into it at `127.0.0.1` and run Chrome
from a terminal window by running:
```
google-chrome
```

You can also start Google Chrome by right-clicking the Desktop and selecting:
```
Applications > Network > Web Browsing > Google Chrome
```

## Additional settings
Refer to the [configuration documentation](docs/configuration).

## Security concerns
This image starts a X11 VNC server which spawns a framebuffer. Google Chrome
also requires that the image be run with the `--privileged` flag set. This flag
disables security labeling for the resulting container. Be very careful if you
run the container on a non-firewalled host.

Some applications (such as Google Chrome) will not run under the root user. A
non-root user named `apps` is included for such scenarios.
