# GCPシステムアーキテクチャ

これらの図は[davidholsgrove/gcp-icons-for-plantuml: PlantUML sprites, macros, and other includes for Google Cloud Platform services and resources](https://github.com/davidholsgrove/gcp-icons-for-plantuml)より取得しています。

## Getting Started

Hello World的な図例です。

```plantuml
@startuml Hello World
!define GCPPuml https://raw.githubusercontent.com/davidholsgrove/gcp-icons-for-plantuml/master/dist
!includeurl GCPPuml/GCPCommon.puml
!includeurl GCPPuml/DeveloperTools/all.puml
!includeurl GCPPuml/Storage/CloudStorage.puml

actor "人物" as personAlias
CloudToolsforVisualStudio(desktopAlias, "ラベル", "テクノロジー", "任意の備考")
CloudStorage(storageAlias, "ラベル", "テクノロジー", "任意の備考")

personAlias --> desktopAlias
desktopAlias --> storageAlias

@enduml
```

## Icons

Available icons can be found at [gcp-icons-for-plantuml/GCPSymbols.md at master · davidholsgrove/gcp-icons-for-plantuml](https://github.com/davidholsgrove/gcp-icons-for-plantuml/blob/master/GCPSymbols.md).

## 基本的な描画例

基本的な描画例です。

```plantuml
@startuml Basic Usage

!define GCPPuml https://raw.githubusercontent.com/davidholsgrove/gcp-icons-for-plantuml/master/dist
!includeurl GCPPuml/GCPCommon.puml
!includeurl GCPPuml/InternetOfThings/CloudIoTCore.puml
!includeurl GCPPuml/DataAnalytics/CloudPubSub.puml

left to right direction

agent "イベント発行" as event #fff

CloudIoTCore(iotRule, "アクションエラールール", "Kinesisが失敗したらエラー")
CloudPubSub(eventStream, "IoTイベント", "2 shards")
CloudPubSub(errorQueue, "ルールエラーキュー", "ルールアクションの失敗")

event --> iotRule : JSONメッセージ
iotRule --> eventStream : メッセージ
iotRule --> errorQueue : 失敗したアクションのメッセージ

@enduml
```

## シンプルな描画例

シンプルな描画を行う場合には、 `!include GCPPuml/GCPSimplified.puml` を使います。

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

## その他の例

その他の描画例は、[gcp-icons-for-plantuml/examples at master · davidholsgrove/gcp-icons-for-plantuml](https://github.com/davidholsgrove/gcp-icons-for-plantuml/tree/master/examples)にあります。
