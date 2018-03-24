---
title: Reliable Actors em recursos de infraestrutura de serviço | Microsoft Docs
description: Descreve como Reliable Actors são em camadas sobre Reliable Services e utilizar as funcionalidades de plataforma do Service Fabric.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: amanbha
ms.assetid: 45839a7f-0536-46f1-ae2b-8ba3556407fb
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 3/9/2018
ms.author: vturecek
ms.openlocfilehash: 088f56f33c85d3c590acf4a2eaa660a9d586f7ec
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="how-reliable-actors-use-the-service-fabric-platform"></a>Como os Reliable Actors utilizam a plataforma Service Fabric
Este artigo explica como funcionam os Reliable Actors na plataforma do Azure Service Fabric. Chamado Reliable Actors executadas uma arquitetura que está alojada numa implementação de um serviço fiável com monitorização de estado a *serviço de atores*. O serviço de atores contém todos os componentes necessários para gerir o ciclo de vida e a mensagem de emissão para os atores:

* O tempo de execução de Ator gere o ciclo de vida, recolha de lixo e impõe o acesso single-threaded.
* Um serviço de escuta de comunicação remota de serviço ator aceita as chamadas de acesso remoto para atores e envia-as para um distribuidor para encaminhar para a instância de ator adequado.
* O fornecedor de estado de Ator encapsula num wrapper a fornecedores de estado (por exemplo, o fornecedor de estado coleções fiável) e fornece um adaptador de gestão do Estado de ator.

Estes componentes em conjunto formam a estrutura de Ator fiável.

## <a name="service-layering"></a>Dispor de serviço
Porque o próprio serviço de atores é um serviço fiável, todos os o [modelo de aplicação](service-fabric-application-model.md), ciclo de vida, [empacotamento](service-fabric-package-apps.md), [implementação](service-fabric-deploy-remove-applications.md), atualizar e dimensionar os conceitos das fiável Serviços aplicam-se da mesma forma para serviços de atores.

![Dispor de serviço de atores][1]

O diagrama anterior mostra a relação entre as estruturas de aplicações de Service Fabric e o código de utilizador. Elementos azuis representam a estrutura da aplicação Reliable Services, laranja representa o framework de Ator fiável e verde representa o código de utilizador.

No Reliable Services, o seu serviço herda o `StatefulService` classe. Esta classe é próprio derivado `StatefulServiceBase` (ou `StatelessService` para serviços sem monitorização de estado). Reliable Actors, utiliza o serviço de atores. O serviço de atores é uma implementação diferente de `StatefulServiceBase` classe que implementa o padrão de ator onde executam a sua atores. Porque o próprio serviço de atores é apenas uma implementação de `StatefulServiceBase`, pode escrever o seu próprio serviço que deriva de `ActorService` e implementar funcionalidades de nível de serviço da mesma forma que faria ao herdar `StatefulService`, tais como:

* Cópia de segurança do serviço e de restauro.
* Partilhado funcionalidade para todos os atores, por exemplo, um disjuntor automático.
* Chamadas de procedimento remoto do serviço de atores em si e em cada ator individuais.

> [!NOTE]
> Serviços com monitorização de estado não são atualmente suportados em Java/Linux.

Para obter mais informações, consulte [implementar funcionalidades de nível de serviço no seu serviço de atores](service-fabric-reliable-actors-using.md).

## <a name="application-model"></a>Modelo de aplicação
Serviços de atores são Reliable Services, pelo que o modelo de aplicação é o mesmo. No entanto, as ferramentas de compilação do framework ator geram alguns dos ficheiros de modelo de aplicação por si.

### <a name="service-manifest"></a>Manifesto de serviço
As ferramentas de compilação do framework ator geram automaticamente o conteúdo do ficheiro do seu serviço de atores ServiceManifest.xml. Este ficheiro inclui:

* Tipo de serviço de atores. O nome do tipo é gerado com base no nome do seu ator projeto. Com base no atributo persistência no seu ator, o sinalizador de HasPersistedState seja também definido em conformidade.
* Pacote do código.
* Pacote de configuração.
* Recursos e os pontos finais.

### <a name="application-manifest"></a>manifesto da aplicação
As ferramentas de compilação do framework ator criar automaticamente uma definição de serviço predefinido para o seu serviço de atores. As ferramentas de compilação preencher as propriedades de serviço predefinido:

* Contagem de conjunto de réplicas é determinada pelo atributo persistência no seu ator. Sempre que o atributo de persistência em seu ator é alterado, a contagem de conjunto de réplica na definição de serviço predefinido é reposta em conformidade.
* Esquema de partição e o intervalo são definidos para Uniform Int64 com o intervalo de chave completo Int64.

## <a name="service-fabric-partition-concepts-for-actors"></a>Conceitos de partição de recursos de infraestrutura do serviço de atores
Serviços de atores são particionado com monitorização de estado. Cada partição de um serviço de atores contém um conjunto de atores. As partições de serviço são distribuídos automaticamente por através de vários nós no Service Fabric. Instâncias de ator são distribuídas, como resultado.

![Ator criação de partições e de distribuição][5]

Reliable Services podem ser criados com esquemas de partição diferentes e os intervalos de chaves de partição. O serviço de atores utiliza o esquema de partições Int64 com o intervalo de chave Int64 completo para mapear atores para partições.

### <a name="actor-id"></a>ID de actor
Cada ator que é criado no serviço tem um ID exclusivo associado, representado pelo `ActorId` classe. `ActorId` é um valor de ID opaco que pode ser utilizado para distribuição uniform de atores entre as partições de serviço através da geração de IDs aleatórios:

```csharp
ActorProxy.Create<IMyActor>(ActorId.CreateRandom());
```
```Java
ActorProxyBase.create<MyActor>(MyActor.class, ActorId.newId());
```


Cada `ActorId` ser codificado para um Int64. Esta é a razão pela qual o serviço de atores tem de utilizar um esquema de partições Int64 com o intervalo de chave completo Int64. No entanto, os valores de ID personalizados podem ser utilizados para um `ActorID`, incluindo GUIDs/UUIDs não, as cadeias e Int64s.

```csharp
ActorProxy.Create<IMyActor>(new ActorId(Guid.NewGuid()));
ActorProxy.Create<IMyActor>(new ActorId("myActorId"));
ActorProxy.Create<IMyActor>(new ActorId(1234));
```
```Java
ActorProxyBase.create(MyActor.class, new ActorId(UUID.randomUUID()));
ActorProxyBase.create(MyActor.class, new ActorId("myActorId"));
ActorProxyBase.create(MyActor.class, new ActorId(1234));
```

Quando estiver a utilizar cadeias e GUIDs/UUIDs não, os valores são codificados para um Int64. No entanto, quando que está a explicitamente a fornecer Int64 para um `ActorId`, o Int64 mapeará diretamente a uma partição sem obter hash. Pode utilizar esta técnica para controlo de partição de atores são colocados em.


## <a name="next-steps"></a>Passos Seguintes
* [Gestão do Estado de ator](service-fabric-reliable-actors-state-management.md)
* [Coleção de ciclo de vida e libertação da memória de ator](service-fabric-reliable-actors-lifecycle.md)
* [Documentação de referência da API de atores](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [Código de exemplo do .NET](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Código de exemplo de Java](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png
