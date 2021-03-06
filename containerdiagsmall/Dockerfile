# docker build -t containerdiagsmall .
# docker run -it containerdiagsmall
#
# Notes:
#   * Base fedora:latest is about 175MB
#   * Tried ubi-minimal which is about 100MB but microdnf is missing many useful packages like fatrace and others 
#   * As of writing this note, this image is about 320MB
#   * gdb adds about 68MB but considered worth it for gcore, etc.
#   * git adds about 41MB so instead we just use wget https://github.com/$GROUP/$REPO/archive/master.zip
#   * Therefore, the rest of the utilities are about 75MB
#   * Deleting files in the parent (e.g. /usr/lib64/python*/__pycache__) isn't useful because it's still in that layer

FROM fedora:latest
LABEL maintainer="kevin.grigorenko@us.ibm.com"

RUN dnf install -y \
        curl \
        fatrace \
        gdb \
        iproute \
        iputils \
        less \
        lsof \
        ltrace \
        ncdu \
        net-tools \
        nmon \
        p7zip \
        procps-ng \
        psmisc \
        sysstat \
        strace \
        tcpdump \
        telnet \
        traceroute \
        unzip \
        util-linux \
        vim \
        wget \
      && \
    dnf clean all && \
    rm -rf \
            /usr/share/vim/*/doc/ \
            /usr/share/vim/*/spell/ \
            /usr/share/vim/*/tutor/

# Update the version echo if needed for things like re-pulling from git
RUN echo "Version 20210202" && \
    get_git() { \
      wget -q -O /tmp/$1_$2_master.zip https://github.com/$1/$2/archive/master.zip; \
      unzip -q /tmp/$1_$2_master.zip -d /opt/; \
      mv /opt/$2-master /opt/$1_$2/; \
      rm /tmp/$1_$2_master.zip; \
    } && \
    get_git kgibm problemdetermination && \
    ln -s /opt/kgibm_problemdetermination/scripts/java/j9/j9javacores.awk /usr/local/bin/ && \
    ln -s /opt/kgibm_problemdetermination/scripts/ihs/ihs_mpmstats.awk /usr/local/bin/ && \
    ln -s /opt/kgibm_problemdetermination/scripts/was/twas_pmi_threadpool.awk /usr/local/bin/

# Defer to the ENTRYPOINT/CMD of Fedora which is bash
