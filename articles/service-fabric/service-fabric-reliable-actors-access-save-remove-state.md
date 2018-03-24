---
title: Gerir o estado do Service Fabric do Azure | Microsoft Docs
description: Saiba como aceder, guardar e remover o estado do serviço Fabric Reliable Actors.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: ''
ms.assetid: 37cf466a-5293-44c0-a4e0-037e5d292214
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/19/2018
ms.author: vturecek
ms.openlocfilehash: c00805fc5d8515fb743256e35a75be1546483245
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="access-save-and-remove-reliable-actors-state"></a>Aceder, guardar e remover o estado de Reliable Actors
[Reliable Actors](service-fabric-reliable-actors-introduction.md) single-threaded objetos que pode encapsular a lógica e o estado e manter o estado de forma fiável. Todas as instâncias de ator tem o seu próprio [Gestor de estado](service-fabric-reliable-actors-state-management.md): pares chave/valor uma estrutura semelhante de dicionário de dados que armazena de forma fiável. O Gestor de estado é um wrapper em torno de um fornecedor de estado. Pode utilizá-lo para armazenar dados, independentemente da que [definição persistência](service-fabric-reliable-actors-state-management.md#state-persistence-and-replication) é utilizado.

Chaves de Gestor de estado têm de ser cadeias. Os valores são genéricos e pode ser qualquer tipo, incluindo tipos personalizados. Valores armazenados no Gestor de estado tem de ser serializável de contrato de dados porque que podem ser transmitidas através da rede para outros nós durante a replicação e podem ser gravados no disco, consoante a definição de persistência de estado de um ator.

O Gestor de estado expõe métodos de dicionário comuns para gerir o estado, semelhante às foi encontrado no dicionário fiável.

Para informações, consulte [melhores práticas na gestão de estado de ator](service-fabric-reliable-actors-state-management.md#best-practices).

## <a name="access-state"></a>Estado de acesso
Estado é acedido através do Gestor de estado por chave. Métodos de Gestor de estado são todos os assíncronos porque poderá necessitam de e/s de disco quando atores tem persistente estado. Após a primeira acesso, os objetos de estado são colocadas em cache na memória. Repita a objetos de acesso de operações de acesso diretamente a partir da memória e devolver de forma síncrona sem incorrer em e/s de disco ou assíncrona contexto-mudar sobrecarga. Um objeto de estado é removido da cache de nos seguintes casos:

* Um método de ator emite uma exceção não processada depois obtém um objecto do Gestor de estado.
* Um ator for reativado, após a ser desativada ou após a falha.
* O fornecedor de estado de páginas de estado para o disco. Este comportamento depende a implementação do fornecedor de estado. O fornecedor de estado predefinido para o `Persisted` definição tem este comportamento.

Pode obter o estado utilizando um padrão *obter* operação que emite `KeyNotFoundException`(c#) ou `NoSuchElementException`(Java) se não existir uma entrada para a chave:

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<int> GetCountAsync()
    {
        return this.StateManager.GetStateAsync<int>("MyState");
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture<Integer> getCountAsync()
    {
        return this.stateManager().getStateAsync("MyState");
    }
}
```

Também pode obter o estado utilizando um *TryGet* método que inicia se não existir uma entrada para uma chave:

```csharp
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task<int> GetCountAsync()
    {
        ConditionalValue<int> result = await this.StateManager.TryGetStateAsync<int>("MyState");
        if (result.HasValue)
        {
            return result.Value;
        }

        return 0;
    }
}
```
```Java
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture<Integer> getCountAsync()
    {
        return this.stateManager().<Integer>tryGetStateAsync("MyState").thenApply(result -> {
            if (result.hasValue()) {
                return result.getValue();
            } else {
                return 0;
            });
    }
}
```

## <a name="save-state"></a>Guardar o Estado
Os métodos de obtenção do Gestor de estado devolvem uma referência a um objeto na memória local. Este objeto no memória local relativos a modificar não irá causar que sejam guardados de forma durável. Quando um objeto é obtido a partir do Gestor de Estados e modificado, tem de ser voltar para o Gestor de estado sejam guardados de forma durável.

Pode inserir o estado utilizando um incondicional *definir*, que é o equivalente a `dictionary["key"] = value` sintaxe:

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task SetCountAsync(int value)
    {
        return this.StateManager.SetStateAsync<int>("MyState", value);
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture setCountAsync(int value)
    {
        return this.stateManager().setStateAsync("MyState", value);
    }
}
```

Pode adicionar o estado utilizando um *adicionar* método. Este método emite `InvalidOperationException`(c#) ou `IllegalStateException`(Java) ao tentar adicionar uma chave que já existe.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task AddCountAsync(int value)
    {
        return this.StateManager.AddStateAsync<int>("MyState", value);
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture addCountAsync(int value)
    {
        return this.stateManager().addOrUpdateStateAsync("MyState", value, (key, old_value) -> old_value + value);
    }
}
```

Também pode adicionar o estado utilizando um *TryAdd* método. Este método não inicia quando tentar adicionar uma chave que já existe.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task AddCountAsync(int value)
    {
        bool result = await this.StateManager.TryAddStateAsync<int>("MyState", value);

        if (result)
        {
            // Added successfully!
        }
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture addCountAsync(int value)
    {
        return this.stateManager().tryAddStateAsync("MyState", value).thenApply((result)->{
            if(result)
            {
                // Added successfully!
            }
        });
    }
}
```

No final de um método de ator, o Gestor de estado guarda automaticamente quaisquer valores que foram adicionados ou modificados por uma operação de inserção ou atualização. "Guardar" pode incluir a persistência para o disco e a replicação, consoante as definições utilizadas. Os valores que não foram modificados não são persistentes ou replicados. Se não existem valores foram modificados, a guardar operação faz nada. Se guardar falhar, o estado modificado é ignorado e o estado original é recarregar.

Pode também guardar estado manualmente ao chamar o `SaveStateAsync` método na base de ator:

```csharp
async Task IMyActor.SetCountAsync(int count)
{
    await this.StateManager.AddOrUpdateStateAsync("count", count, (key, value) => count > value ? count : value);

    await this.SaveStateAsync();
}
```
```Java
interface MyActor {
    CompletableFuture setCountAsync(int count)
    {
        this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value).thenApply();

        this.stateManager().saveStateAsync().thenApply();
    }
}
```

## <a name="remove-state"></a>Remover o Estado
Pode remover estado permanentemente a partir do Gestor de estado de um ator ao chamar o *remover* método. Este método emite `KeyNotFoundException`(c#) ou `NoSuchElementException`(Java) ao tentar remover uma chave que não existe.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task RemoveCountAsync()
    {
        return this.StateManager.RemoveStateAsync("MyState");
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture removeCountAsync()
    {
        return this.stateManager().removeStateAsync("MyState");
    }
}
```

Pode também remover estado permanentemente utilizando o *TryRemove* método. Este método não throw ao tentar remover uma chave que não existe.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task RemoveCountAsync()
    {
        bool result = await this.StateManager.TryRemoveStateAsync("MyState");

        if (result)
        {
            // State removed!
        }
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture removeCountAsync()
    {
        return this.stateManager().tryRemoveStateAsync("MyState").thenApply((result)->{
            if(result)
            {
                // State removed!
            }
        });
    }
}
```

## <a name="next-steps"></a>Passos Seguintes

Estado que é armazenado em Reliable Actors deve ser serializado antes do respetivo escrito no disco e replicados para elevada disponibilidade. Saiba mais sobre [serialização do tipo de Ator](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).

Em seguida, saiba mais sobre [monitorização de desempenho e diagnóstico de Ator](service-fabric-reliable-actors-diagnostics.md).
