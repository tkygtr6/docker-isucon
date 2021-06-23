FROM ruby:2.7

RUN apt update && apt install -y vim git curl wget traceroute dnsutils net-tools htop less dstat gcc
RUN apt install -y nginx

ARG GIT_URL=https://github.com/isucon/isucon7-qualify.git

RUN git clone $GIT_URL /home/isucon/isubata
WORKDIR /home/isucon/isubata/webapp/ruby
RUN git checkout tkygtr6-ruby
RUN bundle update --bundler
RUN bundle install --path vendor/bundle

# Configure nginx
RUN useradd --no-create-home nginx
RUN rm -rf /etc/nginx/nginx.conf
RUN rm -rf /etc/nginx/conf.d
RUN ln -s /home/isucon/isubata/files/web/nginx.conf /etc/nginx/nginx.conf
RUN ln -s /home/isucon/isubata/files/web/conf.d /etc/nginx/conf.d

EXPOSE 5000

ENTRYPOINT ["nginx"]
