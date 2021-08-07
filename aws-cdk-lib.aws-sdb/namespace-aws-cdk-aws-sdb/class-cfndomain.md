# Class CfnDomain

클라우드포메이션의 `AWS::SDB::Domain`.

AWS::SDB::Domain 에 대한 정의 [http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-simpledb.html](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-simpledb.html)

#### 시놉시스 <a id="aws_sdb_CfnDomain_synopsis"></a>

**컨스트럭터**

| constructor\(scope, id, props\) | 새로운 `AWS::SDB::Domain` 생성. |
| :--- | :--- |


**프로퍼티**

| cfnProperties |  |
| :--- | :--- |
| [description](https://docs.aws.amazon.com/cdk/api/latest/typescript/api/aws-sdb/cfndomain.html#aws_sdb_CfnDomain_description) | `AWS::SDB::Domain.Description`. |

**메소드**

| [inspect\(inspector\)](https://docs.aws.amazon.com/cdk/api/latest/typescript/api/aws-sdb/cfndomain.html#aws_sdb_CfnDomain_inspect) | CloudFormation 리소스를 검토하고 속성을 공개합니다. |
| :--- | :--- |
| [renderProperties\(props\)](https://docs.aws.amazon.com/cdk/api/latest/typescript/api/aws-sdb/cfndomain.html#aws_sdb_CfnDomain_renderProperties) |  |

#### 컨스트럭터 <a id="constructors"></a>

**constructor\(scope, id, props\)**

새로운 `AWS::SDB::Domain`생성합니다.

**Declaration**

```text
constructor(scope: cdk.Construct, id: string, props?: CfnDomainProps);
```

**Parameters**

`scope` cdk.Construct

 - 이 리소스가 정의된 스코프

id string

 - 리소스의 스코프 아이디

props CfnDomainProps

 - 리소스에 대한 속성

#### 프로퍼티 <a id="properties"></a>

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

CloudFormation 리소스를 검토하고 속성을 공개합니다.

**정의**

```text
inspect(inspector: cdk.TreeInspector): void;
```

**파라미터**

inspector cdk.TreeInspector

트리 검사기를 사용하여 속성을 수집하고 처리할 수 있습니다.

**반환값**

void

**renderProperties\(props\)**

**정의**

```text
protected renderProperties:
```

**파라미터**

props { \[key: string\]: any; }

**반환값**

{ \[key: string\]: any; }

