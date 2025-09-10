# -------------------------------------------------------------------
#  Recommended: Simple, official image-based build
# -------------------------------------------------------------------
FROM otel/opentelemetry-collector-contrib:0.111.0

ARG JMX_METRICS_VERSION=1.36.0

# Create a directory and copy the JMX jar as root
USER root
RUN mkdir -p /otel-collector-jars
ADD https://github.com/open-telemetry/opentelemetry-java-contrib/releases/download/v${JMX_METRICS_VERSION}/opentelemetry-jmx-metrics.jar /otel-collector-jars/opentelemetry-jmx-metrics.jar
RUN chmod 644 /otel-collector-jars/opentelemetry-jmx-metrics.jar

# Switch back to the non-root user for runtime
USER 10001

# Copy your existing config
COPY config.yml /etc/otelcol/config.yaml

# Expose ports
EXPOSE 9464 4318 8888

ENTRYPOINT ["/otelcol-contrib"]
CMD ["--config", "/etc/otelcol/config.yaml"]



# -------------------------------------------------------------------
#  Alternative: Custom manual build (commented out)
# -------------------------------------------------------------------
# FROM eclipse-temurin:17-jre
#
# ARG OTELCOL_VERSION=0.111.0
# ARG JMX_METRICS_VERSION=1.36.0
#
# # Install curl & download otelcol-contrib binary
# RUN apt-get update && apt-get install -y curl ca-certificates && rm -rf /var/lib/apt/lists/*
# RUN mkdir -p /otel && \
#     curl -L -o /tmp/otelcol.tar.gz \
#     https://github.com/open-telemetry/opentelemetry-collector-releases/releases/download/v${OTELCOL_VERSION}/otelcol-contrib_${OTELCOL_VERSION}_linux_amd64.tar.gz \
#     && tar -xzf /tmp/otelcol.tar.gz -C /otel \
#     && rm /tmp/otelcol.tar.gz
#
# # Grab the JMX metrics jar
# RUN mkdir -p /opt/otel/jmx && \
#     curl -L -o /opt/otel/jmx/opentelemetry-jmx-metrics.jar \
#     https://github.com/open-telemetry/opentelemetry-java-contrib/releases/download/v${JMX_METRICS_VERSION}/opentelemetry-jmx-metrics.jar
#
# COPY otel-collector/config.yml /etc/otelcol/config.yaml
#
# EXPOSE 9464 4318 8888
# ENTRYPOINT ["/otel/otelcol-contrib"]
# CMD ["--config", "/etc/otelcol/config.yaml"]
