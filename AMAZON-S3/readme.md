# Amazon S3 (Simple Storage Service)

## Steps

1. Create Bucket in S3 service;
2. Go to **Objects** -> upload files(without main folder, just upload the files).
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

Link example:
```
http:cipher-bucketdemo.s3.website.ap-south-1.amazonaws.com
```

**Enjoy your hosted Site**

---

## How to host Two site from one Object?

while uploading files, this time we upload folder first, like `site1` and `site2` and its content inside it.

everything else remains same;

Now while visiting we just need to add `/site1/` or so on at end;

Take example that we have two Sites in **Object**

Then link would be: 
`http:cipher-bucketdemo.s3.website.ap-south-1.amazonaws.com/site1/`
and

`http:cipher-bucketdemo.s3.website.ap-south-1.amazonaws.com/site2/`

*remember to add `/` at end as well because else by default S3 looks for default file `index.html` which is not there as per our upload, we have site1 and site2 So it won't work.*
