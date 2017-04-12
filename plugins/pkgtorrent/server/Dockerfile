FROM centos:7

RUN yum install -y epel-release
RUN yum install -y \
    crudini \
    httpd \
    mod_wsgi \
    python-pip
RUN pip install --upgrade pip && \
    pip install pudb && \
    chmod 777 -R /root

# Install and configure the service
RUN ln -s /src/pkgtorrent-service.py \
          /var/www/cgi-bin/torrent-service.py
RUN echo "ServerName server:80" >> /etc/httpd/conf/httpd.conf
WORKDIR /etc/httpd/conf.d
RUN echo "<Directory \"/var/www/cgi-bin\">" > symlinks.conf && \
    echo "Options FollowSymLinks" >> symlinks.conf && \
    echo "</Directory>" >> symlinks.conf
ADD pkgtorrent-service.conf pkgtorrent-service.conf
RUN mkdir /var/cache/torrent_service \
          /var/lib/torrent_service && \
    chown apache: /var/cache/torrent_service

# Add our tracker
RUN echo "http://tracker:6881/announce" > /var/lib/torrent_service/trackers

WORKDIR /root
VOLUME ["/src", "/var/www/html/yum"]
ENTRYPOINT ["/bin/bash"]
