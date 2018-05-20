---
title: As variáveis de ambiente do Azure Service Fabric | Microsoft Docs
description: Documentação de referência para as variáveis de ambiente do Service Fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: reference
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/07/2017
ms.author: mikhegn
ms.openlocfilehash: f7c36fec7ff58c225e41899e8264ca1dde95ce7c
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="service-fabric-environment-variables"></a>Variáveis de ambiente do Service Fabric

Service Fabric tem variáveis de ambiente incorporada definido para cada instância de serviço. A lista completa de variáveis de ambiente abaixo é:

| Variável de ambiente                         | Descrição                                                            | Exemplo                                                              |
|----------------------------------------------|------------------------------------------------------------------------|----------------------------------------------------------------------|
| Fabric_ApplicationName                       | O nome de uri do recurso de infraestrutura da aplicação                                 | fabric:/MyApplication                                                |
| Fabric_CodePackageName                       | O nome do pacote do código a que pertence o processo              | Código                                                                 |
| Fabric_Endpoint\_IPOrFQDN\_*ServiceEndpointName*     | O endereço ip ou FQDN do ponto final                                 | 10.0.0.1                                                     |
| Fabric\_Endpoint\_*ServiceEndpointName*              | Número de porta para o ponto final                                  | 8234                                                                 |
| Fabric_Folder_App_Log                        | Pasta de registo                                                             | C:\\\\Data\\\\_App\\\\_Node_0\\\\MyApplicationType_App12\\\\log      |
| Fabric_Folder_App_Temp                       | Pasta temporária                                                            | C:\\\\Data\\\\_App\\\\_Node_0\\\\MyApplicationType_App12\\\\temp     |
| Fabric_Folder_App_Work                       | Pastas de trabalho                                                            | C:\\\\Data\\\\_App\\\\_Node_0\\\\MyApplicationType_App12\\\\work     |
| Fabric_Folder_Application                    | Na pasta raiz de aplicações                                           | C:\\\\Data\\\\_App\\\\_Node_0\\\\MyApplicationType_App12             |
| Fabric_IsContainerHost                       | Um booleano que especifica se o processo é um contentor                   | false                                                                |
| Fabric_NodeId                                | O ID de nó do nó de execução do processo                            | bf865279ba277deb864a976fbf4c200e                                     |
| Fabric_NodeIPOrFQDN                          | O IP ou FQDN do nó, conforme especificado no cluster do manifesto do ficheiro. | localhost ou 10.0.0.1                                                |
| Fabric_NodeName                              | O nome de nó do nó de execução do processo                          | _Node_0                                                              |
| Fabric_ServiceName                           | O nome do serviço, se o serviço está alojado no modo de ExclusiveProcess. Este valor da variável só está disponível se criar o serviço com ServicePackageActivationMode ExclusiveProcess.  | MyService                                               |
| Fabric_ServicePackageActivationId            | The ServicePackageActivationId                                         | UM GUID                                                               |
| Fabric_ServicePackageName                    | Nome do pacote de serviço do processo faz parte                     | Web1Pkg                                                              |

Variáveis de ambiente interno utilizadas pelo tempo de execução do Service Fabric:

- Fabric_ApplicationHostId
- Fabric_ApplicationHostType
- Fabric_ApplicationId
- Fabric_CodePackageInstanceId
- Fabric_CodePackageInstanceSeqNum
- Fabric_RuntimeConnectionAddress
- Fabric_ServicePackageActivationGuid
- Fabric_ServicePackageInstanceId
- Fabric_ServicePackageInstanceSeqNum
- Fabric_ServicePackageVersionInstance
- FabricActivatorAddress
- FabricPackageFileName
- HostedServiceName