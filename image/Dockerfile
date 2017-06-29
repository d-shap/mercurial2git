FROM        debian:jessie
MAINTAINER  Dmitry Shapovalov

RUN         apt-get update && \
            apt-get install --assume-yes git mercurial mercurial-git && \
                useradd -m repoowner

COPY        hgrc /etc/mercurial
COPY        gitUpdate /home/repoowner

RUN         chown repoowner:repoowner /home/repoowner/gitUpdate && \
            chmod a+x /home/repoowner/gitUpdate

USER        repoowner

ENTRYPOINT  ["/home/repoowner/gitUpdate"]
