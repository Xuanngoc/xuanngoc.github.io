FROM jekyll/jekyll

WORKDIR /usr/src/app

COPY . /usr/src/app
RUN echo $UID

ENV JEKYLL_UID: 1009
ENV JEKYLL_GID: 1009
RUN echo $UID

RUN bundle install

CMD ["sudo", "bundle", "exec", "jekyll", "serve"]
