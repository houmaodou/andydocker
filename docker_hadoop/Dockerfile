# 选择一个已有的os镜像作为基础
FROM centos

# 镜像的作者
MAINTAINER andy_hou

# 安装openssh-server和sudo软件包，并且将sshd的UsePAM参数设置成no
RUN yum install -y openssh-server sudo
RUN sed -i 's/UsePAM yes/UsePAM no/g' /etc/ssh/sshd_config
#安装openssh-clients
RUN yum  install -y openssh-clients

# 添加测试用户root，密码root，并且将此用户添加到sudoers里
RUN echo "root:root!" | chpasswd
RUN echo "root   ALL=(ALL)       ALL" >> /etc/sudoers
# 下面这两句比较特殊，在centos6上必须要有，否则创建出来的容器sshd不能登录
RUN ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key
RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key

# 启动sshd服务并且暴露22端口
RUN mkdir /var/run/sshd
EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]

# -----------------Install JDK  ---------------------------
# Download jdk and save it at the same location as Dockerfile.
ADD jdk-8u66-linux-x64.gz /usr/local/
RUN mv /usr/local/jdk1.8.0_66 /usr/local/jdk1.8
ENV JAVA_HOME /usr/local/jdk1.8
ENV PATH $JAVA_HOME/bin:$PATH
# -----------------Install Hadoop  ---------------------------
# Download Hadoop installation package at the same location as Dockerfile
ADD hadoop-2.6.4.tar.gz /usr/local
RUN mv /usr/local/hadoop-2.6.4 /usr/local/hadoop
ENV HADOOP_HOME /usr/local/hadoop
ENV PATH $HADOOP_HOME/bin:$PATH

