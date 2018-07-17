---
title: Opções de armazenamento no Azure Service Fabric em malha de estado | Documentos da Microsoft
description: Saiba mais sobre armazenamento fiável do Estado em aplicativos de malha de recursos de infraestrutura do serviço de mensagens em fila em execução no Azure Service Fabric em malha.
services: service-fabric-mesh
keywords: ''
author: rwike77
ms.author: ryanwi
ms.date: 07/12/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 6aa268cf56bfb8be9c27a9e0d9e5c9f4464b0c9d
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39076459"
---
# <a name="state-management-with-service-fabric"></a>Gerenciamento de estado com o Service Fabric
Service Fabric suporta muitas opções diferentes para o armazenamento de estado. Para uma descrição geral conceptual dos padrões de gestão de estado e o Service Fabric, veja [conceitos de recursos de infraestrutura do serviço: estado](/azure/service-fabric/service-fabric-concepts-state). Estes mesmos conceitos aplicam-se se os serviços são executados dentro ou fora da malha de recursos de infraestrutura do serviço. 

## <a name="state-storage-options-in-azure-service-fabric-mesh"></a>Opções de armazenamento de estado na malha do Azure Service Fabric
Com a malha do Service Fabric, pode facilmente implementar uma nova aplicação e ligá-la a um arquivo de dados existente, alojado no Azure. Além de usar qualquer base de dados remoto, existem várias opções para armazenar dados, dependendo se o serviço deseja armazenamento local ou remoto. 

* Dados replicados armazenados localmente
  * Coleções fiáveis (não disponíveis em pré-visualização)
    * Uma biblioteca que implementa as estruturas de dados, como filas e pares chave-valor para utilizar no serviço
    * Isto proporciona a maneira mais fácil e rápida para interagir com dados, ao mesmo tempo, a partição fácil roteamento em combinação com inteligente encaminhamento na malha de recursos de infraestrutura do serviço
  * Controladores de volume do Service Fabric (não disponível em pré-visualização)
    * Um driver de volume do docker para montar um volume local para um contentor
    * Isto dá-lhe a flexibilidade ultimate em armazenar os dados localmente, por meio de qualquer API, que suporta o armazenamento de ficheiros.

* Armazenamento remoto
  * Controladores de volume de ficheiros do Azure
    * Um controlador de volume do docker para montar uma partilha de ficheiros do Azure para um contentor
    * Dá-lhe um menos eficiente, mas a opção de dados também barato completa flexível e confiável, por meio de qualquer API, que suporta o armazenamento de ficheiros.
    * [Guia de procedimentos: implementar uma aplicação com o volume de ficheiros do Azure](service-fabric-mesh-howto-deploy-app-azurefiles-volume.md)
    
## <a name="next-steps"></a>Passos Seguintes

Para obter informações sobre o modelo de aplicativo, consulte [recursos de infraestrutura do serviço](service-fabric-mesh-service-fabric-resources.md)
