# module_6_exercise_9

#!/bin/bash

# Define variables
NEXUS_USER="my-username"
NEXUS_PASSWORD="my-password"
NEXUS_IP="my-nexus-ip"
NODE_REPO="npm-hosted-repo"

# Fetch the latest artifact details and save to a JSON file
curl -u ${NEXUS_USER}:${NEXUS_PASSWORD} -X GET "http://${NEXUS_IP}:8081/service/rest/v1/components?repository=${NODE_REPO}&sort=version" | jq "." > artifact.json

# Extract the download URL of the latest artifact
artifactDownloadUrl=$(jq '.items[0].assets[0].downloadUrl' artifact.json --raw-output)

# Fetch the artifact using wget
wget --http-user=${NEXUS_USER} --http-password=${NEXUS_PASSWORD} ${artifactDownloadUrl} -O latest-artifact.tgz

# Untar the downloaded artifact
tar -xzf latest-artifact.tgz

# Navigate to the extracted directory (assuming the package directory is the root directory of the tarball)
cd package

# Install dependencies and start the application
npm install
npm start
