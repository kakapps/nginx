ARG BASETAG=1.18.0

FROM nginx:${BASETAG}

COPY --from=hairyhenderson/gomplate:v3.7.0-slim /gomplate /bin/gomplate

ENV NGINX_VHOST_PRESET="html"

COPY assets/ /assets

WORKDIR /var/www/html
EXPOSE 80 443

ENTRYPOINT ["/assets/bin/docker-entrypoint.sh"]
CMD ["nginx", "-g", "daemon off;"]