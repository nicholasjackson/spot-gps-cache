FROM ubuntu:14.04

EXPOSE 8001

RUN mkdir /spot-gps-cache

ADD ./server /spot-gps-cache/server

RUN chmod 755 /spot-gps-cache/server

CMD ["/spot-gps-cache/server"]
