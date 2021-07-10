---
description: Overview
---

# 개요

## Amazon S3 구성 라이브러

![cfn-resources: Stable](https://docs.aws.amazon.com/cdk/api/v2/img/cfn--resources-stable-success.svg)

![cdk-constructs: Stable](https://docs.aws.amazon.com/cdk/api/v2/img/cdk--constructs-stable-success.svg)

새로운 암호화 되지않은 버킷을 생합니다.

```text
new Bucket(this, 'MyFirstBucket');
```

`Bucket` 컨스트럭터 다음과 같은 배포에 대한 어트리뷰트를 사용합니다 

* `bucketArn` - 버킷의 ARN \(예 : `arn:aws:s3:::bucket_name`\)
* `bucketName` - 버킷의 이름 \(예 `bucket_name`\)
* `bucketWebsiteUrl` - 버킷의 웹사이트 URL \(예:`http://bucket_name.s3-website-us-west-1.amazonaws.com`\)
* `bucketDomainName` - 버킷의 URL \(예:  `bucket_name.s3.amazonaws.com`\)
* `bucketDualStackDomainName` - 버킷의 이중 스택 URL \( `bucket_name.s3.dualstack.eu-west-1.amazonaws.com`\)
* `bucketRegionalDomainName` - 버킷의 지역 URL \(`bucket_name.s3.eu-west-1.amazonaws.com`\)
* `arnForObjects(pattern)` -버킷에 포함된 개체의 ARN \( `arn:aws:s3:::bucket_name/exampleobject.png` 또`arn:aws:s3:::bucket_name/Development/*`\)
* `urlForObject(key)` - 버킷에 포함된 개체의 HTTP URL \( `https://s3.cn-north-1.amazonaws.com.cn/china-bucket/mykey`\)
* `virtualHostedUrlForObject(key)` - 버킷에 포함된 개체의 가상 호스트 스타일 HTTP URL \(예:`https://china-bucket-s3.cn-north-1.amazonaws.com.cn/mykey`\)
* `s3UrlForObject(key)` -버킷에 포함된 개체의 S3 URL \(예: `s3://bucket/mykey`\)

### Encryption \(암호화\)

KMS로 암호화된 버킷 정의 : 

```text
const bucket = new Bucket(this, 'MyEncryptedBucket', {
    encryption: BucketEncryption.KMS
});

// you can access the encryption key:
assert(bucket.encryptionKey instanceof kms.Key);
```

자체 키를 적용할 수 있습니다.

```text
const myKmsKey = new kms.Key(this, 'MyKey');

const bucket = new Bucket(this, 'MyEncryptedBucket', {
    encryption: BucketEncryption.KMS,
    encryptionKey: myKmsKey
});

assert(bucket.encryptionKey === myKmsKey);
```

 [S3 Bucket Keys](https://docs.aws.amazon.com/AmazonS3/latest/dev/bucket-key.html) 를 통한 KMS-SSE 암호화 활성화는 아래와 같이 할 수 있습니다.

```text
const bucket = new Bucket(this, 'MyEncryptedBucket', {
    encryption: BucketEncryption.KMS,
    bucketKeyEnabled: true
});

assert(bucket.bucketKeyEnabled === true);
```

S3 마스터 KMS를 사용하기 위해서 `BucketEncryption.ManagedKms` 를 사할 수 있습니다.

```text
const bucket = new Bucket(this, 'Buck', {
    encryption: BucketEncryption.KMS_MANAGED
});

assert(bucket.encryptionKey == null);
```

### Permissions \(권한\)

`addToResourcePolicy(statement)`를 호출할때 버킷을 위 정책이 자동으로 생성됩니다.

```text
const bucket = new Bucket(this, 'MyBucket');
bucket.addToResourcePolicy(new iam.PolicyStatement({
  actions: ['s3:GetObject'],
  resources: [bucket.arnForObjects('file.txt')],
  principals: [new iam.AccountRootPrincipal()],
}));
```

생성된 버킷 정책에 직접 접근하여 조건을 추가하거나 정책을 삭제할 수 있습니다.

```text
bucket.policy?.applyRemovalPolicy(RemovalPolicy.RETAIN);
```

대부분의 경우 버킷 정책을 직접 조작할 필요가 없습니다. 다만 버킷에는 다른 리소스에 대한 사전정의된 퍼미션 그룹을 사용하기 위한 "위임\(Grant\) 메소드" 가 있습니다.

```text
const lambda = new lambda.Function(this, 'Lambda', { /* ... */ });

const bucket = new Bucket(this, 'MyBucket');
bucket.grantReadWrite(lambda);
```

위 코드는 람다 실행을 위해 읽고 쓰기 위한 권한을 버킷에 제공합니다.

### AWS 기초 보안 Best Practices

#### SSL\(Secure Socket Layer\) 강제

```
const bucket = new Bucket(this, 'Bucket', {
enforceSSL: true
});
```

### 스택간 버킷 공유 

동일한 CDK 어플리케이션 내에서 다른 스택에 있는 버킷을 사용할려면 객체를 다른 스택에 전달해서 사용합니다.

### 기존 버킷 불러오기 

기존 버킷을 CDK 응용 프로그램으로 가져오려`Bucket.from BucketAttributes`메소드 사용하십시오. 이 메소에서는 이미 존재하는 버킷의 속성을 설명하는 `BucketAttributes` 사용할 수 있습니다.

To import an existing bucket into your CDK application, use the `Bucket.fromBucketAttributes` factory method. This method accepts `BucketAttributes` which describes the properties of an already existing bucket:

```text
cost bucket = Bucket.fromBucketAttributes(this, 'ImportedBucket', {
    bucketArn: 'arn:aws:s3:::my-bucket'
});

// now you can just call methods on the bucket
bucket.grantReadWrite(user);
```

Alternatively, short-hand factories are available as `Bucket.fromBucketName` and `Bucket.fromBucketArn`, which will derive all bucket attributes from the bucket name or ARN respectively:

```text
const byName = Bucket.fromBucketName(this, 'BucketByName', 'my-bucket');
const byArn  = Bucket.fromBucketArn(this, 'BucketByArn', 'arn:aws:s3:::my-bucket');
```

\([Example not in your language? Click here.](https://docs.aws.amazon.com/cdk/latest/guide/multiple_languages.html)\)

The bucket's region defaults to the current stack's region, but can also be explicitly set in cases where one of the bucket's regional properties needs to contain the correct values.

```text
const myCrossRegionBucket = Bucket.fromBucketAttributes(this, 'CrossRegionImport', {
  bucketArn: 'arn:aws:s3:::my-bucket',
  region: 'us-east-1',
});
// myCrossRegionBucket.bucketRegionalDomainName === 'my-bucket.s3.us-east-1.amazonaws.com'
```

\([Example not in your language? Click here.](https://docs.aws.amazon.com/cdk/latest/guide/multiple_languages.html)\)

### Bucket Notifications

The Amazon S3 notification feature enables you to receive notifications when certain events happen in your bucket as described under [S3 Bucket Notifications](https://docs.aws.amazon.com/AmazonS3/latest/dev/NotificationHowTo.html) of the S3 Developer Guide.

To subscribe for bucket notifications, use the `bucket.addEventNotification` method. The `bucket.addObjectCreatedNotification` and `bucket.addObjectRemovedNotification` can also be used for these common use cases.

The following example will subscribe an SNS topic to be notified of all `s3:ObjectCreated:*` events:

```text
import { aws_s3_notifications as s3n } from 'aws-cdk-lib';

const myTopic = new sns.Topic(this, 'MyTopic');
bucket.addEventNotification(s3.EventType.OBJECT_CREATED, new s3n.SnsDestination(topic));
```

\([Example not in your language? Click here.](https://docs.aws.amazon.com/cdk/latest/guide/multiple_languages.html)\)

This call will also ensure that the topic policy can accept notifications for this specific bucket.

Supported S3 notification targets are exposed by the `@aws-cdk/aws-s3-notifications` package.

It is also possible to specify S3 object key filters when subscribing. The following example will notify `myQueue` when objects prefixed with `foo/` and have the `.jpg` suffix are removed from the bucket.

```text
bucket.addEventNotification(s3.EventType.OBJECT_REMOVED,
  new s3n.SqsDestination(myQueue),
  { prefix: 'foo/', suffix: '.jpg' });
```

\([Example not in your language? Click here.](https://docs.aws.amazon.com/cdk/latest/guide/multiple_languages.html)\)

### Block Public Access

Use `blockPublicAccess` to specify [block public access settings](https://docs.aws.amazon.com/AmazonS3/latest/dev/access-control-block-public-access.html) on the bucket.

Enable all block public access settings:

```text
const bucket = new Bucket(this, 'MyBlockedBucket', {
    blockPublicAccess: BlockPublicAccess.BLOCK_ALL
});
```

\([Example not in your language? Click here.](https://docs.aws.amazon.com/cdk/latest/guide/multiple_languages.html)\)

Block and ignore public ACLs:

```text
const bucket = new Bucket(this, 'MyBlockedBucket', {
    blockPublicAccess: BlockPublicAccess.BLOCK_ACLS
});
```

\([Example not in your language? Click here.](https://docs.aws.amazon.com/cdk/latest/guide/multiple_languages.html)\)

Alternatively, specify the settings manually:

```text
const bucket = new Bucket(this, 'MyBlockedBucket', {
    blockPublicAccess: new BlockPublicAccess({ blockPublicPolicy: true })
});
```

\([Example not in your language? Click here.](https://docs.aws.amazon.com/cdk/latest/guide/multiple_languages.html)\)

When `blockPublicPolicy` is set to `true`, `grantPublicRead()` throws an error.

### Logging configuration

Use `serverAccessLogsBucket` to describe where server access logs are to be stored.

```text
const accessLogsBucket = new Bucket(this, 'AccessLogsBucket');

const bucket = new Bucket(this, 'MyBucket', {
  serverAccessLogsBucket: accessLogsBucket,
});
```

\([Example not in your language? Click here.](https://docs.aws.amazon.com/cdk/latest/guide/multiple_languages.html)\)

It's also possible to specify a prefix for Amazon S3 to assign to all log object keys.

```text
const bucket = new Bucket(this, 'MyBucket', {
  serverAccessLogsBucket: accessLogsBucket,
  serverAccessLogsPrefix: 'logs'
});
```

\([Example not in your language? Click here.](https://docs.aws.amazon.com/cdk/latest/guide/multiple_languages.html)\)

### S3 Inventory

An [inventory](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-inventory.html) contains a list of the objects in the source bucket and metadata for each object. The inventory lists are stored in the destination bucket as a CSV file compressed with GZIP, as an Apache optimized row columnar \(ORC\) file compressed with ZLIB, or as an Apache Parquet \(Parquet\) file compressed with Snappy.

You can configure multiple inventory lists for a bucket. You can configure what object metadata to include in the inventory, whether to list all object versions or only current versions, where to store the inventory list file output, and whether to generate the inventory on a daily or weekly basis.

```text
const inventoryBucket = new s3.Bucket(this, 'InventoryBucket');

const dataBucket = new s3.Bucket(this, 'DataBucket', {
  inventories: [
    {
      frequency: s3.InventoryFrequency.DAILY,
      includeObjectVersions: s3.InventoryObjectVersion.CURRENT,
      destination: {
        bucket: inventoryBucket,
      },
    },
    {
      frequency: s3.InventoryFrequency.WEEKLY,
      includeObjectVersions: s3.InventoryObjectVersion.ALL,
      destination: {
        bucket: inventoryBucket,
        prefix: 'with-all-versions',
      },
    }
  ]
});
```

\([Example not in your language? Click here.](https://docs.aws.amazon.com/cdk/latest/guide/multiple_languages.html)\)

If the destination bucket is created as part of the same CDK application, the necessary permissions will be automatically added to the bucket policy. However, if you use an imported bucket \(i.e `Bucket.fromXXX()`\), you'll have to make sure it contains the following policy document:

```text
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "InventoryAndAnalyticsExamplePolicy",
      "Effect": "Allow",
      "Principal": { "Service": "s3.amazonaws.com" },
      "Action": "s3:PutObject",
      "Resource": ["arn:aws:s3:::destinationBucket/*"]
    }
  ]
}
```

\([Example not in your language? Click here.](https://docs.aws.amazon.com/cdk/latest/guide/multiple_languages.html)\)

### Website redirection

You can use the two following properties to specify the bucket [redirection policy](https://docs.aws.amazon.com/AmazonS3/latest/dev/how-to-page-redirect.html#advanced-conditional-redirects). Please note that these methods cannot both be applied to the same bucket.

#### Static redirection

You can statically redirect a to a given Bucket URL or any other host name with `websiteRedirect`:

```text
const bucket = new Bucket(this, 'MyRedirectedBucket', {
    websiteRedirect: { hostName: 'www.example.com' }
});
```

\([Example not in your language? Click here.](https://docs.aws.amazon.com/cdk/latest/guide/multiple_languages.html)\)

#### Routing rules

Alternatively, you can also define multiple `websiteRoutingRules`, to define complex, conditional redirections:

```text
const bucket = new Bucket(this, 'MyRedirectedBucket', {
  websiteRoutingRules: [{
    hostName: 'www.example.com',
    httpRedirectCode: '302',
    protocol: RedirectProtocol.HTTPS,
    replaceKey: ReplaceKey.prefixWith('test/'),
    condition: {
      httpErrorCodeReturnedEquals: '200',
      keyPrefixEquals: 'prefix',
    }
  }]
});
```

\([Example not in your language? Click here.](https://docs.aws.amazon.com/cdk/latest/guide/multiple_languages.html)\)

### Filling the bucket as part of deployment

To put files into a bucket as part of a deployment \(for example, to host a website\), see the `@aws-cdk/aws-s3-deployment` package, which provides a resource that can do just that.

### The URL for objects

S3 provides two types of URLs for accessing objects via HTTP\(S\). Path-Style and [Virtual Hosted-Style](https://docs.aws.amazon.com/AmazonS3/latest/dev/VirtualHosting.html) URL. Path-Style is a classic way and will be [deprecated](https://aws.amazon.com/jp/blogs/aws/amazon-s3-path-deprecation-plan-the-rest-of-the-story). We recommend to use Virtual Hosted-Style URL for newly made bucket.

You can generate both of them.

```text
bucket.urlForObject('objectname'); // Path-Style URL
bucket.virtualHostedUrlForObject('objectname'); // Virtual Hosted-Style URL
bucket.virtualHostedUrlForObject('objectname', { regional: false }); // Virtual Hosted-Style URL but non-regional
```

\([Example not in your language? Click here.](https://docs.aws.amazon.com/cdk/latest/guide/multiple_languages.html)\)

#### Object Ownership

You can use the two following properties to specify the bucket [object Ownership](https://docs.aws.amazon.com/AmazonS3/latest/dev/about-object-ownership.html).

**Object writer**

The Uploading account will own the object.

```text
new s3.Bucket(this, 'MyBucket', {
  objectOwnership: s3.ObjectOwnership.OBJECT_WRITER,
});
```

\([Example not in your language? Click here.](https://docs.aws.amazon.com/cdk/latest/guide/multiple_languages.html)\)

**Bucket owner preferred**

The bucket owner will own the object if the object is uploaded with the bucket-owner-full-control canned ACL. Without this setting and canned ACL, the object is uploaded and remains owned by the uploading account.

```text
new s3.Bucket(this, 'MyBucket', {
  objectOwnership: s3.ObjectOwnership.BUCKET_OWNER_PREFERRED,
});
```

\([Example not in your language? Click here.](https://docs.aws.amazon.com/cdk/latest/guide/multiple_languages.html)\)

#### Bucket deletion

When a bucket is removed from a stack \(or the stack is deleted\), the S3 bucket will be removed according to its removal policy \(which by default will simply orphan the bucket and leave it in your AWS account\). If the removal policy is set to `RemovalPolicy.DESTROY`, the bucket will be deleted as long as it does not contain any objects.

To override this and force all objects to get deleted during bucket deletion, enable the`autoDeleteObjects` option.

```text
const bucket = new Bucket(this, 'MyTempFileBucket', {
  removalPolicy: RemovalPolicy.DESTROY,
  autoDeleteObjects: true,
});
```

\([Example not in your language? Click here.](https://docs.aws.amazon.com/cdk/latest/guide/multiple_languages.html)\)

