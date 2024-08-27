FROM python:slim as python
RUN pip3 install anshitsu
RUN python3 svg.py
RUN python3 grayscaling.py

FROM ruby:slim as base
ENV VOLTA_HOME=/usr/local
RUN apt update && \
    apt install -y autoconf \ 
                       bison \
                       build-essential \
                       libssl-dev \
                       libyaml-dev \
                       libreadline6-dev \
                       zlib1g-dev \
                       libncurses5-dev \
                       libffi-dev \
                       libgdbm6 \
                       libgdbm-dev \
                       libdb-dev \
                       locales \
                       git-core \
                       zip \
                       unzip \
                       fontconfig \
                       apt-utils \
                       bash \
                       curl \
                       sudo \
                       librsvg2-bin \
                       libssl-dev \
                       libreadline-dev \
                       sudo \
                       cron \
                       libcairo2-dev \
                       libffi-dev \
                       zlib1g-dev \
                       libatk-bridge2.0-0 \
                       libgtk-3-0 \
                       libasound2 \
                       pandoc \
                       mecab \
                       mecab-ipadic-utf8 \
                       libmecab-dev \
                       libgbm-dev \
                       file \
                       xz-utils \
                       poppler-data \
                       graphviz \
                       poppler-utils && \
                       apt clean

RUN curl https://get.volta.sh | bash &&\
    volta install node@lts pnpm@latest

ENV PNPM_HOME="/pnpm"
ENV PATH="$PNPM_HOME:$PATH"
RUN corepack enable

RUN pnpm install -g @vivliostyle/cli

RUN git clone https://github.com/neologd/mecab-ipadic-neologd.git && \
    cd mecab-ipadic-neologd && \
    sudo bin/install-mecab-ipadic-neologd -y && \
    sudo echo dicdir = /usr/lib/x86_64-linux-gnu/mecab/dic/mecab-ipadic-neologd > /etc/mecabrc

RUN echo 'gem: --no-rdoc --no-ri' >> /.gemrc && \
    gem update && \
    bundle install

RUN pnpm install
RUN pnpm run style
ENTRYPOINT ["./entrypoint.sh"]

