FROM openjdk:11.0.8-jdk


## add mysql repo

RUN set -ex; \
# gpg: key 5072E1F5: public key "MySQL Release Engineering <mysql-build@oss.oracle.com>" imported
	key='A4A9406876FCBD3C456770C88C718D3B5072E1F5'; \
	export GNUPGHOME="$(mktemp -d)"; \
	gpg --batch --keyserver ha.pool.sks-keyservers.net --recv-keys "$key"; \
	gpg --batch --export "$key" > /etc/apt/trusted.gpg.d/mysql.gpg; \
	gpgconf --kill all; \
	rm -rf "$GNUPGHOME"; \
	apt-key list > /dev/null

ENV MYSQL_MAJOR 5.7
RUN echo "deb http://repo.mysql.com/apt/debian/ buster mysql-${MYSQL_MAJOR}" > /etc/apt/sources.list.d/mysql.list



RUN apt-get update && apt-get install -y --no-install-recommends \
    pwgen \
    openssl \
    perl \
    xz-utils \
    net-tools \
    apache2-utils \
    telnet \
    dnsutils \
    redis-tools \
    jq \
    vim \
    mysql-client \
    libhttp-date-perl  libfuse-perl  libxml-simple-perl  libdigest-hmac-perl \
    && rm -rf /var/lib/apt/lists/*

RUN wget -qO /usr/bin/kubectl \
    "http://mirror.azure.cn/kubernetes/kubectl/$(curl -sSL http://mirror.azure.cn/kubernetes/kubectl/stable.txt)/bin/linux/amd64/kubectl" && \
    chmod a+x /usr/bin/kubectl && \
    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
    echo "Asia/Shanghai" > /etc/timezone


ENV GROOVY_HOME /opt/groovy
ENV GROOVY_VERSION 3.0.5

RUN set -o errexit -o nounset \
    && echo "Downloading Groovy" \
    && wget --no-verbose --output-document=groovy.zip "https://archive.apache.org/dist/groovy/${GROOVY_VERSION}/distribution/apache-groovy-binary-${GROOVY_VERSION}.zip" \
    && echo "Installing Groovy" \
    && unzip groovy.zip \
    && rm groovy.zip \
    && mv "groovy-${GROOVY_VERSION}" "${GROOVY_HOME}/" \
    && ln --symbolic "${GROOVY_HOME}/bin/grape" /usr/bin/grape \
    && ln --symbolic "${GROOVY_HOME}/bin/groovy" /usr/bin/groovy \
    && ln --symbolic "${GROOVY_HOME}/bin/groovyc" /usr/bin/groovyc \
    && ln --symbolic "${GROOVY_HOME}/bin/groovyConsole" /usr/bin/groovyConsole \
    && ln --symbolic "${GROOVY_HOME}/bin/groovydoc" /usr/bin/groovydoc \
    && ln --symbolic "${GROOVY_HOME}/bin/groovysh" /usr/bin/groovysh \
    && ln --symbolic "${GROOVY_HOME}/bin/java2groovy" /usr/bin/java2groovy \
    && echo "Editing startGroovy to include java.xml.bind module" \
    && sed --in-place 's|startGroovy ( ) {|startGroovy ( ) {\n    JAVA_OPTS="$JAVA_OPTS --add-modules=ALL-SYSTEM"|' "${GROOVY_HOME}/bin/startGroovy"

COPY ossfs /usr/local/bin/ossfs

ENTRYPOINT [""]
