# Kubernetesシステムアーキテクチャ

これらの図は[dcasati/kubernetes-PlantUML: Kubernetes diagrams using VS Code](https://github.com/dcasati/kubernetes-PlantUML?tab=readme-ov-file#getting-started)より取得しています。

## Getting Started

これはシンプルな描画例です。

```plantuml
@startuml kubernetes

footer Kubernetes Plant-UML
scale max 1024 width

skinparam nodesep 10
skinparam ranksep 10

' Kubernetes
!define KubernetesPuml https://raw.githubusercontent.com/dcasati/kubernetes-PlantUML/master/dist

!includeurl KubernetesPuml/kubernetes_Common.puml
!includeurl KubernetesPuml/kubernetes_Context.puml
!includeurl KubernetesPuml/kubernetes_Simplified.puml

!includeurl KubernetesPuml/OSS/KubernetesSvc.puml
!includeurl KubernetesPuml/OSS/KubernetesPod.puml

actor "User" as userAlias
left to right direction

' Kubernetes Components
Cluster_Boundary(cluster, "Kubernetes Cluster") {
    Namespace_Boundary(ns, "Web") {
        KubernetesSvc(svc, "service", "")
        KubernetesPod(pod1, "web-pod1", "")
        KubernetesPod(pod2, "web-pod2", "")
    }
}

Rel(userAlias,svc,"get HTTP/1.1 index.html", "1")
Rel(svc,pod1,"load Balances to Pods", "2")
Rel(svc,pod2,"load Balances to Pods", "2")
Rel_U(pod1, svc, "serves content", "3")
Rel(svc, userAlias, "return content to", "4")
@enduml
```

## Icons

利用できるアイコンは[dcasati/kubernetes-PlantUML: Kubernetes diagrams using VS Code](https://github.com/dcasati/kubernetes-PlantUML?tab=readme-ov-file#list-of-supported-symbols)にて確認できます。

## 基本的な描画例

もう少し複雑な描画例です。

```plantuml
@startuml kubernetes

footer Kubernetes Plant-UML
scale max 1024 width

skinparam nodesep 10
skinparam ranksep 10

' Kubernetes
!define KubernetesPuml https://raw.githubusercontent.com/dcasati/kubernetes-PlantUML/master/dist

!includeurl KubernetesPuml/kubernetes_Common.puml
!includeurl KubernetesPuml/kubernetes_Context.puml
!includeurl KubernetesPuml/kubernetes_Simplified.puml

!includeurl KubernetesPuml/OSS/KubernetesSvc.puml
!includeurl KubernetesPuml/OSS/KubernetesIng.puml
!includeurl KubernetesPuml/OSS/KubernetesPod.puml
!includeurl KubernetesPuml/OSS/KubernetesRs.puml
!includeurl KubernetesPuml/OSS/KubernetesDeploy.puml
!includeurl KubernetesPuml/OSS/KubernetesHpa.puml

actor "User" as userAlias
left to right direction

' Kubernetes Components
Cluster_Boundary(cluster, "Kubernetes Cluster") {
    Namespace_Boundary(ns, "Back End") {
        KubernetesIng(ingress, "your.domain.com", "")
        KubernetesSvc(svc, "service", "")
        KubernetesPod(pod1, "pod1", "")
        KubernetesPod(pod2, "pod2", "")
        KubernetesPod(pod3, "pod3", "")

        KubernetesRs(rs,"","")
        KubernetesDeploy(deploy,"deployment","")
        KubernetesHpa(hpa, "HPA", "")
    }
}

Rel(userAlias,ingress," ")
Rel(ingress,svc," ")

Rel(svc,pod1," ")
Rel(svc,pod2," ")
Rel(svc,pod3," ")

Rel_U(rs,pod1," ")
Rel_U(rs,pod2," ")
Rel_U(rs,pod3," ")

Rel_U(deploy,rs, " ")
Rel_U(hpa,deploy, " ")

@enduml
```

## 他のアイコンファイルとの組み合わせ

以下は[Azure icons](https://github.com/plantuml-stdlib/Azure-PlantUML)と組み合わせた利用例です。

```plantuml
@startuml kubernetes

footer Kubernetes Plant-UML
scale max 1024 width

skinparam nodesep 10
skinparam ranksep 10

skinparam linetype polyline

' Azure
!define AzurePuml https://raw.githubusercontent.com/RicardoNiepel/Azure-PlantUML/release/2-1/dist

!includeurl AzurePuml/AzureCommon.puml
!includeurl AzurePuml/AzureSimplified.puml

!includeurl AzurePuml/Containers/AzureContainerRegistry.puml
!includeurl AzurePuml/Databases/AzureCosmosDb.puml
!includeurl AzurePuml/Databases/AzureSqlDatabase.puml
!includeurl AzurePuml/DevOps/AzurePipelines.puml
!includeurl AzurePuml/Identity/AzureActiveDirectory.puml
!includeurl AzurePuml/Networking/AzureLoadBalancer.puml
!includeurl AzurePuml/Security/AzureKeyVault.puml

' Kubernetes
!define KubernetesPuml https://raw.githubusercontent.com/dcasati/kubernetes-PlantUML/master/dist

!includeurl KubernetesPuml/kubernetes_Common.puml
!includeurl KubernetesPuml/kubernetes_Context.puml
!includeurl KubernetesPuml/kubernetes_Simplified.puml

!includeurl KubernetesPuml/OSS/KubernetesApi.puml
!includeurl KubernetesPuml/OSS/KubernetesIng.puml
!includeurl KubernetesPuml/OSS/KubernetesPod.puml

actor "DevOps" as devopsAlias
collections "Client Apps" as clientalias
collections "Helm Charts" as helmalias

left to right direction

' Azure Components
AzureActiveDirectory(aad, "\nAzure\nActive Directory", "Global")
AzureContainerRegistry(acr, "ACR", "Canada Central")
AzureCosmosDb(cosmos, "\nCosmos DB", "Global")
AzureKeyVault(keyvault, "\nAzure\nKey Vault", "Global")
AzureLoadBalancer(alb, "\nLoad\nBalancer", "Canada Central")
AzureSqlDatabase(sql, "\nExternal\ndata stores", "Canada Central")
AzurePipelines(ado, "CI/CD\nAzure Pipelines", "Global")

' Kubernetes Components
Cluster_Boundary(cluster, "Kubernetes Cluster") {
    KubernetesApi(KubernetesApi, "Kubernetes API", "")

    Namespace_Boundary(nsFrontEnd, "Front End") {
        KubernetesIng(ingress, "API Gateway", "")
    }

    Namespace_Boundary(nsBackEnd, "Back End") {
        KubernetesPod(KubernetesBE1, "", "")
        KubernetesPod(KubernetesBE2, "", "")
        KubernetesPod(KubernetesBE3, "", "")
    }

    Namespace_Boundary(nsUtil, "Utiliy Services") {
        KubernetesPod(KubernetesUtil1, "", "")
        KubernetesPod(KubernetesUtil2, "","")
    }
}

Rel(devopsAlias, aad, "AUTH")
Rel(helmalias, KubernetesApi, "helm upgrade")

Rel(aad, keyvault, " ")
Rel(KubernetesApi, aad, "RBAC", "ASYNC")

Rel(clientalias, alb, "HTTP", "ASYNC")
Rel(alb, ingress, "HTTP", "ASYNC")

Rel(ingress, KubernetesBE1, " ")
Rel(KubernetesBE1, KubernetesBE2, " ")
Rel(KubernetesBE1, KubernetesBE3, " ")

Rel(KubernetesBE2, sql, " ")
Rel(KubernetesBE3, keyvault, "Pod Identity")
Rel(KubernetesBE3, cosmos, " ")

Rel(ado, acr, "docker push")
Rel_U(KubernetesApi, acr, "docker pull")
@enduml
```

## その他の例

その他の描画例は、[kubernetes-PlantUML/samples at master · dcasati/kubernetes-PlantUML](https://github.com/dcasati/kubernetes-PlantUML/tree/master/samples)にあります。

