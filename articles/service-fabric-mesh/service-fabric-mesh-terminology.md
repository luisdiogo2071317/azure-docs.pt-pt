---
title: Terminologia do malha de recursos de infraestrutura do serviço do Azure | Documentos da Microsoft
description: Conheça os termos comuns para o modo de malha do Azure Service Fabric.
services: service-fabric-mesh
keywords: ''
author: rwike77
ms.author: ryanwi
ms.date: 07/12/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 672e27bf53679c52dab8d42a52378aa90eba33cb
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39114315"
---
# <a name="service-fabric-mesh-terminology"></a>Terminologia de malha do Service Fabric

Malha do Azure Service Fabric é um serviço totalmente gerido que permite aos programadores implementar aplicações de microsserviços sem gerir máquinas virtuais, armazenamento, ou de rede. Este artigo fornece detalhes sobre a terminologia utilizada pelo Azure Service Fabric em malha para compreender os termos usados na documentação.

## <a name="service-fabric"></a>Service Fabric

Service Fabric é uma plataforma de sistemas distribuídos de código-fonte aberto que facilita o empacotamento, implementação e a gestão de microsserviços dimensionáveis e fiáveis. O Service Fabric é o orquestrador que capacita a malha de recursos de infraestrutura do serviço. O Service Fabric fornece opções para como criar e executar as suas aplicações de microsserviços. Pode utilizar qualquer estrutura para escrever seus serviços e escolha onde pretende executar a aplicação a partir de várias opções de ambiente.

## <a name="deployment-and-application-models"></a>Modelos de implementação e aplicação 

Para implementar os seus serviços, terá de descrever como deve ser executado. O Service Fabric suporta três modelos de implementação diferentes:

### <a name="resource-model"></a>Modelo de recursos
Os recursos são tudo o que podem ser implementados individualmente para o Service Fabric, incluindo aplicações, serviços, redes e volumes. Recursos são definidos usando um ficheiro YAML ou um ficheiro JSON com o esquema do modelo de recursos do Azure.

O modelo de recursos é a forma mais simples para descrever as suas aplicações do Service Fabric. Seu foco principal é sobre a implantação simple e gestão de serviços em contentores.

Podem ser implementados recursos em qualquer lugar que executa a Service Fabric.

### <a name="native-model"></a>Modelo nativo
O modelo de aplicação nativa fornece seus aplicativos com acesso completo de nível baixo para o Service Fabric. Aplicações e serviços são definidos como tipos registados nos ficheiros de manifestos XML.

O modelo nativo suporta o framework de Reliable Services, que fornece acesso a APIs de runtime do Service Fabric e gestão de clusters de APIs em c# e Java. O modelo nativo também suporta arbitrários contentores e executáveis.

O modelo nativo não é suportado no ambiente de malha do Service Fabric.

### <a name="docker-compose"></a>Docker Compose 
[Docker Compose](https://docs.docker.com/compose/) faz parte do projeto Docker. O Service Fabric fornece suporte limitado para implantar aplicativos usando o modelo de Docker Compose.

## <a name="environments"></a>Ambientes

Service Fabric é uma tecnologia de plataforma de código aberto baseados em vários produtos e serviços diferentes. A Microsoft fornece as seguintes opções:

 - **Malha de recursos de infraestrutura do serviço**: um serviço totalmente gerido para executar aplicações do Service Fabric no Microsoft Azure.
 - **O Azure Service Fabric**: alojados no Azure a oferta de cluster do Service Fabric. Ele fornece integração entre o Service Fabric e a infraestrutura do Azure, juntamente com a gestão de atualização e configuração de clusters do Service Fabric.
 - **Autónomo do Service Fabric**: um conjunto de ferramentas de instalação e configuração para [implementar clusters do Service Fabric em qualquer lugar](/azure/service-fabric/service-fabric-deploy-anywhere) (no local ou em qualquer fornecedor de cloud). Não é gerido pelo Azure.
 - **Cluster de desenvolvimento do Service Fabric**: fornece uma experiência de desenvolvimento local no Windows, Linux ou Mac para desenvolvimento de aplicações do Service Fabric.

## <a name="environment-framework-and-deployment-model-support-matrix"></a>Matriz de suporte de ambiente, o framework e o modelo de implementação
Ambientes diferentes tem vários níveis de suporte de estruturas e modelos de implementação. A tabela seguinte descreve a estrutura de suporte e combinações de modelo de implementação.

|Modelo de Frameworks\Deployment |Modelo de recursos |Modelo nativo | Compor|
|---|---|---|---|
|Reliable Actors e de serviços fiáveis |Não suportado |Suportadas |Não suportado |
|Qualquer estrutura ou linguagem |Suportado em contentores |Suportado como processos e em contentores |Suportado em contentores |

A tabela seguinte descreve o ambiente com suporte e combinações de modelo de implementação.

|Modelo de Environment\Deployment |Modelo de recursos |Modelo nativo |Compor |
|---|---|---|---|
|Malha de recursos de infraestrutura do serviço do Azure |Suportadas |Não suportado|Não suportado |
|Todos os outros ambientes |Suportado (pré-requisitos para funcionar num ambiente de ter alguns recursos) |Suportadas |Suporte limitado |

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre a malha de recursos de infraestrutura do serviço, leia a visão geral:
- [Descrição geral de malha do Service Fabric](service-fabric-mesh-overview.md)
