# Amazon S3 (Simple Storage Service)

## Steps

1. Create Bucket in S3 service;
2. Go to **Objects** -> upload files.
3. Go to **Properties** -> *Enable* the Static Website Hosting.
4. Go to **Permissions**

   a. Go to **Block Public Access** -> Uncheck everything, So public access is possible for this.
   b. **Bucket Policy** - Go to **Aws Policy Generator** first,
   
   - select policy type -> s3 service
   - add statement:
         - Effect : Allow
         - Principal : *
         - Action : All check("*")
         - Amazon Resource Name(ARN) : Get it from *Bucket*
   - Generate;

  You will get something like this; (Action might be `*`, change it as shown if not working. Also add `/*` after Resource name;
```code
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::cipher-bucketdemo/*"
        }
    ]
}

```

Paste it in permission policy section; 

And done!
you can go back to Properties section, and there one link would be visible, just access that;

**Enjoy your hosted Site**
