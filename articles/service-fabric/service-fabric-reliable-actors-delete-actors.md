---
title: Eliminar atores do Service Fabric do Azure | Microsoft Docs
description: Saiba como eliminar manualmente o serviço de recursos de infraestrutura Reliable Actors e o respetivo estado.
services: service-fabric
documentationcenter: .net
author: amanbha
manager: timlt
editor: vturecek
ms.assetid: b91384cc-804c-49d6-a6cb-f3f3d7d65a8e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/19/2018
ms.author: amanbha
ms.openlocfilehash: 32153a916ed9c868c002f4b69f9f6f3cdcc3c9d5
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="delete-reliable-actors-and-their-state"></a>Eliminar Reliable Actors e o respetivo estado
Recolha de lixo de atores desativados limpa apenas o objeto de ator, mas não remove os dados armazenados no Gestor de estado de um ator. Quando um ator for reativado, dos dados é novamente disponibilizados ao mesmo através do Gestor de estado. Nos casos em que atores armazenar dados no Gestor de estado e estão desativados mas nunca reativados, poderá ser necessário limpar os seus dados.

O [serviço de Atores](service-fabric-reliable-actors-platform.md) fornece uma função para a eliminação dos atores de um autor da chamada remota:

```csharp
ActorId actorToDelete = new ActorId(id);

IActorService myActorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

await myActorServiceProxy.DeleteActorAsync(actorToDelete, cancellationToken)
```
```Java
ActorId actorToDelete = new ActorId(id);

ActorService myActorServiceProxy = ActorServiceProxy.create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

myActorServiceProxy.deleteActorAsync(actorToDelete);
```

Eliminar um ator tem os seguintes efeitos dependendo se pretende ou não está atualmente ativa o ator:

* **Ator Active Directory**
  * Ator é removido da lista de atores Active Directory e está desativada.
  * O estado é eliminado permanentemente.
* **Ator inativo**
  * O estado é eliminado permanentemente.

Um ator não é possível chamar eliminar em si próprio a partir de um dos respectivos métodos de ator porque não é possível eliminar o ator ao executar dentro de um contexto de chamada de ator em que o tempo de execução tem de obter um bloqueio em torno da chamada de ator para impor o acesso single-threaded.

Para obter mais informações sobre Reliable Actors, leia o seguinte:
* [Os temporizadores de ator e lembretes](service-fabric-reliable-actors-timers-reminders.md)
* [Eventos de ator](service-fabric-reliable-actors-events.md)
* [Reentrancy ator](service-fabric-reliable-actors-reentrancy.md)
* [Monitorização de desempenho e diagnóstico de ator](service-fabric-reliable-actors-diagnostics.md)
* [Documentação de referência da API de ator](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [Código de exemplo do c#](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Código de exemplo de Java](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-lifecycle/garbage-collection.png
