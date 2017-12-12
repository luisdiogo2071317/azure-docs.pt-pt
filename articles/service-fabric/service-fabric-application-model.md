---
title: "Modelo de aplicação de Service Fabric do Azure | Microsoft Docs"
description: "Como modelo e descrevem as aplicações e serviços no Service Fabric."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: mani-ramaswamy
ms.assetid: 17a99380-5ed8-4ed9-b884-e9b827431b02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/07/2017
ms.author: ryanwi
ms.openlocfilehash: d5f6fbb9d9c0bc0d9762f8d6b4b4eb3b02d29adc
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2017
---
# <a name="model-an-application-in-service-fabric"></a>Modelar uma aplicação no Service Fabric
Este artigo fornece uma descrição geral do modelo de aplicação de Service Fabric do Azure e como definir uma aplicação e do serviço através de ficheiros de manifesto.

## <a name="understand-the-application-model"></a>Compreender o modelo de aplicação
Uma aplicação é uma coleção de serviços constituintes que efetuar determinada uma função ou funções. Um serviço executa uma função completa e autónoma e pode iniciar e executar independentemente outros serviços.  Um serviço é composto por código, configuração e dados. Para cada serviço, código consiste dos binários do executável, configuração consiste em definições de serviço que podem ser carregadas em tempo de execução e dados constituem em dados estáticos arbitrários a ser consumidos pelo serviço. Cada componente neste modelo de aplicação hierárquica pode ser atualizado e com a versão independentemente.

![O modelo de aplicação de Service Fabric][appmodel-diagram]

Um tipo de aplicação é uma categorização de uma aplicação e é composta por um pacote de tipos de serviço. Um tipo de serviço é uma categorização de um serviço. A categorização pode ter configurações e definições diferentes, mas a funcionalidade principal permanece igual. As instâncias de um serviço são variações de configuração de serviço diferentes do mesmo tipo de serviço.  

Classes (ou "tipos") de aplicações e serviços são descritas através de ficheiros XML (os manifestos da aplicação e manifestos de serviço).  Os manifestos descrevem a aplicações e serviços e modelos em relação às quais as aplicações podem ser instanciadas do cluster arquivo de imagens.  Manifestos descritos detalhadamente no [aplicações e serviço manifestos](service-fabric-application-and-service-manifests.md). A definição de esquema para o ficheiro ServiceManifest.xml e ApplicationManifest.xml é instalada com o SDK de Service Fabric e ferramentas para *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*. O esquema XML é descrito da [documentação de esquema ServiceFabricServiceModel.xsd](service-fabric-service-model-schema.md).

O código para instâncias de aplicações diferentes run processos separados, mesmo quando alojadas pelo mesmo nó de Service Fabric. Além disso, o ciclo de vida de cada instância da aplicação pode ser gerido (por exemplo, atualizado) independentemente. O diagrama seguinte mostra como tipos de aplicação são compostas por tipos de serviço, que por sua vez, são compostos de código, configuração e pacotes de dados. Para simplificar o diagrama, apenas o código/configuração/dados de pacotes para `ServiceType4` são apresentados, apesar de cada tipo de serviço inclui alguns ou todos os os tipos de pacotes.

![Tipos de aplicação de Service Fabric e os tipos de serviço][cluster-imagestore-apptypes]

Pode ser ativo no cluster de uma ou mais instâncias de um tipo de serviço. Por exemplo, instâncias de serviço com estado ou réplicas, atingir elevada fiabilidade através da replicação de estado entre réplicas localizadas em diferentes nós do cluster. Replicação essencialmente fornece redundância estar disponível, mesmo se falha um nó num cluster do serviço. A [particionada serviço](service-fabric-concepts-partitioning.md) mais divide o respetivo estado (e padrões de acesso para esse Estado) em nós do cluster.

O diagrama seguinte mostra a relação entre as aplicações e instâncias de serviço, partições e réplicas.

![As partições e réplicas dentro de um serviço][cluster-application-instances]

> [!TIP]
> Pode ver o esquema de aplicações num cluster utilizando a ferramenta de Service Fabric Explorer disponível na http://&lt;yourclusteraddress&gt;: 19080/Explorer. Para obter mais informações, consulte [visualizar o cluster com o Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
> 
> 


## <a name="next-steps"></a>Passos seguintes
- Saiba mais sobre [escalabilidade de aplicação](service-fabric-concepts-scalability.md).
- Saiba mais sobre o serviço [estado](service-fabric-concepts-state.md), [criação de partições](service-fabric-concepts-partitioning.md), e [disponibilidade](service-fabric-availability-services.md).
- Leia sobre como aplicações e serviços são definidos na [aplicações e serviço manifestos](service-fabric-application-and-service-manifests.md).
- [Aplicação aloja modelos](service-fabric-hosting-model.md) descrevem a relação entre réplicas (ou instâncias) de um serviço implementado e o processo de anfitrião do serviço.

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png


