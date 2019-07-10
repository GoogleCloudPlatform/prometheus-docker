FROM gcr.io/google-appengine/debian9
ENV PROMETHEUS_VERSION 2.11.0

ENV PROMETHEUS_URL="https://github.com/prometheus/prometheus/releases/download/v$PROMETHEUS_VERSION/prometheus-$PROMETHEUS_VERSION.linux-amd64.tar.gz"
ENV PROMETHEUS_SHA256 9705ee80713809b29995e94ce8786a901c046c9b3fd555aac965bf50a28bc2e3

RUN set -x && \
    apt-get update && apt-get install -qq -y wget && \
    mkdir -p /etc/prometheus && \
    mkdir prometheus-files && cd prometheus-files && \
    wget -O prometheus.tar.gz "$PROMETHEUS_URL" && \
    echo "$PROMETHEUS_SHA256 prometheus.tar.gz" | sha256sum -c - && \
    tar xfvz prometheus.tar.gz --strip-components=1 && \
    mv prometheus          /bin/ && \
    mv promtool            /bin/ && \
    mv prometheus.yml      /etc/prometheus/ && \
    mv console_libraries   /usr/share/prometheus/ && \
    mv consoles            /usr/share/prometheus/ && \
    cd - && \
    rm -r prometheus-files && \
    ln -s /usr/share/prometheus/console_libraries /usr/share/prometheus/consoles/ /etc/prometheus && \
    mkdir -p /prometheus && \
    chown -R nobody:nogroup /etc/prometheus /prometheus

USER       nobody
EXPOSE     9090
VOLUME     [ "/prometheus" ]
WORKDIR    /prometheus
ENTRYPOINT [ "/bin/prometheus" ]
CMD        [ "--config.file=/etc/prometheus/prometheus.yml", \
             "--storage.tsdb.path=/prometheus", \
             "--web.console.libraries=/usr/share/prometheus/console_libraries", \
             "--web.console.templates=/usr/share/prometheus/consoles" ]

