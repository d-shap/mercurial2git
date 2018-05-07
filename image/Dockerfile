FROM        debian:jessie
MAINTAINER  Dmitry Shapovalov

ENV         RUN_USER repoowner
ENV         RUN_GROUP repoowner

COPY        . /tmp

# Install packages
RUN         apt-get update && \
            apt-get install --assume-yes \
                git \
                mercurial \
                mercurial-git && \
            apt-get clean

# Create user and group
RUN         groupadd $RUN_GROUP && \
            useradd -m -g $RUN_GROUP $RUN_USER

# Configure mercurial
RUN         mv /tmp/hgrc /etc/mercurial

# Configure helper scripts
RUN         mv /tmp/gitUpdate /home/$RUN_USER && \
            chown $RUN_USER:$RUN_GROUP /home/$RUN_USER/gitUpdate && \
            chmod a+x /home/$RUN_USER/gitUpdate

USER        $RUN_USER

ENTRYPOINT  ["/home/$RUN_USER/gitUpdate"]
