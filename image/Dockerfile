FROM        debian:buster
MAINTAINER  Dmitry Shapovalov

ENV         RUN_USER=repoowner \
            RUN_GROUP=repoowner

COPY        . /tmp

# Install common packages and configure environment
RUN         apt-get update && \
            apt-get install --assume-yes \
                apt-utils \
                build-essential \
                cron \
                net-tools \
                procps \
                software-properties-common \
                vim \
                wget && \
            apt-get update && \
            apt-get install --assume-yes \
                git \
                mercurial \
                mercurial-git && \
            apt-get clean

# Create user and group
RUN         groupadd $RUN_GROUP && \
            useradd -m -g $RUN_GROUP -d /home/repoowner $RUN_USER

# Configure app
RUN         mv /tmp/hgrc /etc/mercurial

# Configure helper scripts
RUN         mv /tmp/gitUpdate /home/repoowner && \
            chown $RUN_USER:$RUN_GROUP /home/repoowner/gitUpdate && \
            chmod 755 /home/repoowner/gitUpdate

USER        $RUN_USER

ENTRYPOINT  ["/home/repoowner/gitUpdate"]
