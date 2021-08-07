# Class CfnDomain

클라우드포메이션의 `AWS::SDB::Domain`.

AWS::SDB::Domain 에 대한 정[http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-simpledb.html](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-simpledb.html)

#### Synopsis <a id="aws_sdb_CfnDomain_synopsis"></a>

**Constructors**

| constructor\(scope, id, props\) | Create a new `AWS::SDB::Domain`. |
| :--- | :--- |


**Properties**

| cfnProperties |  |
| :--- | :--- |
| [description](https://docs.aws.amazon.com/cdk/api/latest/typescript/api/aws-sdb/cfndomain.html#aws_sdb_CfnDomain_description) | `AWS::SDB::Domain.Description`. |

**Methods**

| [inspect\(inspector\)](https://docs.aws.amazon.com/cdk/api/latest/typescript/api/aws-sdb/cfndomain.html#aws_sdb_CfnDomain_inspect) | Examines the CloudFormation resource and discloses attributes. |
| :--- | :--- |
| [renderProperties\(props\)](https://docs.aws.amazon.com/cdk/api/latest/typescript/api/aws-sdb/cfndomain.html#aws_sdb_CfnDomain_renderProperties) |  |

#### Constructors <a id="constructors"></a>

**constructor\(scope, id, props\)**

Create a new `AWS::SDB::Domain`.

**Declaration**

```text
constructor(scope: cdk.Construct, id: string, props?: CfnDomainProps);
```

**Parameters**

scope cdk.Construct

scope in which this resource is defined.id string

scoped id of the resource.props CfnDomainProps

resource properties.

#### Properties <a id="properties"></a>

**cfnProperties**

**Declaration**

```text
protected readonly cfnProperties:
```

**Property Value**

{ \[key: string\]: any; }

**description**

`AWS::SDB::Domain.Description`.

http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-simpledb.html\#cfn-sdb-domain-description

**Declaration**

```text
description: string | undefined;
```

**Property Value**

string \| undefined

#### Methods <a id="methods"></a>

**inspect\(inspector\)**

Examines the CloudFormation resource and discloses attributes.

**Declaration**

```text
inspect(inspector: cdk.TreeInspector): void;
```

**Parameters**

inspector cdk.TreeInspector

tree inspector to collect and process attributes.

**Returns**

void

**renderProperties\(props\)**

**Declaration**

```text
protected renderProperties:
```

**Parameters**

props { \[key: string\]: any; }

**Returns**

{ \[key: string\]: any; }

