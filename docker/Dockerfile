FROM clojure:lein@sha256:8443576895caae2be34924b8d5eb03ba193b196343346ae26984412914771e4e AS builder

RUN mkdir /build

WORKDIR /build

COPY project.clj /build
COPY src /build/src

RUN lein metajar

FROM gcr.io/personalsdm-216019/java-atomist:0.13.0-20191119164121@sha256:60d43b18bb8d86f2906f8a01a51a95dd4a7115ff35a6c921ef1f5b65783d97bd

MAINTAINER Jim Clark <jim@atomist.com>

RUN mkdir -p /usr/src/app \
    && mkdir -p /usr/src/app/bin \
    && mkdir -p /usr/src/app/lib

WORKDIR /usr/src/app

COPY --from=builder /build/target/lib /usr/src/app/lib

COPY --from=builder /build/target/metajar/service.jar /usr/src/app/

CMD ["-Djava.net.preferIPv4Stack=true", "-jar", "/usr/src/app/service.jar", "-Dclojure.core.async.pool-size=20"]

ENV APP_NAME=service

EXPOSE 4000
