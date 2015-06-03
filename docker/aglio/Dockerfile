FROM dockerfile/nodejs

RUN npm install -g aglio

RUN groupadd aglio \
 && useradd aglio -g aglio

USER aglio
