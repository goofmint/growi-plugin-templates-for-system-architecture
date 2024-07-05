# GCP System Architecture

Those diagrams are getting from [davidholsgrove/gcp-icons-for-plantuml: PlantUML sprites, macros, and other includes for Google Cloud Platform services and resources](https://github.com/davidholsgrove/gcp-icons-for-plantuml).

## Getting Started

Here is a simple diagram. 

```plantuml
@startuml Hello World
!define GCPPuml https://raw.githubusercontent.com/davidholsgrove/gcp-icons-for-plantuml/master/dist
!includeurl GCPPuml/GCPCommon.puml
!includeurl GCPPuml/DeveloperTools/all.puml
!includeurl GCPPuml/Storage/CloudStorage.puml

actor "Person" as personAlias
CloudToolsforVisualStudio(desktopAlias, "Label", "Technology", "Optional Description")
CloudStorage(storageAlias, "Label", "Technology", "Optional Description")

personAlias --> desktopAlias
desktopAlias --> storageAlias

@enduml
```

## Icons

Available icons can be found at [gcp-icons-for-plantuml/GCPSymbols.md at master · davidholsgrove/gcp-icons-for-plantuml](https://github.com/davidholsgrove/gcp-icons-for-plantuml/blob/master/GCPSymbols.md).

## Basic diagrams

```plantuml
@startuml Basic Usage

!define GCPPuml https://raw.githubusercontent.com/davidholsgrove/gcp-icons-for-plantuml/master/dist
!includeurl GCPPuml/GCPCommon.puml
!includeurl GCPPuml/InternetOfThings/CloudIoTCore.puml
!includeurl GCPPuml/DataAnalytics/CloudPubSub.puml

left to right direction

agent "Published Event" as event #fff

CloudIoTCore(iotRule, "Action Error Rule", "error if Kinesis fails")
CloudPubSub(eventStream, "IoT Events", "2 shards")
CloudPubSub(errorQueue, "Rule Error Queue", "failed Rule actions")

event --> iotRule : JSON message
iotRule --> eventStream : messages
iotRule --> errorQueue : Failed action message

@enduml
```

## Simplified View

A simple view is available by loading `!include AWSPuml/AWSSimplified.puml`.

```plantuml
@startuml Two Modes - Technical View

!define GCPPuml https://raw.githubusercontent.com/davidholsgrove/gcp-icons-for-plantuml/master/dist
!include GCPPuml/GCPCommon.puml

' Include Material Design Common icons
!include <material/common>

' Uncomment the following line to create simplified view
!include GCPPuml/GCPSimplified.puml

!include GCPPuml/APIManagement/CloudEndpoints.puml
!include GCPPuml/Compute/AppEngine.puml
!include <material/cellphone_android>

left to right direction

GCPEntityColoring(MA_CELLPHONE_ANDROID)
MA_CELLPHONE_ANDROID(#9E9E9E, 1, sources, rectangle, "Android")
CloudEndpoints(endpoints, "GCP Cloud Endpoints", "api")
AppEngine(engine, "GCP App Engine", "API Backend Instances")

sources <--> endpoints
endpoints <--> engine

@enduml
```

## Other examples

Other examples can be found at [gcp-icons-for-plantuml/examples at master · davidholsgrove/gcp-icons-for-plantuml](https://github.com/davidholsgrove/gcp-icons-for-plantuml/tree/master/examples).

