# AWS System Architecture

Those diagrams are getting from [awslabs/aws-icons-for-plantuml: PlantUML sprites, macros, and other includes for Amazon Web Services services and resources](https://github.com/awslabs/aws-icons-for-plantuml?tab=readme-ov-file).

## Getting Started

Here is a simple diagram. `!$AWS_DARK = true` to draw in dark mode.

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

## Icons

Available icons can be found at [aws-icons-for-plantuml/AWSSymbols.md at main - awslabs/aws-icons-for-plantuml](https://github.com/awslabs/aws-icons-for- plantuml/blob/main/AWSSymbols.md).

## Basic diagrams

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

## Simplified View

A simple view is available by loading `!include AWSPuml/AWSSimplified.puml`.

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

## Sequence Diagram

This is an example of using AWS icons in a sequence diagram.

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

## Grouping

Here is an example of a work using groups.

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

## Other examples

Other examples can be found at [aws-icons-for-plantuml/examples at main - awslabs/aws-icons-for-plantuml](https://github.com/awslabs/aws-icons-for-plantuml/tree/ main/examples).
