# AWS cmd

aws configure

aws s3api --profile=mtv_ai_lab list-objects-v2 --bucket archive-cband --query "Contents[?StorageClass=='GLACIER']" --output text | awk '{print substr($0, index($0, $2))}' | awk '{NF-=3};3' > glacier-restore.txt

aws s3api --profile=mtv_ai_lab restore-object --bucket archive-cband --key 2022/01/31/2022-01-31_22.tar.xz --restore-request '{"Days":7,"GlacierJobParameters":{"Tier":"Bulk"}}'

aws s3api --profile=mtv_ai_lab head-object --bucket archive-cband --key 2022/01/31/2022-01-31_23.tar.xz

aws s3 cp --profile=mtv_ai_lab s3://archive-cband/2022/01/31/2022-01-31_23.tar.xz --profile=quantnh s3://07weather-radar/training-dataset/ --storage-class STANDARD --force-glacier-transfer

