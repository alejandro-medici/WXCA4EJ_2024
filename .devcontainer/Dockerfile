#######################################################################################
##     The Base image with usable tools
#######################################################################################

FROM registry.access.redhat.com/ubi8/ubi-minimal:latest AS osbase

LABEL maintainer="Alejandro Medici <alejandro.medici@bibm.com>"

# Versions of Maven and Java 21
ENV MAVEN_VERSION=3.9.10
ENV SPRINGBOOT_VERSION=3.3.3
ENV JAVA_VERSION_ORIGIN=1.8.0
ENV JAVA_VERSION_DEST=21

RUN microdnf update && \
    microdnf -y install wget tar gzip git unzip which procps-ng && \
    microdnf clean all && rm -rf /var/cache/microdnf

#installing Java source and dest version
RUN echo "** Installing Java ** "
#java-${JAVA_VERSION_ORIGIN}-openjdk-devel
RUN INSTALL_PKGS=" java-${JAVA_VERSION_DEST}-openjdk-devel glibc-langpack-en findutils nss_wrapper" && \
    microdnf -y --setopt=tsflags=nodocs --setopt=install_weak_deps=0 install $INSTALL_PKGS && \
    microdnf -y clean all --enablerepo='*'
#RUN yum install -y gcc-c++ make 


# Download and decompress maven
RUN echo "** Installing Maven ** "
RUN mkdir -p /tmp/build/maven && \
    cd /tmp/build/maven && \
    wget -O apache-maven-${MAVEN_VERSION}.tar.gz https://dlcdn.apache.org/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.tar.gz && \
    tar -zxf apache-maven-${MAVEN_VERSION}.tar.gz && \
    mv apache-maven-${MAVEN_VERSION} /usr/bin/

ENV M2_HOME=/usr/bin/apache-maven-${MAVEN_VERSION}
ENV M2=$M2_HOME/bin
ENV PATH=$M2:$PATH

# Download and decompress srringBoot
RUN mkdir -p /tmp/build/springboot && \
    cd /tmp/build/springboot && \
    wget -O springboot-${SPRINGBOOT_VERSION}.bin.zip https://repo.maven.apache.org/maven2/org/springframework/boot/spring-boot-cli/${SPRINGBOOT_VERSION}/spring-boot-cli-${SPRINGBOOT_VERSION}-bin.zip && \
    unzip springboot-${SPRINGBOOT_VERSION}.bin.zip && \
    mv spring-${SPRINGBOOT_VERSION} /usr/bin/ 

ENV SPRINGBOOT_HOME=/usr/bin/springboot-${SPRINGBOOT_VERSION}
ENV SPRINGBOOT=$SPRINGBOOT_HOME/bin
ENV PATH=$SPRINGBOOT:$PATH

RUN echo export JAVA_HOME=$(readlink -nf $(which java) | xargs dirname | xargs dirname) >> $HOME/.bashrc 

COPY ./ /scripts

WORKDIR /scripts

#RUN pip install -r requirements.txt

CMD ["/bin/bash"]