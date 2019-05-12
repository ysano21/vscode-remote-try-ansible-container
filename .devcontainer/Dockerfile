FROM python:3

# update all
RUN apt-get update && apt-get install -y --no-install-recommends

# Copy default endpoint specific user settings overrides into container to specify Python path
COPY .devcontainer/settings.vscode.json /root/.vscode-remote/data/Machine/settings.json

# install ansible boto boto3
RUN pip install --upgrade pip

# make work directory
RUN mkdir /workspace
WORKDIR /workspace

# Install Python dependencies from requirements.txt if it exists
COPY .devcontainer/requirements.txt /workspace/
RUN if [ -f "requirements.txt" ]; then pip install -r requirements.txt && rm requirements.txt*; fi

# Clean up
RUN apt-get autoremove -y \
    && apt-get clean -y