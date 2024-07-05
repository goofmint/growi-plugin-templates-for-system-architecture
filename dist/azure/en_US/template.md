# Azure System Architecture

Those diagrams are getting from [davidholsgrove/gcp-icons-for-plantuml: PlantUML sprites, macros, and other includes for Google Cloud Platform services and resources](https://github.com/davidholsgrove/gcp-icons-for-plantuml).

## Getting Started

Here is a simple diagram. 

```plantuml
@startuml Hello World
!define AzurePuml https://raw.githubusercontent.com/plantuml-stdlib/Azure-PlantUML/release/2-2/dist
!includeurl AzurePuml/AzureCommon.puml
!includeurl AzurePuml/Databases/all.puml
!includeurl AzurePuml/Compute/AzureFunction.puml

actor "Person" as personAlias
AzureFunction(functionAlias, "Label", "Technology", "Optional Description")
AzureCosmosDb(cosmosDbAlias, "Label", "Technology", "Optional Description")

personAlias --> functionAlias
functionAlias --> cosmosDbAlias

@enduml
```

## Icons

Available icons can be found at [Azure-PlantUML/AzureSymbols.md at master · plantuml-stdlib/Azure-PlantUML](https://github.com/plantuml-stdlib/Azure-PlantUML/blob/master/AzureSymbols.md#azure-symbols).

## Basic diagrams

```plantuml
@startuml Basic usage - Stream processing with Azure Stream Analytics

!define AzurePuml https://raw.githubusercontent.com/plantuml-stdlib/Azure-PlantUML/release/2-2/dist
!includeurl AzurePuml/AzureCommon.puml
!includeurl AzurePuml/Analytics/AzureEventHub.puml
!includeurl AzurePuml/Analytics/AzureStreamAnalyticsJob.puml
!includeurl AzurePuml/Databases/AzureCosmosDb.puml

left to right direction

agent "Device Simulator" as devices #fff

AzureEventHub(fareDataEventHub, "Fare Data", "PK: Medallion HackLicense VendorId; 3 TUs")
AzureEventHub(tripDataEventHub, "Trip Data", "PK: Medallion HackLicense VendorId; 3 TUs")
AzureStreamAnalyticsJob(streamAnalytics, "Stream Processing", "6 SUs")
AzureCosmosDb(outputCosmosDb, "Output Database", "1,000 RUs")

devices --> fareDataEventHub
devices --> tripDataEventHub
fareDataEventHub --> streamAnalytics
tripDataEventHub --> streamAnalytics
streamAnalytics --> outputCosmosDb

@enduml
```

## Simplified View

A simple view is available by loading `!includeurl AzurePuml/AzureSimplified.puml`.

```plantuml
@startuml Two Mode Sample

!define AzurePuml https://raw.githubusercontent.com/plantuml-stdlib/Azure-PlantUML/release/2-2/dist
!includeurl AzurePuml/AzureCommon.puml

!includeurl AzurePuml/AzureSimplified.puml

!includeurl AzurePuml/Analytics/AzureEventHub.puml
!includeurl AzurePuml/Compute/AzureFunction.puml
!includeurl AzurePuml/Databases/AzureCosmosDb.puml
!includeurl AzurePuml/Storage/AzureDataLakeStorage.puml
!includeurl AzurePuml/Analytics/AzureStreamAnalyticsJob.puml
!includeurl AzurePuml/InternetOfThings/AzureTimeSeriesInsights.puml
!includeurl AzurePuml/Identity/AzureActiveDirectoryB2C.puml
!includeurl AzurePuml/DevOps/AzureApplicationInsights.puml


LAYOUT_LEFT_RIGHT

AzureEventHub(rawEventsHubAlias, "Raw Event Hub", "PK: Medallion HackLicense VendorId; 3 TUs")
AzureDataLakeStorage(datalakeAlias, "Data Lake", "GRS")
AzureStreamAnalyticsJob(streamAnalyticsAlias, "Aggregate Events", "6 SUs")
AzureFunction(stateFunctionAlias, "State Processor", "C#, Consumption Plan")
AzureEventHub(aggregatedEventsHubAlias, "Aggregated Hub", "6 TUs")
AzureCosmosDb(stateDBAlias, "State Database", "SQL API, 1000 RUs")
AzureTimeSeriesInsights(timeSeriesAlias, "Time Series", "2 Data Processing Units")

rawEventsHubAlias ----> datalakeAlias
rawEventsHubAlias --> streamAnalyticsAlias
rawEventsHubAlias ---> stateFunctionAlias
streamAnalyticsAlias --> aggregatedEventsHubAlias
aggregatedEventsHubAlias --> timeSeriesAlias
stateFunctionAlias --> stateDBAlias

@enduml
```

## Other examples

Other examples can be found at [Azure-PlantUML/samples at master · plantuml-stdlib/Azure-PlantUML](https://github.com/plantuml-stdlib/Azure-PlantUML/tree/master/samples).
