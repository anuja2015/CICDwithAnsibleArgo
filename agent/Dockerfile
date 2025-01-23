# Start with Maven image with JDK 17
FROM openjdk:17-slim

#Set env variables for Maven version
ENV MAVEN_VERSION=3.8.8
ENV MAVEN_HOME=/opt/maven
ENV PATH=$MAVEN_HOME/bin:$PATH

# Install Ansible and AZURE CLI
RUN  apt update && apt upgrade -y && apt install -y python3 python3-pip \
    && apt install -y ansible curl \
    && curl -sL https://aka.ms/InstallAzureCLIDeb | bash \
    && pip install ansible[azure]
#
RUN pip install kubernetes

# Install Docker CLI and Maven
RUN apt update && apt install -y \
    docker.io \
    git \
    && curl -fsSL https://archive.apache.org/dist/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz | tar -xz -C /opt && \
    ln -s /opt/apache-maven-$MAVEN_VERSION /opt/maven \
    && apt clean \
    && rm -rf /var/lib/apt/lists/*

# Install kubectl (for managing AKS)
RUN curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl \
    && chmod +x ./kubectl \
    && mv ./kubectl /usr/local/bin/

# Install the necessary Ansible collections
RUN ansible-galaxy collection install azure.azcollection kubernetes.core

# Set working directory
WORKDIR /app

# Ensure Maven and Docker are available
RUN mvn -version && docker --version \
    && ansible --version && kubectl version --client && \
    az version

# Default command (can be overridden)
CMD ["bash"]

