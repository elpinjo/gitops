ARG EDGE_VERSION

FROM ibmhybridcr.azurecr.io/webmethods-edge-runtime:${EDGE_VERSION}

ARG WPM_CRED
ARG GITHUB_CREDS_USR
ARG GITHUB_CREDS_PSW

WORKDIR /opt/softwareag/wpm


ENV PATH=/opt/softwareag/wpm/bin:$PATH

RUN /opt/softwareag/wpm/bin/wpm.sh install -ws https://packages.webmethods.io -wr licensed -d /opt/softwareag/IntegrationServer -j ${WPM_CRED} WmJDBCAdapter
RUN /opt/softwareag/wpm/bin/wpm.sh install -ws https://packages.webmethods.io -wr supported -d /opt/softwareag/IntegrationServer -j ${WPM_CRED} WmE2EMIntegrationAgent
RUN /opt/softwareag/wpm/bin/wpm.sh install -u ${GITHUB_CREDS_USR} -p ${GITHUB_CREDS_PSW} -r https://github.com/elpinjo -d /opt/softwareag/IntegrationServer OrderEntry
RUN /opt/softwareag/wpm/bin/wpm.sh install -u ${GITHUB_CREDS_USR} -p ${GITHUB_CREDS_PSW} -r https://github.com/elpinjo -d /opt/softwareag/IntegrationServer OrderAnalytics

COPY mysql-connector-j-9.1.0.jar /opt/softwareag/IntegrationServer/packages/WmJDBCAdapter/code/jars
COPY eem.jks /opt/softwareag/common/config/
WORKDIR /