# AWSシステムアーキテクチャ

これらの図は[awslabs/aws-icons-for-plantuml: PlantUML sprites, macros, and other includes for Amazon Web Services services and resources](https://github.com/awslabs/aws-icons-for-plantuml?tab=readme-ov-file)より取得しています。

## Getting Started

簡単な図です。 `!$AWS_DARK = true` でダークモードにて描画します。

```plantuml
@startuml Hello World
' 以下をアンコメントするとダークモードで描画します
'!$AWS_DARK = true

!define AWSPuml https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/v18.0/dist
!include AWSPuml/AWSCommon.puml
!include AWSPuml/BusinessApplications/all.puml
!include AWSPuml/Storage/SimpleStorageService.puml

actor "人物" as personAlias
WorkDocs(desktopAlias, "ラベル", "テクノロジー", "任意の備考")
SimpleStorageService(storageAlias, "ラベル", "テクノロジー", "任意の備考")

personAlias --> desktopAlias
desktopAlias --> storageAlias

@enduml
```

## アイコン

利用できるアイコンは[aws-icons-for-plantuml/AWSSymbols.md at main · awslabs/aws-icons-for-plantuml](https://github.com/awslabs/aws-icons-for-plantuml/blob/main/AWSSymbols.md)にて確認できます。

## 基本形

シンプルな作例です。

```plantuml
@startuml Basic Usage - AWS IoT Rules Engine

'以下をアンコメントするとダークモードで描画します
'!$AWS_DARK = true

!define AWSPuml https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/v18.0/dist
!include AWSPuml/AWSCommon.puml
!include AWSPuml/InternetOfThings/IoTRule.puml
!include AWSPuml/Analytics/KinesisDataStreams.puml
!include AWSPuml/ApplicationIntegration/SimpleQueueService.puml

left to right direction

agent "イベント発行" as event

IoTRule(iotRule, "アクションエラールール", "Kinesisが失敗したらエラー")
KinesisDataStreams(eventStream, "IoTイベント", "2 shards")
SimpleQueueService(errorQueue, "エラーキュールール", "ルールアクションの失敗")

event --> iotRule : JSON message
iotRule --> eventStream : messages
iotRule --> errorQueue : Failed action message

@enduml
```

以下はRoboMakerの作例です。

```plantuml
@startuml Raw usage - Images
' 以下をアンコメントするとダークモードで描画します
'!$AWS_DARK = true

!define AWSPuml https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/v18.0/dist
!include AWSPuml/AWSCommon.puml
!include AWSPuml/MachineLearning/SageMakerModel.puml
!include AWSPuml/Robotics/RoboMaker.puml

component "$SageMakerModelIMG()" as myMLModel
database "$RoboMakerIMG()" as myRoboticService
RoboMaker(mySecondFunction, "強化学習", "Gazebo")

rectangle "$SageMakerModelIMG()" as mySecondML

myMLModel --> myRoboticService
mySecondFunction --> mySecondML

@enduml
```

## シンプルな図

`!include AWSPuml/AWSSimplified.puml` を読み込むと、AWSのシンプルな表示が利用できます。

```plantuml
@startuml Two Modes - Technical View
' 以下をアンコメントするとダークモードで描画します
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

Users(sources, "イベント", "数百万ユーザー")
APIGateway(votingAPI, "投票API", "user votes")
Cognito(userAuth, "ユーザー認証", "投票を送信する用jwt")
Lambda(generateToken, "ユーザー認証情報", "JWTの返却")
Lambda(recordVote, "投票の記録", "ユーザー単位の投票入力または更新")
DynamoDB(voteDb, "投票データベース", "1ユーザー1エントリー")

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
' 以下をアンコメントするとダークモードで描画します
'!$AWS_DARK = true

!define AWSPuml https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/v18.0/dist
!include AWSPuml/AWSCommon.puml
!include AWSPuml/Compute/all.puml
!include AWSPuml/ApplicationIntegration/APIGateway.puml
!include AWSPuml/General/Internetalt1.puml
!include AWSPuml/Database/DynamoDB.puml

actor ユーザー as user
APIGatewayParticipant(api, クレジットカードシステム, すべてPOSTメソッド)
LambdaParticipant(lambda,認証済みカード,)
DynamoDBParticipant(db, 支払いトランザクション, sortkey=transaction_id+token)
Internetalt1Participant(processor, 承認システム, ステータスとトークンの返却)

user -> api: トランザクション処理\nPOST /prod/process
api -> lambda: カード保有者の詳細情報を\nラムダ関数で取得
lambda -> processor: APIトークンで送信\nカード番号, 有効期限, CID
processor -> processor: 検証とトークンの生成
processor -> lambda: ステータスコードとトークンの返却
lambda -> db: PUT トランザクションID, トークン
lambda -> api: ステータスコードと\nトランザクションIDの返却
api -> user: ステータスコードの返却
@enduml
```

## グルーピング

グループを用いた作例です。

```plantuml
@startuml Auto Scaling
'Copyright 2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.
'SPDX-License-Identifier: MIT (For details, see https://github.com/awslabs/aws-icons-for-plantuml/blob/master/LICENSE)

' Uncomment the line below for "dark mode" styling
'!$AWS_DARK = true

!define AWSPuml https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/v18.0/dist
!include AWSPuml/AWSCommon.puml
!include AWSPuml/AWSSimplified.puml
!include AWSPuml/Compute/EC2AutoScaling.puml
!include AWSPuml/Compute/EC2Instance.puml
!include AWSPuml/Groups/AWSCloud.puml
!include AWSPuml/Groups/AvailabilityZone.puml
!include AWSPuml/Groups/VPC.puml
!include AWSPuml/Groups/AutoScalingGroup.puml
!include AWSPuml/NetworkingContentDelivery/VPCNATGateway.puml

skinparam rectangle<<hidden>> {
  shadowing false
  BackgroundColor transparent
  BorderColor transparent
}

!unquoted procedure LayoutRectangle($p_alias)
rectangle " " as $p_alias <<hidden>>
!endprocedure

AWSCloudGroup(cloud) {
  VPCGroup(vpc) {
    AvailabilityZoneGroup(az_2, "  Availability Zone 2") {
      VPCNATGateway(az_2_nat_gateway, "NAT gateway", "")
      EC2Instance(az_2_ec2_1, "Instance", "")
      EC2Instance(az_2_ec2_2, "Instance", "")

      az_2_nat_gateway -[hidden]d- az_2_ec2_1
      az_2_ec2_1 -[hidden]d- az_2_ec2_2
    }

    LayoutRectangle(layout_rectangle) {
      EC2AutoScaling(ec2_auto_scaling, "Amazon EC2 Auto Scaling", "")
      AutoScalingGroupGroup(asg_1)
      AutoScalingGroupGroup(asg_2)

      ec2_auto_scaling -[hidden]d- asg_1
      asg_1 -[hidden]d- asg_2
    }

    AvailabilityZoneGroup(az_1, "  Availability Zone 1") {
      VPCNATGateway(az_1_nat_gateway, "NAT gateway", "")
      EC2Instance(az_1_ec2_1, "Instance", "")
      EC2Instance(az_1_ec2_2, "Instance", "")

      az_1_nat_gateway -[hidden]d- az_1_ec2_1
      az_1_ec2_1 -[hidden]d- az_1_ec2_2
    }

    ec2_auto_scaling -[hidden]l- az_1_nat_gateway
    ec2_auto_scaling -[hidden]r- az_2_nat_gateway

    asg_1 -[dashed,$AWS_COLOR_SMILE]l- az_1_ec2_1
    asg_1 -[dashed,$AWS_COLOR_SMILE]r- az_2_ec2_1

    asg_2 -[dashed,$AWS_COLOR_SMILE]l- az_1_ec2_2
    asg_2 -[dashed,$AWS_COLOR_SMILE]r- az_2_ec2_2
  }
}
@enduml
```

## 他の例

他の例は[aws-icons-for-plantuml/examples at main · awslabs/aws-icons-for-plantuml](https://github.com/awslabs/aws-icons-for-plantuml/tree/main/examples)を参照してください。
