FROM golang:latest

MAINTAINER Nic Jackson <jackson.nic@gmail.com>

RUN apt-get update && \
    apt-get install -y git && \
    apt-get install -y ruby && \
    apt-get install -y ruby-dev && \
    apt-get install -y zlib1g-dev && \
    apt-get install -y build-essential && \
    apt-get clean && \ 
    rm -rf /var/lib/apt/lists/*

RUN gem install bundle

ADD ./Gemfile Gemfile

RUN bundle
