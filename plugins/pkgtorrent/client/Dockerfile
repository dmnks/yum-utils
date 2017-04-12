FROM centos:7

RUN yum install -y epel-release
RUN yum install -y \
    # Torrent client
    aria2 \
    crudini \
    python-pip \
    # Provides /usr/bin/transmission-show which is useful to inspect .torrent
    # files
    transmission-cli
RUN pip install --upgrade pip && \
    pip install pudb && \
    chmod 777 -R /root

# Install and configure the plugin
RUN ln -s /src/yum-torrent.py \
          /usr/lib/yum-plugins/yum-torrent.py
WORKDIR /etc/yum/pluginconf.d
ADD yum-torrent.conf yum-torrent.conf
RUN crudini --set yum-torrent.conf main client /usr/bin/aria2c && \
    crudini --set yum-torrent.conf main complete "SEED(" && \
    crudini --set yum-torrent.conf \
                  main options "--allow-overwrite=true" \
    # Unlimited download speed would cause occasional freeze on host
    crudini --set yum-torrent.conf \
                  main options "--max-overall-download-limit=1M"
RUN mkdir /var/cache/yum_torrent

# Add our repo and disable everything else
RUN yum-config-manager --disable \* --save
WORKDIR /etc/yum.repos.d
RUN crudini --set torrent.repo torrent name torrent && \
    crudini --set torrent.repo torrent baseurl http://server/yum && \
    crudini --set torrent.repo torrent gpgcheck 0

WORKDIR /root
VOLUME ["/src"]
ENTRYPOINT ["/bin/bash"]
