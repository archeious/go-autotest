FROM centos:centos7
MAINTAINER Jeff Smith

# Install NGINX
RUN yum -y update; yum clean all
RUN yum -y install http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm; yum -y makecache
RUN yum -y install nginx-1.8.0

# Remove default files we don't need
RUN rm /etc/nginx/conf.d/default.conf
RUN rm /etc/nginx/conf.d/example_ssl.conf

ENV cachebust=1434316727
COPY conf/jenkins.conf /etc/nginx/conf.d/jenkins.conf
COPY conf/ares.conf /etc/nginx/conf.d/ares.conf
COPY conf/passwords /etc/nginx/passwords
COPY conf/nginx.conf /etc/nginx/nginx.conf

EXPOSE 80

CMD ["nginx"]
