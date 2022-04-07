# Kubernetes Cronjob Tasks

This document describes the individual Cronjob Tasks associated with our current kubernetes infrastructure. The backup  cronjobs to S3 bucket(cubl-backup) have a 30 day lifecycle rule. [Github Repository](https://github.com/culibraries/k8s-cronjob-tasks)

## Active Tasks 

1. [alb-targetgroup-update](https://github.com/culibraries/k8s-cronjob-tasks/tree/main/alb-targetgroup-update)
    This task updates the instances within the target group. The cta-test cluster is 100% spot instances, and the task updates the target group with new spot instances. When cta-prod is moved to AWS EKS, the cluster could be 100% spot instances. This task would need modification to include the cta-prod cluster.
1. [cert-aws-upload](https://github.com/culibraries/k8s-cronjob-tasks/tree/main/cert-aws-upload)
    The CTA Infrastructure and Application section uses a cert-manager to produce HTTPS (SSL/TLS) certificates. This nightly task uploads the production cluster certificate to the AWS Certificate Manager service.
1. [cybercom-db-backup](https://github.com/culibraries/k8s-cronjob-tasks/tree/main/cybercom-db-backup)
    This cronjob task is for backing up MongoDB for the production cluster. Once backed up, file uploaded to S3 cubl-backup/cybercom/mongo/
1. [deployment-restart](https://github.com/culibraries/k8s-cronjob-tasks/tree/main/deployment-restart)
    Cronjob is used to restart multiple deployments. This action is no longer needed with the update of nginx resolver.
1. [ir-reports](https://github.com/culibraries/k8s-cronjob-tasks/tree/main/ir-reports)
    Cronjob for creating IR Reports for metadata. Library personnel use the Cloud-Browser to access reports.  [ir-exportq](https://github.com/culibraries/ir-exportq) celery queue used to run report.
1. [ir-s3-sync](https://github.com/culibraries/k8s-cronjob-tasks/tree/main/ir-s3-sync)
    Cronjob used to sync FCRepo S3 Bucket.
1. [patron-fine](https://github.com/culibraries/k8s-cronjob-tasks/tree/main/patron-fine)
    Cronjob to check for new parton fine files. Once a new file the job will transform and upload to S3(cubl-patron-fines). 
1. [solr](https://github.com/culibraries/k8s-cronjob-tasks/tree/main/solr)
    Solr backup of IR solr index and cloud configuration. Backup uploaded to S3 cubl-backup/solr bucket.
1. [solr-json](https://github.com/culibraries/k8s-cronjob-tasks/tree/main/solr-json)
    Cronjob takes a dump of all documents within the IR solr index. The export is in JSON.
1. [survey](https://github.com/culibraries/k8s-cronjob-tasks/tree/main/survey)
    Cronjob that checks the mongo collection that holds the survey schedule. If survey is scheduled, ENV variable updated and a restart of deployment.  

## Inactive Tasks

1. [folio-db-backup](https://github.com/culibraries/k8s-cronjob-tasks/tree/main/folio-db-backup)
    Honeysuckle version backup of postgres in cluster DB. This task is retired
1. [gatecount](https://github.com/culibraries/k8s-cronjob-tasks/tree/main/gatecount)
    Retired task that was an example task that collected gatecount numbers
1. [wekan](https://github.com/culibraries/k8s-cronjob-tasks/tree/main/wekan)
    Retired backup of Wekan Mongo DB instance. This was used for Libkan(Retired project boards)
