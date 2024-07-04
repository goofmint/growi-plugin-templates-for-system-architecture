# AWSシステムアーキテクチャ

これらの図は[awslabs/aws-icons-for-plantuml: PlantUML sprites, macros, and other includes for Amazon Web Services services and resources](https://github.com/awslabs/aws-icons-for-plantuml?tab=readme-ov-file#basic-usage)より取得しています。

## Getting Started

簡単な図です。 `!$AWS_DARK = true` でダークモードにて描画します。

```plantuml
@startuml Hello World
' Uncomment the line below for "dark mode" styling
'!$AWS_DARK = true

!define AWSPuml https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/v18.0/dist
!include AWSPuml/AWSCommon.puml
!include AWSPuml/BusinessApplications/all.puml
!include AWSPuml/Storage/SimpleStorageService.puml

actor "Person" as personAlias
WorkDocs(desktopAlias, "Label", "Technology", "Optional Description")
SimpleStorageService(storageAlias, "Label", "Technology", "Optional Description")

personAlias --> desktopAlias
desktopAlias --> storageAlias

@enduml
```

## アイコン

利用できるアイコンは[aws-icons-for-plantuml/AWSSymbols.md at main · awslabs/aws-icons-for-plantuml](https://github.com/awslabs/aws-icons-for-plantuml/blob/main/AWSSymbols.md)にて確認できます。

## 基本形

```plantuml
@startuml Raw usage - Images
' Uncomment the line below for "dark mode" styling
'!$AWS_DARK = true

!define AWSPuml https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/v18.0/dist
!include AWSPuml/AWSCommon.puml
!include AWSPuml/MachineLearning/SageMakerModel.puml
!include AWSPuml/Robotics/RoboMaker.puml

component "$SageMakerModelIMG()" as myMLModel
database "$RoboMakerIMG()" as myRoboticService
RoboMaker(mySecondFunction, "Reinforcement Learning", "Gazebo")

rectangle "$SageMakerModelIMG()" as mySecondML

myMLModel --> myRoboticService
mySecondFunction --> mySecondML

@enduml
```

## シンプルな図

`!include AWSPuml/AWSSimplified.puml` を読み込むと、AWSのシンプルな表示が利用できます。

```plantuml
@startuml Two Modes - Technical View
' Uncomment the line below for "dark mode" styling
'!$AWS_DARK = true

!define AWSPuml https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/v18.0/dist
!include AWSPuml/AWSCommon.puml

!include AWSPuml/AWSSimplified.puml

!include AWSPuml/General/Users.puml
!include AWSPuml/ApplicationIntegration/APIGateway.puml
!include AWSPuml/SecurityIdentityCompliance/Cognito.puml
!include AWSPuml/Compute/Lambda.puml
!include AWSPuml/Database/DynamoDB.puml

left to right direction

Users(sources, "Events", "millions of users")
APIGateway(votingAPI, "Voting API", "user votes")
Cognito(userAuth, "User Authentication", "jwt to submit votes")
Lambda(generateToken, "User Credentials", "return jwt")
Lambda(recordVote, "Record Vote", "enter or update vote per user")
DynamoDB(voteDb, "Vote Database", "one entry per user")

sources --> userAuth
sources --> votingAPI
userAuth <--> generateToken
votingAPI --> recordVote
recordVote --> voteDb
@enduml
```

## シーケンス図

シーケンス図の中でAWSのアイコンを利用する例です。

```plantuml
@startuml Sequence Diagram - Technical
' Uncomment the line below for "dark mode" styling
'!$AWS_DARK = true

!define AWSPuml https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/v18.0/dist
!include AWSPuml/AWSCommon.puml
!include AWSPuml/Compute/all.puml
!include AWSPuml/ApplicationIntegration/APIGateway.puml
!include AWSPuml/General/Internetalt1.puml
!include AWSPuml/Database/DynamoDB.puml

actor User as user
APIGatewayParticipant(api, Credit Card System, All methods are POST)
LambdaParticipant(lambda,AuthorizeCard,)
DynamoDBParticipant(db, PaymentTransactions, sortkey=transaction_id+token)
Internetalt1Participant(processor, Authorizer, Returns status and token)

user -> api: Process transaction\nPOST /prod/process
api -> lambda: Invokes lambda with cardholder details
lambda -> processor: Submit via API token\ncard number, expiry, CID
processor -> processor: Validate and create token
processor -> lambda: Returns status code and token
lambda -> db: PUT transaction id, token
lambda -> api: Returns\nstatus code, transaction id
api -> user: Returns status code
@enduml
```

## グルーピング

```plantuml
@startuml VPC
' Uncomment the line below for "dark mode" styling
'!$AWS_DARK = true

!define AWSPuml https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/v18.0/dist
!include AWSPuml/AWSCommon.puml
!include AWSPuml/AWSSimplified.puml
!include AWSPuml/Compute/EC2.puml
!include AWSPuml/Compute/EC2Instance.puml
!include AWSPuml/Groups/AWSCloud.puml
!include AWSPuml/Groups/VPC.puml
!include AWSPuml/Groups/AvailabilityZone.puml
!include AWSPuml/Groups/PublicSubnet.puml
!include AWSPuml/Groups/PrivateSubnet.puml
!include AWSPuml/NetworkingContentDelivery/VPCNATGateway.puml
!include AWSPuml/NetworkingContentDelivery/VPCInternetGateway.puml

hide stereotype
skinparam linetype ortho

AWSCloudGroup(cloud) {
  VPCGroup(vpc) {
    VPCInternetGateway(internet_gateway, "Internet gateway", "")

    AvailabilityZoneGroup(az_1, "\tAvailability Zone 1\t") {
      PublicSubnetGroup(az_1_public, "Public subnet") {
        VPCNATGateway(az_1_nat_gateway, "NAT gateway", "") #Transparent
      }
      PrivateSubnetGroup(az_1_private, "Private subnet") {
        EC2Instance(az_1_ec2_1, "Instance", "") #Transparent
      }

      az_1_ec2_1 .u.> az_1_nat_gateway
    }

    AvailabilityZoneGroup(az_2, "\tAvailability Zone 2\t") {
      PublicSubnetGroup(az_2_public, "Public subnet") {
        VPCNATGateway(az_2_nat_gateway, "NAT gateway", "") #Transparent
      }
      PrivateSubnetGroup(az_2_private, "Private subnet") {
        EC2Instance(az_2_ec2_1, "Instance", "") #Transparent
      }

      az_2_ec2_1 .u.> az_2_nat_gateway
    }

    az_2_nat_gateway .[hidden]u.> internet_gateway
    az_1_nat_gateway .[hidden]u.> internet_gateway
  }
}
@enduml
```


