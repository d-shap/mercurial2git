FROM        debian:stretch
MAINTAINER  Dmitry Shapovalov

ENV         RUN_USER repoowner
ENV         RUN_GROUP repoowner

COPY        . /tmp

# Install common packages and configure environment
RUN         apt-get update && \
            apt-get install --assume-yes \
                apt-utils \
                build-essential \
                git \
                mercurial \
                mercurial-git \
                software-properties-common \
                unzip \
                vim \
                wget && \
            apt-get clean

# Create user and group
RUN         groupadd $RUN_GROUP && \
            useradd -m -g $RUN_GROUP -d /home/repoowner $RUN_USER

# Configure mercurial
RUN         mv /tmp/hgrc /etc/mercurial

# Configure helper scripts
RUN         mv /tmp/gitUpdate /home/repoowner && \
            chown $RUN_USER:$RUN_GROUP /home/repoowner/gitUpdate && \
            chmod 755 /home/repoowner/gitUpdate

USER        $RUN_USER

ENTRYPOINT  ["/home/repoowner/gitUpdate"]
