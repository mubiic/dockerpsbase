FROM            mubiic/ubuntubash:15.04
MAINTAINER      Howard Mei      <howardmei@mubiic.com>
ENV             KUBECONF        **TBD**
ENV             APPCONF         **TBD**
ENV             S6VER           v1.A2.1
ENV             BUILDTAG        ubuntuproc:15.04/r20150612/sg/A

# Add conf to fix s6 init suite permissions
COPY            etc             /etc
COPY            usr             /usr

# Update the timezone and locale according to the env variables set above
RUN             NewPackages="libapparmor1 cron inotify-tools procps psmisc gettext-base" && DEBIAN_FRONTEND=noninteractive && \
                apt-get-min update && apt-install-min ${NewPackages} && apt-cleanup-min && \
                printf "\\n%s \\n%s \\n%s \\n%s \\n" "# Build Layer 3 Meta Info" \
                "Package ++: [s6-overlay-${S6VER}] ${NewPackages}" \
                "Maintainer: Howard Mei <howardmei@mubiic.com>" "Build Tag: $BUILDTAG" >> /etc/kube-imagemeta


# Add s6suite overlay to provide init manager
ADD             https://github.com/mubiic/s6-overlay/releases/download/${S6VER}/s6-overlay-amd64.tar.gz /tmp/s6-overlay.tar.gz
RUN             tar xvfz /tmp/s6-overlay.tar.gz -C / && rm -f /tmp/s6-overlay.tar.gz


# Create a devops user in sudo group and initialize its home directory
# Remember to use gosu devops:sudo to run daemon processes because sudo is removed here
RUN             adduser --system --shell /bin/bash --gecos 'DevOps User in Sudo Group' \
                --ingroup sudo --disabled-password --home /home/devops devops && \
                cp -f /root/.bashrc /root/.bash_profile /root/.bash_prompt /root/.inputrc \
                      /root/.gitconfig /root/.gitignore /root/.gitattributes /home/devops && \
                chown -R devops:sudo /home/devops && \
                mkdir -p /opt/app && touch /opt/app/.placeholder && chown -R devops:sudo /opt/app && \
                unset DEBIAN_FRONTEND && env --unset=DEBIAN_FRONTEND

# Set Default User and Wordking Dir
USER            root
ENV             HOME            /root
WORKDIR         /opt/app

# Define base volume as a common attach point.
VOLUME          ["/opt/app","/etc/cont-init.d","/etc/cont-finish.d","/etc/services.d"]

ENTRYPOINT      ["/init"]
CMD             ["pstop"]
