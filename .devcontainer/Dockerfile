FROM centos:8

# update all
RUN dnf install -y openssh-clients python3 python3-pip
# docker client
RUN yum install -y yum-utils \
    && yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
RUN dnf install -y docker-ce-cli
RUN dnf install -y epel-release
RUN dnf install -y git sshpass

# Copy default endpoint specific user settings overrides into container to specify Python path
COPY .devcontainer/settings.vscode.json /root/.vscode-remote/data/Machine/settings.json

# install ansible boto boto3
#RUN pip3 install --upgrade pip3

# make work directory
RUN mkdir /workspace
WORKDIR /workspace

# Install Python dependencies from requirements.txt if it exists
COPY .devcontainer/requirements.txt /workspace/
RUN if [ -f "requirements.txt" ]; then pip3 install -r requirements.txt && rm requirements.txt*; fi

ENV DOCKER_HOST=host.docker.internal:2375

# Clean up
#RUN apt-get autoremove -y \
#    && apt-get clean -y
