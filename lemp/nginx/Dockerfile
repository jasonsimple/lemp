FROM docker.io/centos:centos6
MAINTAINER the fifth groups
RUN yum -y install pcre-devel zlib-devel gcc gcc-c++ openssh-server
ADD get-pip.py ./
RUN python get-pip.py
RUN pip install supervisor
RUN echo "root:123123" | chpasswd
RUN ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key
RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
RUN mkdir -pv /var/run/sshd
RUN mkdir -pv /root/.ssh
RUN sed -i 's/session required pam_loginuid.so/#session required pam_loginuid.so/g' /etc/pam.d/sshd
ADD authorized_keys /root/.ssh/authorized_keys
COPY nginx-1.6.2.tar.gz /usr/local/
RUN useradd nginx -M -s /sbin/nologin
WORKDIR /usr/local/
RUN tar zxf /usr/local/nginx-1.6.2.tar.gz
WORKDIR /usr/local/nginx-1.6.2
RUN ./configure --prefix=/usr/local/nginx --user=nginx --group=nginx && make && make install
RUN ln -s /usr/local/nginx/sbin/nginx /usr/local/sbin
ADD supervisord.conf /etc/supervisord.conf
EXPOSE 80 22
CMD ["/usr/bin/supervisord","-c", "/etc/supervisord.conf"]
