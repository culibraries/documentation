# Patron Fines

The patron fines workflow provides a means for Sierra generated fines file to be accessed by library personnel.

## Overview

Contact: Mee Chang

1. Library Personnel submit a request to generate export
2. Sierra exports file to FTP server
3. Data file is stored in AWS EFS filesystem
4. Cronjob runs every 2 mins (mounts EFS filesystem)
5. Job runs transforms  
6. Job uploads file to S3(cubl-patron-fines)
7. Library personnel access through Cloud-Browser

## Transform

Transform repository: [https://github.com/culibraries/patron-fines](https://github.com/culibraries/patron-fines)

## Kubernetes Cronjob

Dockerfile and deploy YAML: [https://github.com/culibraries/k8s-cronjob-tasks/tree/main/patron-fine](https://github.com/culibraries/k8s-cronjob-tasks/tree/main/patron-fine)
