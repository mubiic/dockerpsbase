FROM            mubiic/alpinebash:3.2
MAINTAINER      Howard Mei      <howardmei@mubiic.com>
ENV             KUBECONF        **TBD**
ENV             APPCONF         **TBD**
ENV             S6VER           v1.A2.1
ENV             BUILDTAG        alpineproc:3.2/r20150612/sg/A

# Add conf to fix s6 init suite permissions
COPY            etc             /etc
COPY            usr             /usr

# TODO: Remove sudo package in proc image, use /usr/bin/gosu instead for better security
RUN             NewPackages="bind-tools inotify-tools procps psmisc@testing gettext" && \
                apk update && apk add --update ${NewPackages} && \
                printf "\\n%s \\n%s \\n%s \\n%s \\n" "# Build Layer 3 Meta Info" \
                "Package ++: [s6-overlay-${S6VER}] ${NewPackages}" \
                "Maintainer: Howard Mei <howardmei@mubiic.com>" "Build Tag: $BUILDTAG" >> /etc/kube-imagemeta

# Add s6 suite overlay to provide init manager
ADD             https://github.com/mubiic/s6-overlay/releases/download/${S6VER}/s6-overlay-amd64.tar.gz /tmp/s6-overlay.tar.gz
RUN             tar xvfz /tmp/s6-overlay.tar.gz -C / && rm -f /tmp/s6-overlay.tar.gz

# Create a devops user in sudo group and initialize its home directory
RUN             adduser -S -s /bin/bash -g 'DevOps User in Sudo Group' \
                -G sudo -D -h /home/devops devops && \
                cp -f /root/.bashrc /root/.bash_profile /root/.bash_prompt /root/.inputrc \
                      /root/.gitconfig /root/.gitignore /root/.gitattributes /home/devops && \
                chown -R devops:sudo /home/devops && \
                mkdir -p /opt/app && touch /opt/app/.placeholder && chown -R devops:sudo /opt/app


# Set Default User and Wordking Dir. Change to devops during app src mounting,
# use s6-setuidgid devops:sudo to run daemon services in /etc/services.d/*
# Change back to root to run the /init script
USER            root
ENV             HOME            /root
WORKDIR         /opt/app


# Define base volumes as common attach points.
VOLUME          ["/opt/app","/etc/cont-init.d","/etc/cont-finish.d","/etc/services.d"]

# Define the Entry Point and/or Default Command
ENTRYPOINT      ["/init"]
CMD             ["pstop"]
