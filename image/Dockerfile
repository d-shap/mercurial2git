FROM        debian:jessie
MAINTAINER  Dmitry Shapovalov

COPY        . /tmp

# Install packages
RUN         apt-get update && \
            apt-get install --assume-yes \
                git \
                mercurial \
                mercurial-git && \
            apt-get clean

# Create user and group
RUN         useradd -m repoowner

# Configure mercurial
RUN         mv /tmp/hgrc /etc/mercurial

# Configure helper scripts
RUN         mv /tmp/gitUpdate /home/repoowner && \
            chown repoowner:repoowner /home/repoowner/gitUpdate && \
            chmod a+x /home/repoowner/gitUpdate

USER        repoowner

ENTRYPOINT  ["/home/repoowner/gitUpdate"]
