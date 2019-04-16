FROM fktym/alpine-sdkman:alpine3.9 AS builder

ARG JAVA_VERSION=11.0.2-amzn
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

COPY --from=builder /root/.sdkman/candidates/java/current /opt/jdk

ENV JAVA_HOME="/opt/jdk"
ENV PATH="$PATH:$JAVA_HOME/bin"

