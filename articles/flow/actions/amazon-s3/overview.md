# Amazon S3 overview

Flow has built-in support for working with [Amazon S3](https://docs.aws.amazon.com/s3/), allowing flows to read, write, list, and delete objects in an S3 bucket. Use these actions to integrate S3 as a source or destination for files in your data and integration workflows — for example, ingesting files dropped into a bucket, archiving processed output, or moving objects between staging and main storage.

To use any S3 action, you first need an [Amazon S3 connection](./connecting-to-amazon-s3.md) configured with an Access Key ID, Secret Access Key, Region, and Bucket Name from the [AWS IAM Console](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html). The same connection is reused across all actions.

<br/>

## Explore

#### Connecting to Amazon S3
Set up the connection used by every S3 action. Covers the required AWS credentials, supported connection properties, and how to test the connection before saving.  
[Read more](./connecting-to-amazon-s3.md)

<br/>

#### Listing objects
Get the names of objects in a bucket, optionally filtered by prefix. Use [Get Amazon S3 object names](./get-s3object-names.md) when you need the full list as a variable, or [For each Amazon S3 object name](./foreach-s3object-name.md) when you want to iterate one object at a time.

<br/>

#### Reading objects
Download an object's contents into a flow. [Read Amazon S3 object as stream](./read-s3object-as-stream.md) is preferred for performance and lower memory use; [Read Amazon S3 object as byte array](./read-s3object-as-byte-array.md) is required when the same content needs to be read more than once (for example, processing it and then archiving it). Once read, the contents must be loaded with a compatible action — such as those in [Excel](../excel/overview.md) or [CSV](../csv/overview.md) — before the data can be used.

<br/>

#### Writing and removing objects
[Upload Amazon S3 object](./upload-s3object.md) creates a new object from a byte array or stream, with an option to overwrite existing files. [Append to Amazon S3 object](./append-to-s3object.md) adds data to an existing object, or creates it if it doesn't exist yet. [Delete Amazon S3 object](./delete-s3object.md) removes an object from the bucket.
