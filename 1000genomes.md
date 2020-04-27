# 1000genomes public bucket

Rclone config:
```
[aws-anonymous]
type = s3
provider = AWS
env_auth = false
region = us-east-1
# anonymous access
access_key_id =
secret_access_key =
```

Data sizes:
```
$ rclone lsd aws-anonymous:1000genomes
           0 2020-04-27 09:58:15        -1 1000G_2504_high_coverage
           0 2020-04-27 09:58:15        -1 alignment_indices
           0 2020-04-27 09:58:15        -1 changelog_details
           0 2020-04-27 09:58:15        -1 complete_genomics_indices
           0 2020-04-27 09:58:15        -1 data
           0 2020-04-27 09:58:15        -1 hgsv_sv_discovery
           0 2020-04-27 09:58:15        -1 phase1
           0 2020-04-27 09:58:15        -1 phase3
           0 2020-04-27 09:58:15        -1 pilot_data
           0 2020-04-27 09:58:15        -1 release
           0 2020-04-27 09:58:15        -1 sequence_indices
           0 2020-04-27 09:58:15        -1 technical

$ rclone --fast-list size aws-anonymous:1000genomes/1000G_2504_high_coverage
Total objects: 5009
Total size: 37.556 TBytes (41293014551858 Bytes)

$ rclone size aws-anonymous:1000genomes/alignment_indices
Total objects: 66
Total size: 226.024 MBytes (237002921 Bytes)

$ rclone size aws-anonymous:1000genomes/changelog_details
Total objects: 1986
Total size: 167.806 MBytes (175957267 Bytes)

$ rclone size aws-anonymous:1000genomes/complete_genomics_indices
Total objects: 5
Total size: 3.031 MBytes (3177883 Bytes)

$ rclone --fast-list size aws-anonymous:1000genomes/data
Total objects: 8078
Total size: 63.301 TBytes (69599983742852 Bytes)

$ rclone size aws-anonymous:1000genomes/hgsv_sv_discovery
Total objects: 47
Total size: 2.031 TBytes (2232812207917 Bytes)

$ rclone --fast-list size aws-anonymous:1000genomes/phase1
Total objects: 21748
Total size: 89.076 TBytes (97939595293633 Bytes)

$ rclone --fast-list size aws-anonymous:1000genomes/phase3
Total objects: 280113
Total size: 235.619 TBytes (259065352992919 Bytes)

$ rclone size aws-anonymous:1000genomes/pilot_data
Total objects: 37880
Total size: 12.363 TBytes (13592754458349 Bytes)

$ rclone size aws-anonymous:1000genomes/release
Total objects: 2207
Total size: 3.069 TBytes (3374134173655 Bytes)

$ rclone size aws-anonymous:1000genomes/sequence_indices
Total objects: 236
Total size: 1.664 GBytes (1786442468 Bytes)

$ rclone --fast-list --checkers 30 size aws-anonymous:1000genomes/technical
Total objects: 118652
Total size: 242.055 TBytes (266141943147392 Bytes)
```