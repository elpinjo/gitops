ARG EDGE_VERSION

FROM iwhicr.azurecr.io/webmethods-edge-runtime:${EDGE_VERSION}

ARG WPM_CRED
ARG GITHUB_CREDS_USR
ARG GITHUB_CREDS_PSW
ARG AGENT_KEY

WORKDIR /opt/webmethods/wpm


ENV PATH=/opt/webmethods/wpm/bin:$PATH

RUN mkdir /opt/webmethods/instana; cd /opt/webmethods/instana; \
   curl -o instana-javaagent-1.0.0.jar https://_:${AGENT_KEY}@artifact-public.instana.io/artifactory/shared/com/instana/instana-javaagent/1.0.0/instana-javaagent-1.0.0.jar

RUN /opt/webmethods/wpm/bin/wpm.sh install -ws https://packages.webmethods.io -wr licensed -d /opt/webmethods/IntegrationServer -j ${WPM_CRED} WmJDBCAdapter
RUN /opt/webmethods/wpm/bin/wpm.sh install -ws https://packages.webmethods.io -wr supported -d /opt/webmethods/IntegrationServer -j ${WPM_CRED} WmE2EMIntegrationAgent
RUN /opt/webmethods/wpm/bin/wpm.sh install -ws https://packages.webmethods.io -wr supported -d /opt/webmethods/IntegrationServer -j ${WPM_CRED} WmStreaming
RUN /opt/webmethods/wpm/bin/wpm.sh install -u ${GITHUB_CREDS_USR} -p ${GITHUB_CREDS_PSW} -r https://github.com/elpinjo -d /opt/webmethods/IntegrationServer OrderEntry
RUN /opt/webmethods/wpm/bin/wpm.sh install -u ${GITHUB_CREDS_USR} -p ${GITHUB_CREDS_PSW} -r https://github.com/elpinjo -d /opt/webmethods/IntegrationServer OrderAnalytics
RUN /opt/webmethods/wpm/bin/wpm.sh install -u ${GITHUB_CREDS_USR} -p ${GITHUB_CREDS_PSW} -r https://github.com/elpinjo -d /opt/webmethods/IntegrationServer NL_MIHE_OrderManagementProject

ADD --chown=1724:1724 https://jdbc.postgresql.org/download/postgresql-42.7.8.jar /opt/webmethods/IntegrationServer/lib/jars/custom/
ADD --chown=1724:1724 eem.jks /opt/webmethods/common/conf/
WORKDIR /
