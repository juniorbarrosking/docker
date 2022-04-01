FROM mysql:5.7

COPY schema.sql /docker-entrypoint-init.db/

EXPOSE 3306

VOLUME ["/backup/"]