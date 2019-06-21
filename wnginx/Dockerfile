FROM nginx
MAINTAINER liuwanglei/key&can
EXPOSE 80
RUN apt-get update && \
    apt-get install -y vim\
    keepalived
CMD ["/usr/sbin/nginx","-g","daemon off;"]
