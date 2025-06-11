ARG EDGE_VERSION

FROM ibmhybridcr.azurecr.io/webmethods-edge-runtime:${EDGE_VERSION}

ARG WPM_CRED
ARG GITHUB_CREDS_USR
ARG GITHUB_CREDS_PSW
ARG AGENT_KEY

WORKDIR /opt/softwareag/wpm


ENV PATH=/opt/softwareag/wpm/bin:$PATH
ENV JAVA_CUSTOM_OPTS="-javaagent:/opt/softwareag/instana/instana-javaagent-1.0.0.jar"

RUN mkdir /opt/softwareag/instana; cd /opt/softwareag/instana; \
   curl -o instana-javaagent-1.0.0 https://_:${AGENT_KEY}@artifact-public.instana.io/artifactory/shared/com/instana/instana-javaagent/1.0.0/instana-javaagent-1.0.0.jar

RUN /opt/softwareag/wpm/bin/wpm.sh install -ws https://packages.webmethods.io -wr licensed -d /opt/softwareag/IntegrationServer -j ${WPM_CRED} WmJDBCAdapter
RUN /opt/softwareag/wpm/bin/wpm.sh install -ws https://packages.webmethods.io -wr supported -d /opt/softwareag/IntegrationServer -j ${WPM_CRED} WmE2EMIntegrationAgent
RUN /opt/softwareag/wpm/bin/wpm.sh install -ws https://packages.webmethods.io -wr supported -d /opt/softwareag/IntegrationServer -j ${WPM_CRED} WmStreaming
RUN /opt/softwareag/wpm/bin/wpm.sh install -u ${GITHUB_CREDS_USR} -p ${GITHUB_CREDS_PSW} -r https://github.com/elpinjo -d /opt/softwareag/IntegrationServer OrderEntry
RUN /opt/softwareag/wpm/bin/wpm.sh install -u ${GITHUB_CREDS_USR} -p ${GITHUB_CREDS_PSW} -r https://github.com/elpinjo -d /opt/softwareag/IntegrationServer OrderAnalytics
RUN /opt/softwareag/wpm/bin/wpm.sh install -u ${GITHUB_CREDS_USR} -p ${GITHUB_CREDS_PSW} -r https://github.com/elpinjo -d /opt/softwareag/IntegrationServer NL_MIHE_GitbasedProject.git

COPY mysql-connector-j-9.1.0.jar /opt/softwareag/IntegrationServer/packages/WmJDBCAdapter/code/jars 
#/opt/softwareag/IntegrationServer/lib/jars/custom
COPY postgresql-42.7.6.jar /opt/softwareag/IntegrationServer/packages/WmJDBCAdapter/code/jars
COPY eem.jks /opt/softwareag/common/conf/
WORKDIR /
