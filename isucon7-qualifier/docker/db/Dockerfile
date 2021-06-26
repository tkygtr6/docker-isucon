FROM golang:1.9 AS build-dev

ARG GIT_URL=https://github.com/tkygtr6/isucon7-qualify.git

RUN \
  git clone $GIT_URL /home/isucon/isubata && \
  cd /home/isucon/isubata && \
  git checkout tkygtr6-ruby && \
  cd /home/isucon/isubata/bench && \
  go get github.com/constabulary/gb/... && \
  gb vendor restore && \
  make && \
  bin/gen-initial-dataset

FROM --platform=linux/amd64 mysql:5.7

ARG GIT_URL=https://github.com/tkygtr6/isucon7-qualify.git

RUN apt update && apt install -y vim git curl wget traceroute dnsutils net-tools htop less dstat gcc

RUN git clone $GIT_URL /home/isucon/isubata && \
    cd /home/isucon/isubata && \
    git checkout tkygtr6-ruby

ENV MYSQL_ALLOW_EMPTY_PASSWORD=yes MYSQL_DATABASE=isubata MYSQL_USER=isucon MYSQL_PASSWORD=isucon

COPY --from=build-dev /home/isucon/isubata/db/isubata.sql /docker-entrypoint-initdb.d/01_isubata.sql
COPY --from=build-dev /home/isucon/isubata/bench/isucon7q-initial-dataset.sql.gz /docker-entrypoint-initdb.d/isucon7q-initial-dataset.sql.gz

RUN rm /etc/mysql/mysql.conf.d/mysqld.cnf
RUN ln -s /home/isucon/isubata/files/db/mysqld.cnf /etc/mysql/mysql.conf.d/mysqld.cnf

CMD ["--character-set-server=utf8mb4"]
