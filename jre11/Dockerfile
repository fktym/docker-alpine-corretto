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
        java -version

ENV PATH="$PATH:$JAVA_HOME/bin"

# jlink
# https://docs.oracle.com/javase/9/tools/jlink.htm
RUN jlink --compress=2 --no-header-files --no-man-pages --verbose \
        --add-modules=java.base,java.logging,java.xml,jdk.unsupported,java.sql,java.naming,java.desktop,java.management,java.security.jgss,java.instrument,jdk.charsets,jdk.crypto.ec \
        --output=/tmp/jre


# ---------------- Create image -------------------
FROM frolvlad/alpine-glibc:alpine-3.9

COPY --from=builder /tmp/jre /opt/jre

ENV JAVA_HOME="/opt/jre"
ENV PATH="$PATH:$JAVA_HOME/bin"

