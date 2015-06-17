### Alpine/Ubuntu multi-proc Docker images for Mubiic application containers/kubes
#### Author: [Howard Mei](howardmei at mubiic dot com)
#### License: Apache
#### Credits: [just-containers@github](https://github.com/just-containers/base)

### Proc image s6 init manager usage
s6 init config dir by applying order[inside scripts applying order is according to naming sort,
so 00-servicename0, 01-servicename1, 0N-servicenameN are the proper script names]:
/etc/fix-attrs.d      holds scripts to ensure files owners and permissions are correct
/etc/cont-init.d      holds one-time system scripts to execute container init before all
/etc/services.d       holds user services for long-lived daemon processes to be supervised:
**s6-setuidgid could be used to run daemon as another user in following scripts**
/etc/services.d/*/run           hold service daemons running management scripts
/etc/services.d/*/finish        hold service daemons exit clean up scripts
/etc/services.d/*/log/run       hold service daemons running logging scripts
/etc/services.d/*/log/finish    hold service daemons exit logging scripts
/etc/cont-finish.d    holds one-time system scripts to clean up container env before exit

s6init working dir: /var/run/s6

#### Available Images:
Base Image Repo is located at:[mubiic/dockerosebase@github](https://github.com/mubiic/dockerosbase)
Proc Image Repo is located at:[mubiic/dockerosebase@github](https://github.com/mubiic/dockerpsbase)
Public Images are avaliable at: [mubiic@dockerhub](https://registry.hub.docker.com/repos/mubiic/)
