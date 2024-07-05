# Azureシステムアーキテクチャ

これらの図は[plantuml-stdlib/Azure-PlantUML: PlantUML sprites, macros, and other includes for Azure services](https://github.com/plantuml-stdlib/Azure-PlantUML?tab=readme-ov-file)より取得しています。

## Getting Started

Hello World的な図例です。

```plantuml
@startuml Hello World
!define AzurePuml https://raw.githubusercontent.com/plantuml-stdlib/Azure-PlantUML/release/2-2/dist
!includeurl AzurePuml/AzureCommon.puml
!includeurl AzurePuml/Databases/all.puml
!includeurl AzurePuml/Compute/AzureFunction.puml

actor "人物" as personAlias
AzureFunction(functionAlias, "ラベル", "テクノロジー", "任意の備考")
AzureCosmosDb(cosmosDbAlias, "ラベル", "テクノロジー", "任意の備考")

personAlias --> functionAlias
functionAlias --> cosmosDbAlias

@enduml
```

## アイコン

利用できるアイコンは[Azure-PlantUML/AzureSymbols.md at master · plantuml-stdlib/Azure-PlantUML](https://github.com/plantuml-stdlib/Azure-PlantUML/blob/master/AzureSymbols.md#azure-symbols)にて確認できます。
## 基本的な描画例

基本的な描画例です。

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

## シンプルな描画例

シンプルな描画を行う場合には、 `!includeurl AzurePuml/AzureSimplified.puml` を使います。

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

## その他の例

その他の描画例は、[Azure-PlantUML/samples at master · plantuml-stdlib/Azure-PlantUML](https://github.com/plantuml-stdlib/Azure-PlantUML/tree/master/samples)にあります。
