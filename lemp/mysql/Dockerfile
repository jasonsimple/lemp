FROM docker.io/centos:centos6
MAINTAINER The fifth group
RUN yum -y install gcc gcc-c++ ncurses-devel python-setuptools openssh-server
RUN easy_install supervisor
RUN useradd admin
RUN echo "admin:admin" | chpasswd
RUN ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key
RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
RUN mkdir -pv /var/run/sshd
RUN mkdir -pv /home/admin/.ssh
RUN sed -i 's/session required pam_loginuid.so/#session required pam_loginuid.so/g' /etc/pam.d/sshd
ADD authorized_keys /home/admin/.ssh/authorized_keys
ADD cmake-2.8.12.tar.gz /usr/local/
WORKDIR /usr/local/cmake-2.8.12
RUN ./configure && gmake && gmake install
ADD mysql-5.5.38.tar.gz /usr/local/
WORKDIR /usr/local/mysql-5.5.38
RUN cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci -DWITH_EXTRA_CHARSETS=all -DSYSCONFDIR=/etc
RUN make && make install
RUN /bin/cp support-files/my-medium.cnf /etc/my.cnf
RUN /bin/cp support-files/mysql.server /etc/rc.d/init.d/mysqld
RUN chmod +x /etc/rc.d/init.d/mysqld && chkconfig --add mysqld && chkconfig mysqld on
RUN echo "PATH=$PATH:/usr/local/mysql/bin" >> /etc/profile &&  source /etc/profile
RUN ln -sv /usr/local/mysql/bin/ /usr/bin/mysql
RUN groupadd mysql
RUN useradd -M -s /sbin/nologin mysql -g mysql
RUN chown -R mysql:mysql /usr/local/mysql
RUN /usr/local/mysql/scripts/mysql_install_db --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data --user=mysql > /dev/nul
COPY supervisord.conf /etc/supervisord.conf
EXPOSE 3306
EXPOSE 22
CMD ["/usr/bin/supervisord","-c","/etc/supervisord.conf"]

