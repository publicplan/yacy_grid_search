FROM gradle:5.4-jdk8-alpine as builder
MAINTAINER publicplan GmbH
LABEL MAINTAINER="publicplan GmbH"

COPY --chown=gradle:gradle ./ /app/yacy_grid_search
WORKDIR /app/yacy_grid_search

RUN gradle build --info && \
    mkdir build/distributions/app && \
    unzip build/distributions/*.zip -d build/distributions/app && \
    mkdir data/search-8800/conf/ -p && \
    mkdir data/aggregation-8800/conf/ -p && \
    cp -rf conf/* data/search-8800/conf/ &&\
    cp -rf conf/* data/aggregation-8800/conf/

FROM openjdk:8-alpine as runner

COPY --from=builder /app/yacy_grid_search/build/distributions/app/* /app
COPY --from=builder /app/yacy_grid_search/data /app/data
# COPY --from=builder /app/yacy_grid_mcp/conf /app/conf

WORKDIR /app
EXPOSE 8800
ENTRYPOINT ["/bin/sh"]
CMD ["bin/yacy_grid_search"]
