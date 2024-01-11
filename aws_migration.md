## Migrate AWS data from one account to another
  - Source account - A
  - Destination account - B

### S3
  - Create a IAM user in B, create cli credentials
    
  - Create a bucket policy in A
        ```
            {
                 "Version": "2012-10-17",
                 "Statement": [
                {
                        "Effect": "Allow",
                        "Principal": {
                            "AWS": "<arn of IAM user in B>"
                    },
                "Action": [
                    "s3:GetObject",
                    "s3:ListBucket",
                    "s3:GetObjectTagging"
                ],
                "Resource": [
                    "arn:aws:s3:::<bucket name in A>",
                    "arn:aws:s3:::<buckect name in A>/*"
                    ]
                }]
            }
        ```

  - Configure the aws cli
        ```
            aws configure --profile B_profile   <add details of IAM user in B>
        ```
 
  - Sync or copy bucket data
        ```
            aws s3 sync s3://<bucket in A> s3://<bucket in B> --profile B_profile --storage-class GLACIER
        ```

### EC2 Disk snapshot
  - In account A , Navigate to snapshot, select actions -> snapshot settings -> Modify permissions -> add B account id for private sharing
  - In account B , Navigate to snapshot, find shared snapshot, and copy to own snapshot
