FROM fktym/alpine-sdkman:alpine3.9 AS builder

ARG JAVA_VERSION=8.0.212-amzn
ENV JAVA_HOME=/root/.sdkman/candidates/java/$JAVA_VERSION

SHELL ["/bin/bash", "-c"]
RUN . ~/.sdkman/bin/sdkman-init.sh && \
	sdk update && \
	sdk install java $JAVA_VERSION && \
	sdk flush broadcast && \
	sdk flush archives && \
	sdk flush temp && \
        find $JAVA_HOME -name "*src.zip" -exec rm {} \; && \
        java -version


# ---------------- Create image -------------------
FROM frolvlad/alpine-glibc:alpine-3.9

COPY --from=builder /root/.sdkman/candidates/java/current/jre /opt/jre

ENV JAVA_HOME="/opt/jre"
ENV PATH="$PATH:$JAVA_HOME/bin"

RUN apk --no-cache add tzdata && \
    cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime && \
    echo "Asia/Tokyo" > /etc/timezone && \
    apk del tzdata

