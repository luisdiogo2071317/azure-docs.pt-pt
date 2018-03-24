---
title: Implementar funcionalidades no atores do Service Fabric do Azure | Microsoft Docs
description: Descreve como escrever o seu próprio serviço de atores que implementa a funcionalidades de nível de serviço da mesma forma que faria ao herdar de StatefulService.
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
ms.date: 03/19/2018
ms.author: vturecek
ms.openlocfilehash: 60989825ecdefa853d0e2df99619e3cb350cb6bc
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="implementing-service-level-features-in-your-actor-service"></a>Implementar funcionalidades de nível de serviço no seu serviço de atores
Conforme descrito em [dispor de serviço](service-fabric-reliable-actors-platform.md#service-layering), o próprio serviço de atores é um serviço fiável.  Pode escrever o seu próprio serviço que deriva de `ActorService` e implementar funcionalidades de nível de serviço da mesma forma que faria ao herdar StatefulService, tais como:

- Cópia de segurança do serviço e de restauro.
- Partilhado funcionalidade para todos os atores, por exemplo, um disjuntor automático.
- Chamadas de procedimento remoto do serviço de atores em si e em cada ator individuais.

## <a name="using-the-actor-service"></a>Utilizar o serviço de atores
Instâncias de ator ter acesso ao serviço de atores em que estejam a ser executados. Através do serviço de atores, instâncias de ator programaticamente podem obter o contexto de serviço. O contexto de serviço tem o ID de partição, o nome do serviço, o nome da aplicação e outras informações específicas da plataforma do Service Fabric:

```csharp
Task MyActorMethod()
{
    Guid partitionId = this.ActorService.Context.PartitionId;
    string serviceTypeName = this.ActorService.Context.ServiceTypeName;
    Uri serviceInstanceName = this.ActorService.Context.ServiceName;
    string applicationInstanceName = this.ActorService.Context.CodePackageActivationContext.ApplicationName;
}
```
```Java
CompletableFuture<?> MyActorMethod()
{
    UUID partitionId = this.getActorService().getServiceContext().getPartitionId();
    String serviceTypeName = this.getActorService().getServiceContext().getServiceTypeName();
    URI serviceInstanceName = this.getActorService().getServiceContext().getServiceName();
    String applicationInstanceName = this.getActorService().getServiceContext().getCodePackageActivationContext().getApplicationName();
}
```

Como Reliable Services, o serviço de atores tem de ser registado com um tipo de serviço no tempo de execução do Service Fabric. Para o serviço de atores para executar as instâncias de ator, o tipo de ator também tem de ser registado com o serviço de atores. O método de registo `ActorRuntime` realiza este trabalho para atores. No caso mais simples, apenas pode registar o tipo de ator e implicitamente será utilizado o serviço de atores com as predefinições:

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>().GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```

Em alternativa, pode utilizar um lambda fornecido pelo método de registo para construir o serviço de atores por si. Pode, em seguida, configure o serviço de atores e explicitamente construir as instâncias de ator, onde pode inserir dependências para o seu ator através do respetivo construtor:

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>(
            (context, actorType) => new ActorService(context, actorType, () => new MyActor()))
            .GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```
```Java
static class Program
{
    private static void Main()
    {
      ActorRuntime.registerActorAsync(
              MyActor.class,
              (context, actorTypeInfo) -> new FabricActorService(context, actorTypeInfo),
              timeout);

        Thread.sleep(Long.MAX_VALUE);
    }
}
```

## <a name="actor-service-methods"></a>Métodos de serviço de atores
A implementa do serviço de Atores `IActorService` (c#) ou `ActorService` (Java), que por sua vez implementa `IService` (c#) ou `Service` (Java). Esta é a interface utilizada pelo Reliable Services sistema de interação remota, que permite chamadas de procedimento remoto em métodos do serviço. Contém os métodos de nível de serviço que podem ser chamados remotamente através de comunicação remota do serviço e permite-lhe [enumerar](service-fabric-reliable-actors-enumerate.md) e [eliminar](service-fabric-reliable-actors-delete-actors.md) atores.


## <a name="custom-actor-service"></a>Serviço de atores personalizado
Ao utilizar o lambda de registo de ator, pode registar o seus próprios serviço de atores personalizado que deriva de `ActorService` (c#) e `FabricActorService` (Java). Neste serviço de atores personalizado, pode implementar a sua própria funcionalidade de nível de serviço ao escrever uma classe de serviço que herda `ActorService` (c#) ou `FabricActorService` (Java). Um serviço de atores personalizado herda todas as funcionalidades de tempo de execução de ator do `ActorService` (c#) ou `FabricActorService` (Java) e pode ser utilizado para implementar os suas próprias métodos do serviço.

```csharp
class MyActorService : ActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<ActorBase> newActor)
        : base(context, typeInfo, newActor)
    { }
}
```
```Java
class MyActorService extends FabricActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, BiFunction<FabricActorService, ActorId, ActorBase> newActor)
    {
         super(context, typeInfo, newActor);
    }
}
```

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>(
            (context, actorType) => new MyActorService(context, actorType, () => new MyActor()))
            .GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```
```Java
public class Program
{
    public static void main(String[] args)
    {
        ActorRuntime.registerActorAsync(
                MyActor.class,
                (context, actorTypeInfo) -> new FabricActorService(context, actorTypeInfo),
                timeout);
        Thread.sleep(Long.MAX_VALUE);
    }
}
```

## <a name="implementing-actor-backup-and-restore"></a>Implementar a cópia de segurança de ator e restauro
Um serviço de atores personalizado pode expor um método para cópias de segurança ator, tirando partido do serviço de escuta de sistema de interação remota já está presente no `ActorService`.  Por exemplo, consulte [atores de cópia de segurança e restauro](service-fabric-reliable-actors-backup-and-restore.md).

## <a name="actor-using-remoting-v2-stack"></a>Ator utilizando a pilha do sistema de interação remota V2
Com o pacote nuget 2.8, os utilizadores podem agora utilizar a pilha de comunicação remota V2, que é mais performant e fornece as funcionalidades, como serialização personalizada. V2 de sistema de interação remota não é retrocompatível com pilha de comunicação remota existente (estamos a ligar agora-lo como a pilha do sistema de interação remota V1).

As alterações seguintes são necessários para utilizar a pilha do sistema de interação remota V2.
 1. Adicione o seguinte atributo de assemblagem em Interfaces de Atores.
   ```csharp
   [assembly:FabricTransportActorRemotingProvider(RemotingListener = RemotingListener.V2Listener,RemotingClient = RemotingClient.V2Client)]
   ```

 2. Compilação e atualizar ActorService Ator cliente e projetos para começar a utilizar a pilha de V2.

### <a name="actor-service-upgrade-to-remoting-v2-stack-without-impacting-service-availability"></a>Ator serviço atualizar para o sistema de interação remota V2 pilha sem afetar a disponibilidade do serviço.
Esta alteração será uma atualização do passo 2. Siga os passos na mesma sequência conforme indicado.

1.  Adicione o seguinte atributo de assemblagem em Interfaces de Atores. Este atributo irá iniciar os serviços de dois escuta para ActorService, V1 (existente) e o serviço de escuta V2. Atualize ActorService com esta alteração.

  ```csharp
  [assembly:FabricTransportActorRemotingProvider(RemotingListener = RemotingListener.CompatListener,RemotingClient = RemotingClient.V2Client)]
  ```

2. Atualize ActorClients depois de concluir a atualização acima.
Este passo certifica-se de que proxy de Ator está a utilizar o sistema de interação remota V2 pilha.

3. Este passo é opcional. Altere o atributo acima para remover o serviço de escuta V1.

    ```csharp
    [assembly:FabricTransportActorRemotingProvider(RemotingListener = RemotingListener.V2Listener,RemotingClient = RemotingClient.V2Client)]
    ```

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
