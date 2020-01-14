#gcs-backup

##Process
The gcs-backup creates a cronjob which runs on a selected schedule.  It also includes a follower for a factomd network in a statefulset.  The cronjob it creates backs up the network that the follower is followeing.  It does this by scaling the follower down, while the persistent volume remains.  It then calls for a job that creates a pod that mounts to that volume.  It then uses GCS-fuse to upload a tar and gzipped directory of the factomd database to a GCS bucket.  Once that job is complete, it will re-scale the follower back up.

##Restore
The restore job is generally the reverse.  It creates a control pod that cycles down the follower, then uses another job to mount to the volume with the database.  It then pulls the file down from the given bucket using GCS Fuse, uncompresses it in the FactomD database location.  Then, once that's complete, it scales the follower back up to continue.


##TODO:
