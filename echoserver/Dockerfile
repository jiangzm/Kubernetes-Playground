FROM nginx:1.15-alpine

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories
RUN apk update && apk add --no-cache openssl nginx-mod-http-lua nginx-mod-http-lua-upstream

RUN mkdir -p /run/nginx
ADD nginx.conf /etc/nginx/nginx.conf
ADD template.lua /usr/local/share/lua/5.1/
ADD README.md README.md
ADD run.sh /usr/local/bin/run.sh
RUN chmod +x /usr/local/bin/run.sh

ENTRYPOINT ["/usr/local/bin/run.sh"]
