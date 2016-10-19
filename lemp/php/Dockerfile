FROM docker.io/centos:centos6
MAINTAINER the fifth groups
RUN yum -y install gd libxml2-devel libjpeg-devel libpng-devel mysql-devel gcc gcc-c++ openssh-server
ADD get-pip.py ./
RUN python get-pip.py 
RUN pip install supervisor
RUN useradd -M -s /sbin/nologin php
RUN echo "root:123123" | chpasswd
RUN ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key
RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
RUN mkdir -pv /var/run/sshd
RUN mkdir -pv /root/.ssh
RUN sed -i 's/session required pam_loginuid.so/#session required pam_loginuid.so/g' /etc/pam.d/sshd
ADD authorized_keys /root/.ssh/authorized_keys
ADD php-5.3.28.tar.gz /usr/local/
WORKDIR /usr/local/php-5.3.28/
RUN /bin/cp /usr/lib64/mysql/libmysqlclient.so.16.0.0 /usr/lib/libmysqlclient.so
RUN ./configure --prefix=/usr/local/php --with-gd --with-zlib --with-mysql --with-mysqli --with-mysql-sock --with-config-file-path=/usr/local/php --enable-mbstring --enable-fpm --with-jpeg-dir=/usr/lib && make && make install
WORKDIR /usr/local/php/etc/
RUN cp php-fpm.conf.default php-fpm.conf
RUN mkdir -pv /var/www/html/webphp
RUN echo "<?phpinfo();?>" > /var/www/html/webphp/index.php
ADD supervisord.conf /etc/supervisord.conf
EXPOSE 22
CMD ["/usr/bin/supervisord","-c", "/etc/supervisord.conf"]

