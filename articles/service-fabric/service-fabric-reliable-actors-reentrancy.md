---
title: Reentrada no actors do Azure Service Fabric | Documentos da Microsoft
description: Introdução ao reentrada dos Reliable Actors do Service Fabric.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: amanbha
ms.assetid: be23464a-0eea-4eca-ae5a-2e1b650d365e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: c7a4066a949ad6e66c45dff67f1e80801f2fa4cd
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44055265"
---
# <a name="reliable-actors-reentrancy"></a>Reentrada do Reliable Actors
O tempo de execução do Reliable Actors, por predefinição, permite reentrância com base no contexto de chamadas lógicas. Isto permite atores são reentrantes se eles estiverem na mesma cadeia de contexto de chamada. Por exemplo, o Ator A envia uma mensagem para o Ator B, que envia uma mensagem para c de Ator. Como parte do processamento da mensagem, se chama o C de Ator Ator A, a mensagem é reentrantes, pelo que terá permissão. Outras mensagens que fazem parte de um contexto de chamada diferente serão bloqueadas no Ator A até terminar de processamento.

Existem duas opções disponíveis para reentrância de ator definida no `ActorReentrancyMode` enum:

* `LogicalCallContext` (comportamento predefinido)
* `Disallowed` -Desativa reentrância

```csharp
public enum ActorReentrancyMode
{
    LogicalCallContext = 1,
    Disallowed = 2
}
```
```Java
public enum ActorReentrancyMode
{
    LogicalCallContext(1),
    Disallowed(2)
}
```
Reentrada pode ser configurada num `ActorService`do definições durante o registo. A definição aplica-se a todas as instâncias de ator criadas no serviço de ator.

O exemplo seguinte mostra um serviço de ator que define o modo de reentrância como `ActorReentrancyMode.Disallowed`. Neste caso, se um ator envia uma mensagem de reentrância para outro ator, uma exceção de tipo `FabricException` será gerada.

```csharp
static class Program
{
    static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<Actor1>(
                (context, actorType) => new ActorService(
                    context,
                    actorType, () => new Actor1(),
                    settings: new ActorServiceSettings()
                    {
                        ActorConcurrencySettings = new ActorConcurrencySettings()
                        {
                            ReentrancyMode = ActorReentrancyMode.Disallowed
                        }
                    }))
                .GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}
```
```Java
static class Program
{
    static void Main()
    {
        try
        {
            ActorConcurrencySettings actorConcurrencySettings = new ActorConcurrencySettings();
            actorConcurrencySettings.setReentrancyMode(ActorReentrancyMode.Disallowed);

            ActorServiceSettings actorServiceSettings = new ActorServiceSettings();
            actorServiceSettings.setActorConcurrencySettings(actorConcurrencySettings);

            ActorRuntime.registerActorAsync(
                Actor1.getClass(),
                (context, actorType) -> new FabricActorService(
                    context,
                    actorType, () -> new Actor1(),
                    null,
                    stateProvider,
                    actorServiceSettings, timeout);

            Thread.sleep(Long.MAX_VALUE);
        }
        catch (Exception e)
        {
            throw e;
        }
    }
}
```


## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre a reentrada no [documentação de referência da API de Ator](https://msdn.microsoft.com/library/azure/dn971626.aspx)
