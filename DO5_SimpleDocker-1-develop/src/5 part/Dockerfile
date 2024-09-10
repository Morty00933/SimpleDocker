FROM nginx:latest
RUN apt-get update && apt-get install -y \
    build-essential \
    fcgiwrap \
    spawn-fcgi \
    && rm -rf /var/lib/apt/lists/*
COPY ./fastcgi-server /usr/src/fastcgi-server
RUN cd /usr/src/fastcgi-server && make
CMD ["/usr/src/fastcgi-server/fastcgi-server", "8080"]
COPY ./nginx/nginx.conf /etc/nginx/nginx.conf
RUN apt-get clean && rm -rf /var/lib/apt/lists/*
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
