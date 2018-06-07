---
title: Ver registos de contentores no Service Fabric do Azure | Microsoft Docs
description: Descreve como visualizar os registos do contentor para um serviços de contentor do Service Fabric em execução com o Service Fabric Explorer.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/15/2018
ms.author: ryanwi
ms.openlocfilehash: c8b6bc791700e6811f5681ee70329e4d2ac05991
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34824616"
---
# <a name="view-logs-for-a-service-fabric-container-service"></a>Ver registos para um serviço de contentor do Service Fabric
Azure Service Fabric é orchestrator um contentor e suporta [contentores de Linux e Windows](service-fabric-containers-overview.md).  Este artigo descreve como ver registos do contentor de um serviço de contentor em execução ou de um contentor Inativos para que possam diagnosticar e resolver problemas.

## <a name="access-the-logs-of-a-running-container"></a>Aceder a registos de um contentor em execução
Os registos do contentor podem ser acedidos utilizando [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).  Num browser, abrir Service Fabric Explorer a partir do ponto final de gestão do cluster, navegando até [ http://mycluster.region.cloudapp.azure.com:19080/Explorer ](http://mycluster.region.cloudapp.azure.com:19080/Explorer).  

Contentor registos estão localizados no nó de cluster que a instância do serviço de contentor está em execução. Por exemplo, obter os registos do contentor web front-end do [votar do Linux na aplicação de exemplo](service-fabric-quickstart-containers-linux.md). Na vista de árvore, expanda **Cluster**>**aplicações**>**VotingType**>**fabric: / voto / azurevotefront**.  Em seguida, expanda a partição (d1aa737e-f22a-e347-be16-eec90be24bc1, neste exemplo) e que o contentor está em execução no nó de cluster *_lnxvm_0*.

Na vista de árvore, localize o pacote do código no *_lnxvm_0* nó expandindo **nós**>**_lnxvm_0**>**fabric: / votar**  > **azurevotfrontPkg**>**código pacotes**>**código**.  Em seguida, selecione o **registos do contentor** opção para apresentar os registos do contentor.

![Plataforma do Service Fabric][Image1]

## <a name="access-the-logs-of-a-dead-or-crashed-container"></a>Aceder a registos de um contentor Inativos ou falhado
A partir de v6.2, pode também obter os registos para utilizar um contentor Inativos ou falhado [REST APIs](/rest/api/servicefabric/sfclient-index) ou [CLI de recursos de infraestrutura de serviço (SFCTL)](service-fabric-cli.md) comandos.

### <a name="set-container-retention-policy"></a>Definir a política de retenção de contentores
Para ajudar a diagnosticar falhas no arranque de contentores, o Service Fabric (versão 6.1 ou superior) suporta a retenção de contentores que terminaram ou cujo arranque falhou. Esta política pode ser definida no ficheiro **ApplicationManifest.xml**, conforme mostrado no fragmento seguinte:
```xml
 <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" ContainersRetentionCount="2"  RunInteractive="true"> 
 ```

A definição **ContainersRetentionCount** especifica o número de contentores que vão ser retidos quando falham. Se não for especificado um valor negativo, todos os contentores com falhas serão mantidos. Quando o **ContainersRetentionCount** atributo não for especificado, não existem contentores serão mantidos. O atributo **ContainersRetentionCount** também suporta Parâmetros de Aplicação, para que os utilizadores possam especificar valores diferentes para clusters de teste e produção. Utilize restrições de posicionamento para segmentar o serviço de contentores para um nó particular ao utilizar esta funcionalidade para impedir a passagem do serviço de contentores para outros nós. Quaisquer contentores retidos que utilizem esta funcionalidade têm de ser removidos manualmente.

### <a name="rest"></a>REST
Utilize o [obter contentor registos implementado no nó](/rest/api/servicefabric/sfclient-api-getcontainerlogsdeployedonnode) operação para obter os registos para um contentor falhado. Especifique o nome do nó que estava a executar no contentor, nome da aplicação, nome do manifesto do serviço e o nome de pacote do código.  Especifique `&Previous=true`. A resposta irá conter os registos do contentor para o contentor inactivo da instância de pacote do código.

O URI do pedido tem o seguinte formato:

```
/Nodes/{nodeName}/$/GetApplications/{applicationId}/$/GetCodePackages/$/ContainerLogs?api-version=6.2&ServiceManifestName={ServiceManifestName}&CodePackageName={CodePackageName}&Previous={Previous}
```

Exemplo de pedido:
```
GET http://localhost:19080/Nodes/_Node_0/$/GetApplications/SimpleHttpServerApp/$/GetCodePackages/$/ContainerLogs?api-version=6.2&ServiceManifestName=SimpleHttpServerSvcPkg&CodePackageName=Code&Previous=true  
```

Corpo de resposta 200:
```json
{   "Content": "Exception encountered: System.Net.Http.HttpRequestException: Response status code does not indicate success: 500 (Internal Server Error).\r\n\tat System.Net.Http.HttpResponseMessage.EnsureSuccessStatusCode()\r\n" } 
```

### <a name="service-fabric-sfctl"></a>Recursos de infraestrutura do serviço (SFCTL)
Utilize o [sfctl get-contentor-registos do serviço](service-fabric-sfctl-service.md) comando para obter os registos para um contentor falhado.  Especifique o nome do nó que estava a executar no contentor, nome da aplicação, nome do manifesto do serviço e o nome de pacote do código. Especifique o `-previous` sinalizador.  A resposta irá conter os registos do contentor para o contentor inactivo da instância de pacote do código.

```
sfctl service get-container-logs --node-name _Node_0 --application-id SimpleHttpServerApp --service-manifest-name SimpleHttpServerSvcPkg --code-package-name Code –previous
```
Resposta:
```json
{   "content": "Exception encountered: System.Net.Http.HttpRequestException: Response status code does not indicate success: 500 (Internal Server Error).\r\n\tat System.Net.Http.HttpResponseMessage.EnsureSuccessStatusCode()\r\n" }
```

## <a name="next-steps"></a>Passos Seguintes
- Completar o [criar um tutorial de aplicação do Linux contentor](service-fabric-tutorial-create-container-images.md).
- Saiba mais sobre [Service Fabric e os contentores](service-fabric-containers-overview.md)

[Image1]: media/service-fabric-containers-view-logs/view-container-logs-sfx.png
