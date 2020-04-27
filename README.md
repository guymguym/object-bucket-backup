# Object Bucket Backup (OBB)

This helm chart installs a backup job that uses Kubernetes, noobaa.io and rclone.org to make a backup of an object-store bucket.

The default values.yaml configures the backup source to a public AWS bucket called 1000genomes - see https://rclone.org/s3/#anonymous-access-to-public-buckets.

Every backup instance allocates an ObjectBucketClaim that creates a new bucket in noobaa and returns secret & configmap to mount by the backup pod. That OBC is then managing the lifecycle of the bucket, and helm will not delete it even if the chart is uninstalled, and resinstalling the chart with existing release name will reuse the same OBC and target bucket.

NooBaa provide S3 service to manage, secure and optimize data and the data location can be changed dynamically using BackingStores and BucketClasses - see https://github.com/noobaa/noobaa-operator.

## Prepare

Install NooBaa or use the Openshift Container Storage OCS operator from operatorhub.
The default values assumes noobaa is installed in namespace `noobaa-obb`.
If you want to use NooBaa from another namespace you will need to override the target properties that contain the namespace such as `target.obcStorageClassName` and `target.rclone.endpoint`.

## myvalues.yaml

Create your value override file `myvalues.yaml` (gitignored by default):
```yaml
source:
  prefix: "changelog_details/"    # 170 MB
  # prefix: "sequence_indices/"   # 1.6 GB
  # prefix: "release/20100804"    # 290 GB
  # prefix: "release/20101123"    #  30 GB
  # prefix: "release/20110521"    # 145 GB
  # prefix: "release/20130502"    # 2.6 TB
  # prefix: "release/"            # 3.0 TB
target:
  obcStorageClassName: "<noobaa-namespace>.noobaa.io"
  rclone:
    endpoint: "https://s3.<<noobaa-namespace>>.svc"
```

## Usage

1. Install a backup job:
```
helm install backup-job . -f myvalues.yaml
```

2. Read backup job notes:
```
helm get notes backup-job
```

3. Upgrade the helm release to run another backup job:
```
helm upgrade backup-job . -f myvalues.yaml
```

4. Manage the target bucket:
```
noobaa status
noobaa obc list 
noobaa obc status NAME
kubectl edit bucketclass noobaa-default-bucket-class
```

Notice that we configure helm to leave the OBC after uninstall of a backup job, so that you will be able to choose if you prefer to keep the backup bucket or delete it manually by deleting the OBC.

Notice that we currently configure the env with `RCLONE_NO_CHECK_CERTIFICATE=true` which is not secure. The problem seems to be that rclone needs to allow to set certificate setting per endpoint config, because for the internal noobaa endpoint we also set `RCLONE_CA_CERT="/var/run/secrets/kubernetes.io/serviceaccount/service-ca.crt"` but when copying between a public endpoint and a cluster endpoint setting `RCLONE_CA_CERT` fails to use the public root CA's.

