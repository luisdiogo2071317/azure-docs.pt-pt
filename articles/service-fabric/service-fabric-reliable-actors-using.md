---
title: Implementar recursos no actors do Azure Service Fabric | Documentos da Microsoft
description: Descreve como escrever seu próprio serviço de ator que implementa os recursos de nível de serviço da mesma forma que faria ao herdar StatefulService.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: amanbha
ms.assetid: 45839a7f-0536-46f1-ae2b-8ba3556407fb
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/19/2018
ms.author: vturecek
ms.openlocfilehash: 6aff9e9599d31942f994f3cb4e5e9219f33dc7e1
ms.sourcegitcommit: 30221e77dd199ffe0f2e86f6e762df5a32cdbe5f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/23/2018
ms.locfileid: "39205525"
---
# <a name="implementing-service-level-features-in-your-actor-service"></a>Implementação de recursos de nível de serviço no seu serviço de ator
Conforme descrito em [disposição em camadas de serviço](service-fabric-reliable-actors-platform.md#service-layering), o próprio serviço de ator é um serviço fiável.  Pode escrever seu próprio serviço, que deriva de `ActorService` e implementar recursos de nível de serviço da mesma forma que faria ao herdar StatefulService, tais como:

- Serviço backup e restauração.
- Partilhado funcionalidade para todos os atores, por exemplo, um disjuntor automático.
- Chamadas de procedimento remoto no próprio serviço de ator e em cada ator individual.

## <a name="using-the-actor-service"></a>Utilizar o serviço de ator
Instâncias de ator tem acesso para o serviço de ator em que estão a executar. Através do serviço de ator, instâncias de ator por meio de programação podem obter o contexto de serviço. O contexto de serviço tem o ID de partição, o nome do serviço, o nome da aplicação e outras informações específicas da plataforma do Service Fabric:

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

Como todos os serviços confiáveis, o serviço de ator tem de estar registado com um tipo de serviço no runtime do Service Fabric. Para o serviço de ator executar as instâncias de ator, o tipo de ator também tem de ser registado com o serviço de ator. O método de registo `ActorRuntime` realiza este trabalho para atores. No caso mais simples, apenas pode registar o tipo de ator e o serviço de ator com predefinições implicitamente será utilizado:

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

Em alternativa, pode utilizar um lambda fornecido pelo método de registo para construir o serviço de ator por conta própria. Pode, em seguida, configurar o serviço de ator e construir explicitamente as instâncias de ator, em que possa injetar dependências para o ator por meio de seu construtor:

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

## <a name="actor-service-methods"></a>Métodos de serviço de ator
A implementa do serviço de Ator `IActorService` (c#) ou `ActorService` (Java), que por sua vez implementa `IService` (c#) ou `Service` (Java). Esta é a interface usada por comunicação remota do Reliable Services, que permite chamadas de procedimento remoto em métodos de serviço. Ele contém métodos de nível de serviço que podem ser chamados remotamente por meio de comunicação remota do serviço e permitir que [enumerar](service-fabric-reliable-actors-enumerate.md) e [eliminar](service-fabric-reliable-actors-delete-actors.md) atores.


## <a name="custom-actor-service"></a>Serviço de ator personalizado
Ao utilizar o lambda de registo de ator, pode registrar seu próprio serviço de ator personalizado que deriva `ActorService` (c#) e `FabricActorService` (Java). Este serviço de ator personalizado, pode implementar sua própria funcionalidade de nível de serviço, escrevendo uma classe de serviço que herda `ActorService` (c#) ou `FabricActorService` (Java). Um serviço de ator personalizado herda todas as funcionalidades do runtime de ator partir `ActorService` (c#) ou `FabricActorService` (Java) e pode ser usado para implementar seus próprios métodos de serviço.

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

## <a name="implementing-actor-backup-and-restore"></a>Implementação de ator backup e restauração
Um serviço de ator personalizado pode expor um método para fazer cópias de segurança de dados de ator, aproveitando o serviço de escuta de comunicação remota já está presente no `ActorService`.  Por exemplo, veja [cópia de segurança e restaurar atores](service-fabric-reliable-actors-backup-and-restore.md).

## <a name="actor-using-remoting-v2interfacecompatible-stack"></a>Com a pilha de V2(InterfaceCompatible) de comunicação remota de ator
Gestão remota V2 (InterfaceCompatible também conhecido como V2_1) stack tem todas as funcionalidades de comunicação remota do V2 de pilha além é pilha compatível de interface para a V1 de comunicação remota de pilha, mas não é retrocompatível com V2 e V1. Para fazer a atualização da V1 V2_1 sem afetar a disponibilidade do serviço, siga os abaixo [artigo](#actor-service-upgrade-to-remoting-v2interfacecompatible-stack-without-impacting-service-availability).

As seguintes alterações são necessárias para usar a pilha de V2_1 de comunicação remota.
 1. Adicione o seguinte atributo de assembly em Interfaces de Ator.
   ```csharp
   [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2_1,RemotingClientVersion = RemotingClientVersion.V2_1)]
   ```

 2. Projetos de compilação e atualizar ActorService Ator cliente e para começar a utilizar a pilha da V2.

#### <a name="actor-service-upgrade-to-remoting-v2interfacecompatible-stack-without-impacting-service-availability"></a>Ator serviço atualizar para a pilha de V2(InterfaceCompatible) de comunicação remota sem afetar a disponibilidade do serviço.
Esta alteração será uma atualização do passo 2. Siga os passos na mesma sequência, conforme listado.

1.  Adicione o seguinte atributo de assembly em Interfaces de Ator. Este atributo iniciará dois serviços de escuta para ActorService, V1 (existente) e o serviço de escuta de V2_1. Atualize ActorService com esta alteração.

  ```csharp
  [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V1|RemotingListenerVersion.V2_1,RemotingClientVersion = RemotingClientVersion.V2_1)]
  ```

2. Atualize ActorClients depois de concluir a atualização acima.
Este passo torna-se de que o Proxy de Ator está a utilizar o Stack V2_1 de comunicação remota.

3. Este passo é opcional. Altere o atributo acima para remover o serviço de escuta do V1.

    ```csharp
    [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2_1,RemotingClientVersion = RemotingClientVersion.V2_1)]
    ```

## <a name="actor-using-remoting-v2-stack"></a>Utilizar a comunicação remota V2 pilha de ator
Com o pacote de nuget 2,8, os utilizadores podem agora utilizar a pilha de gestão remota V2, que apresenta um desempenho melhor e fornece funcionalidades como a serialização personalizada. Gestão remota V2 não é retrocompatível com pilha de comunicação remota existente (estamos a ligar agora-lo como pilha de comunicação remota do V1).

As seguintes alterações são necessárias para usar a pilha de V2 de comunicação remota.
 1. Adicione o seguinte atributo de assembly em Interfaces de Ator.
   ```csharp
   [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2,RemotingClientVersion = RemotingClientVersion.V2)]
   ```

 2. Projetos de compilação e atualizar ActorService Ator cliente e para começar a utilizar a pilha da V2.

#### <a name="actor-service-upgrade-to-remoting-v2-stack-without-impacting-service-availability"></a>Ator serviço atualizar para a pilha de V2 de comunicação remota sem afetar a disponibilidade do serviço.
Esta alteração será uma atualização do passo 2. Siga os passos na mesma sequência, conforme listado.

1.  Adicione o seguinte atributo de assembly em Interfaces de Ator. Este atributo iniciará dois serviços de escuta para ActorService, V1 (existente) e o serviço de escuta do V2. Atualize ActorService com esta alteração.

  ```csharp
  [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V1|RemotingListenerVersion.V2,RemotingClientVersion = RemotingClientVersion.V2)]
  ```

2. Atualize ActorClients depois de concluir a atualização acima.
Este passo torna-se de que o Proxy de Ator está a utilizar comunicação remota V2 pilha.

3. Este passo é opcional. Altere o atributo acima para remover o serviço de escuta do V1.

    ```csharp
    [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2,RemotingClientVersion = RemotingClientVersion.V2)]
    ```

## <a name="next-steps"></a>Passos Seguintes
* [Gerenciamento de estado do ator](service-fabric-reliable-actors-state-management.md)
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
