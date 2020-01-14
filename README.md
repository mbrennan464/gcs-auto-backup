# gcs-backup

## Process
The gcs-backup creates a cronjob which runs on a selected schedule.  It also includes a follower for a factomd network in a statefulset.  The cronjob it creates backs up the network that the follower is followeing.  It does this by scaling the follower down, while the persistent volume remains.  It then calls for a job that creates a pod that mounts to that volume.  It then uses GCS-fuse to upload a tar and gzipped directory of the factomd database to a GCS bucket.  Once that job is complete, it will re-scale the follower back up.

## Requirements
* A service account with access to the selected GCS bucket, and access to the Kubernetes API from within the cluster is required. 
* The GCS bucket needs to be created as well.  `gsutil mb gs://[BUCKET_NAME]/`
* Images must be available in the GCR for the specific project as well.  They are required and listed in the values.yaml for each environment.

## Usage
From the main directory, 
`DECRYPT_CHARTS=true  helm-wrapper  install ./gcs-backup-helm --name gcs-backup --namespace fctd  -f ./gcs-backup-helm/values/dev/values.yaml -f ./gcs-backup-helm/secrets/dev/secrets.yaml`

## Restore
Restore is not available by job or chart yet - in progress


## TODO:
* Possible change of host for images
