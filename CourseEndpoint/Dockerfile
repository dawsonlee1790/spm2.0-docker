# DO NOT ALTER OR REMOVE COPYRIGHT NOTICES OR THIS HEADER.
# 
# Copyright (c) 2017 Oracle and/or its affiliates. All rights reserved.
# 
# GlassFish on Docker with Oracle Linux and OpenJDK
FROM oraclelinux:7-slim

# Maintainer
MAINTAINER Arindam Bandyopadhyay<arindam.bandyopadhyay@oracle.com>

# Set environment variables and default password for user 'admin'
ENV GLASSFISH_PKG=glassfish-4.1.2.zip \
    GLASSFISH_URL=https://download.oracle.com/glassfish/4.1.2/release/glassfish-4.1.2.zip \
    GLASSFISH_HOME=/glassfish4 \
    MD5=9a37928ea6e54334101b8a89e03a8d7d \
    PATH=$PATH:/glassfish4/bin \
    JAVA_HOME=/usr/lib/jvm/java-openjdk 
      

# Install packages, download and extract GlassFish
# Setup password file
# Enable DAS
RUN yum -y install unzip java-1.8.0-openjdk-1.8.0.171-8.b10.el7_5 && \
    curl -O $GLASSFISH_URL && \
    echo "$MD5 *$GLASSFISH_PKG" | md5sum -c - && \
    unzip -o $GLASSFISH_PKG && \
    rm -f $GLASSFISH_PKG && \
    yum -y remove unzip && \
    rm -rf /var/cache/yum  && \
    curl http://repo1.maven.org/maven2/mysql/mysql-connector-java/5.1.34/mysql-connector-java-5.1.34.jar -o glassfish4/glassfish/lib/mysql-connector-java-5.1.34.jar

# COPY glassfish4 /glassfish4
COPY CourseEndpoint.war glassfish4/glassfish/domains/domain1/autodeploy/CourseEndpoint.war
COPY domain.xml /glassfish4/glassfish/domains/domain1/config/domain.xml
COPY org.eclipse.persistence.moxy.jar glassfish4/glassfish/modules/org.eclipse.persistence.moxy.jar
COPY docker-entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]

 


# Ports being exposed
EXPOSE 4848 8080 8181

# Start asadmin console and the domain
CMD ["asadmin", "start-domain", "-v"]
