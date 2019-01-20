FROM ubuntu:16.04

ENV LS_HOME /var/lib/logstash
ENV LS_HEAP_SIZE "2g"
ENV LS_LOG_DIR /var/log/logstash
ENV LS_LOG_FILE "${LS_LOG_DIR}/$name.log"
ENV LS_CONF_DIR /etc/logstash/conf.d
ENV LS_OPEN_FILES 16384
ENV LS_NICE 19
ENV LS_OPTS ""
ENV program /usr/share/logstash/bin/logstash
ENV args "agent -f ${LS_CONF_DIR} -l ${LS_LOG_FILE} ${LS_OPTS}"
ENV LS_JAVA_OPTS "${LS_JAVA_OPTS} -Djava.io.tmpdir=${LS_HOME}"
ENV HOME ${LS_HOME}
ENV test_args "-t -f ${LS_CONF_DIR} ${LS_OPTS}"
ENV LS_DEB "logstash-6.3.2.deb"
ENV LS_DOWNLOAD_URL "https://artifacts.elastic.co/downloads/logstash/${LS_DEB}"

WORKDIR ${LS_HOME}
RUN apt update && apt install -y openjdk-8-jre-headless wget logrotate

RUN wget ${LS_DOWNLOAD_URL} && dpkg -i ${LS_DEB} && /usr/share/logstash/bin/logstash-plugin install logstash-filter-translate
RUN mkdir ${LS_HOME}/patterns

COPY logstash.conf.dns ${LS_CONF_DIR}/logstash.conf
COPY patterns ${LS_HOME}/patterns/
COPY logstash.yml /etc/logstash/

RUN ${program} ${test_args}

ENTRYPOINT [ "/bin/bash", "/usr/share/logstash/bin/logstash", "-f", "/etc/logstash/conf.d", "-l", "/var/log/logstash/$name.log" ]
