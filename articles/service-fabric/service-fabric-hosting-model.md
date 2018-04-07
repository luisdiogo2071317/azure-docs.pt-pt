---
title: Modelo de alojamento de Service Fabric do Azure | Microsoft Docs
description: Descreve a relação entre réplicas (ou instâncias) de um serviço de recursos de infraestrutura de serviço implementado e o processo de anfitrião do serviço.
services: service-fabric
documentationcenter: .net
author: harahma
manager: timlt
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/15/2017
ms.author: harahma
ms.openlocfilehash: b2ba019f21256ee98276ef30847c43709b9b3462
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="azure-service-fabric-hosting-model"></a>Modelo de alojamento de Service Fabric do Azure
Este artigo fornece uma descrição geral da aplicação que aloja modelos fornecidos pelo Azure Service Fabric e descreve as diferenças entre o **processo partilhado** e **processo exclusivo** modelos. Descreve como uma aplicação implementada procura um nó de Service Fabric e a relação entre réplicas (ou instâncias) do serviço e o processo de anfitrião do serviço.

Antes de prosseguir, não se esqueça de que compreende os conceitos vários e relações explicado [modelar uma aplicação no Service Fabric][a1]. 

> [!NOTE]
> Neste artigo, a menos que explicitamente mencionadas como o que significa algo diferente:
>
> - *Réplica* refere-se para ambos os uma réplica de um serviço com estado e uma instância de um serviço sem estado.
> - *CodePackage* é tratado como equivalentes para um *ServiceHost* processo regista um *ServiceType*e as réplicas de anfitriões de serviços que *ServiceType*.
>

Para compreender o modelo de alojamento, vamos guiá-lo através de um exemplo. Vamos supor que temos uma *ApplicationType* 'MyAppType', que tem um *ServiceType* 'MyServiceType'. 'MyServiceType' fornecido pelo *ServicePackage* 'MyServicePackage', que tem um *CodePackage* 'MyCodePackage'. Regista 'MyCodePackage' *ServiceType* MyServiceType quando é executada.

Vamos supor que temos um cluster de três nós e, criamos um *aplicação* **fabric: / App1** do tipo 'MyAppType'. No interior desta aplicação **fabric: / App1**, vamos criar um serviço **recursos de infraestrutura: App1/ServiceA** do tipo 'MyServiceType'. Este serviço tem duas partições (por exemplo, **P1** e **P2**) e três réplicas por partição. O diagrama seguinte mostra a vista desta aplicação dado que acaba por ficar implementado num nó.


![Diagrama de vista de nó da aplicação implementada][node-view-one]


Service Fabric ativado 'MyServicePackage', que foi iniciado 'MyCodePackage', que está a alojar réplicas ambas as partições de. Todos os nós do cluster têm a mesma vista porque escolhemos o número de réplicas por partição seja igual ao número de nós no cluster. Vamos criar outro serviço, **fabric: / App1/ServiceB**, na aplicação **fabric: / App1**. Este serviço tem uma partição (por exemplo, **P3**) e três réplicas por partição. O diagrama seguinte mostra a vista de novo no nó:


![Diagrama de vista de nó da aplicação implementada][node-view-two]


Service Fabric colocado o novo réplica para a partição **P3** do serviço **fabric: / App1/ServiceB** na ativação existente de 'MyServicePackage'. Agora. Vamos criar outra aplicação **fabric: / App2** do tipo 'MyAppType'. Dentro de **fabric: / App2**, criar um serviço **recursos de infraestrutura: App2/ServiceA**. Este serviço tem duas partições (**P4** e **P5**) e três réplicas por partição. O diagrama seguinte mostra a nova vista de nó:


![Diagrama de vista de nó da aplicação implementada][node-view-three]


Service Fabric ativa uma nova cópia 'MyServicePackage', que inicia uma nova cópia 'MyCodePackage'. As réplicas de ambas as partições do serviço **fabric: / App2/ServiceA** (**P4** e **P5**) são colocados nesta nova cópia 'MyCodePackage'.

## <a name="shared-process-model"></a>Partilhado do modelo de processo
A secção anterior descreve predefinida fornecido pelo Service Fabric, referido como o modelo de processo partilhado do modelo de alojamento. Neste modelo, para uma aplicação específica, apenas uma cópia de um determinado *ServicePackage* está ativado num nó (que inicia a todos os o *CodePackages* nela contidas). Todas as réplicas de todos os serviços de um determinado *ServiceType* são colocados no *CodePackage* que regista que *ServiceType*. Por outras palavras, todas as réplicas de todos os serviços num nó de um determinado *ServiceType* partilhar o mesmo processo.

## <a name="exclusive-process-model"></a>Modelo de processo exclusivo
O modelo alojamento fornecido pelo Service Fabric é o modelo de processo exclusivo. Neste modelo, num determinado nó, cada réplica se encontra no seu próprio processo dedicado. Service Fabric ativa uma nova cópia *ServicePackage* (que inicia a todos os *CodePackages* nela contidas). As réplicas são colocadas no *CodePackage* que registado o *ServiceType* do serviço ao qual pertence a réplica. 

Se estiver a utilizar o Service Fabric versão 5.6 ou posterior, pode escolher o modelo de processo exclusivo no momento cria um serviço (utilizando [PowerShell][p1], [REST] [ r1], ou [FabricClient][c1]). Especifique **ServicePackageActivationMode** como 'ExclusiveProcess'.

```powershell
PS C:\>New-ServiceFabricService -ApplicationName "fabric:/App1" -ServiceName "fabric:/App1/ServiceA" -ServiceTypeName "MyServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1 -ServicePackageActivationMode "ExclusiveProcess"
```

```csharp
var serviceDescription = new StatelessServiceDescription
{
    ApplicationName = new Uri("fabric:/App1"),
    ServiceName = new Uri("fabric:/App1/ServiceA"),
    ServiceTypeName = "MyServiceType",
    PartitionSchemeDescription = new SingletonPartitionSchemeDescription(),
    InstanceCount = -1,
    ServicePackageActivationMode = ServicePackageActivationMode.ExclusiveProcess
};

var fabricClient = new FabricClient(clusterEndpoints);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

Se tiver um serviço predefinido no seu manifesto de aplicação, pode escolher o modelo de processo exclusivo, especificando o **ServicePackageActivationMode** atributo:

```xml
<DefaultServices>
  <Service Name="MyService" ServicePackageActivationMode="ExclusiveProcess">
    <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
      <SingletonPartition/>
    </StatelessService>
  </Service>
</DefaultServices>
```
Agora vamos criar outro serviço, **fabric: / App1/ServiceC**, na aplicação **fabric: / App1**. Este serviço tem duas partições (por exemplo, **P6** e **P7**) e três réplicas por partição. Definir **ServicePackageActivationMode** para 'ExclusiveProcess'. O diagrama seguinte mostra a nova vista no nó:


![Diagrama de vista de nó da aplicação implementada][node-view-four]


Como pode ver, o Service Fabric ativado duas novas cópias dos 'MyServicePackage' (um para cada réplica da partição **P6** e **P7**). Service Fabric colocado cada réplica na respetiva cópia dedicada de *CodePackage*. Quando utiliza o modelo de processo exclusivas para uma aplicação específica, vários copia de um determinado *ServicePackage* podem estar ativas num nó. No exemplo anterior, três cópias dos 'MyServicePackage' estão ativas para **fabric: / App1**. Cada uma destas cópias Active Directory de 'MyServicePackage' tem um **ServicePackageActivationId** associados à mesma. Este ID identifica essa cópia na aplicação **fabric: / App1**.

Quando utilizar o modelo de processo partilhado para uma aplicação, há apenas uma cópia ativa de *ServicePackage* num nó. O **ServicePackageActivationId** para esta ativação de *ServicePackage* é uma cadeia vazia. É este o caso, por exemplo, com **fabric: / App2**.

> [!NOTE]
>- O processo partilhado corresponde ao modelo de alojamento **ServicePackageActivationMode** é igual a **SharedProcess**. Esta é a predefinição de modelo de alojamento, e **ServicePackageActivationMode** não tem de ser especificado no momento da criação do serviço.
>
>- O processo exclusivo corresponde ao modelo de alojamento **ServicePackageActivationMode** é igual a **ExclusiveProcess**. Para utilizar esta definição, deve especificar explicitamente no momento da criação do serviço. 
>
>- Para ver o modelo de alojamento de um serviço, consultar o [descrição do serviço][p2]e observe o valor de **ServicePackageActivationMode**.
>
>

## <a name="work-with-a-deployed-service-package"></a>Trabalhar com um pacote de serviço implementado
Uma cópia ativa um *ServicePackage* num nó é referido como um [implementado o pacote de serviço][p3]. Quando utiliza o modelo de processo exclusivo para a criação de serviços, para uma aplicação específica, podem existir vários pacotes de serviço implementado para os mesmos *ServicePackage*. Se estiver a efetuar operações específicas para um pacote de serviço implementado, deve fornecer **ServicePackageActivationId** para identificar um pacote específico de serviço implementado. Por exemplo, forneça o ID, se estiver [relatórios de estado de funcionamento de um pacote de serviço implementado] [ p4] ou [reiniciar o pacote do código de um pacote de serviço implementado] [p5].

Pode encontrar o **ServicePackageActivationId** de um pacote de serviço implementado através da consulta a lista de [implementar pacotes service] [ p3] num nó. Quando estiver a consultar para o [implementado tipos de serviço][p6], [implementado réplicas][p7], e [implementado os pacotes de código ] [ p8] num nó, o resultado da consulta contém também o **ServicePackageActivationId** do pacote de serviço principal implementada.

> [!NOTE]
>- No modelo de alojamento de processo partilhado, num determinado nó, para uma aplicação específica, apenas uma cópia de um *ServicePackage* está ativado. Tem um **ServicePackageActivationId** igual a *uma cadeia vazia*e não precisa de ser especificado ao executar operações relacionadas com o pacote de serviço implementado. 
>
> - Sob o alojamento de processo exclusivo de modelo, num determinado nó, para uma aplicação específica, cópias de uma ou mais de um *ServicePackage* podem estar ativas. Cada ativação tem um *vazios* **ServicePackageActivationId**, especificado ao executar operações relacionadas com o pacote de serviço implementado. 
>
> - Se **ServicePackageActivationId** for omitida, será assumida a *uma cadeia vazia*. Se um pacote de serviço implementado foi ativado no modelo de processo partilhado estiver presente, será possível efetuar a operação no mesmo. Caso contrário, a operação falha.
>
> - Não consultar uma vez e cache a **ServicePackageActivationId**. O ID é gerado dinamicamente e pode ser alteradas para várias razões. Antes de efetuar uma operação que precisa de **ServicePackageActivationId**, primeiro, deve consultar a lista de [implementar pacotes service] [ p3] num nó. Em seguida, utilize o **ServicePackageActivationId** do resultado da consulta para efetuar a operação original.
>
>

## <a name="guest-executable-and-container-applications"></a>Aplicações de executável e contentor de convidado
Trata do Service Fabric [executável convidado] [ a2] e [contentor] [ a3] aplicações como serviços sem monitorização de estado, que são autónomo. Não há nenhum tempo de execução do Service Fabric no *ServiceHost* (um processo ou contentor). Uma vez que estes serviços são autónomo, o número de réplicas por *ServiceHost* não se aplica a estes serviços. A configuração mais comuns utilizada com estes serviços é partição única, com [InstanceCount] [ c2] igual a -1 (uma cópia o código do serviço em execução em cada nó do cluster). 

A predefinição **ServicePackageActivationMode** para estes serviços é **SharedProcess**, caso em que o Service Fabric ativa apenas uma cópia *ServicePackage* num nó para uma determinada aplicação.  Isto significa que apenas uma cópia do código do serviço será executado um nó. Se pretender várias cópias do seu código de serviço para ser executada num nó, especifique **ServicePackageActivationMode** como **ExclusiveProcess** no momento da criação do serviço. Por exemplo, pode fazer isto ao criar vários serviços (*Service1* para *ServiceN*) de *ServiceType* (especificado na *ServiceManifest*), ou quando o serviço não está particionado em várias. 

## <a name="change-the-hosting-model-of-an-existing-service"></a>Alterar o modelo de alojamento de um serviço existente
À hora presente, não é possível alterar o modelo de alojamento de um serviço do processo partilhado para o processo exclusivos (ou vice-versa).

## <a name="choose-between-the-hosting-models"></a>Escolha entre os modelos de alojamento
Deve avaliar o modelo de alojamento melhor se adeque às necessidades. O modelo de processo partilhado utiliza recursos do sistema operativo melhor, porque os processos menos são gerados e várias réplicas no mesmo processo podem partilhar portas. No entanto, se uma das réplicas tem um erro em que tem de colocar para baixo o anfitrião do serviço, afeta todos os outras réplicas no mesmo processo.

 O modelo de processo exclusivo fornece isolamento melhor, com cada réplica num processo próprio. Se uma das réplicas tem um erro, não afetar outras réplicas. Este modelo é útil nos casos em que a partilha de porta não é suportado pelo protocolo de comunicação. -Facilita a capacidade de aplicar a governação de recursos ao nível de réplica. No entanto, o processo de exclusivo consome mais recursos de sistema operativo, como se cria um processo para cada réplica no nó.

## <a name="exclusive-process-model-and-application-model-considerations"></a>Considerações de modelo de modelo de processo exclusivo e a aplicação
Para a maioria das aplicações, pode modelar a sua aplicação no Service Fabric mantendo um *ServiceType* por *ServicePackage*. 

Em determinados casos, de Service Fabric também permite que mais do que um *ServiceType* por *ServicePackage* (e um *CodePackage* pode registar mais do que um  *ServiceType*). Seguem-se alguns dos cenários em que estas configurações podem ser úteis:

- Se pretender otimizar a utilização de recursos por gerar menos processos e ter superior densidade de réplica por processo.
- As réplicas a partir de diferentes *ServiceTypes* necessário partilhar alguns dados comuns que tenha uma memória ou elevada inicialização custo.
- Tiver um oferta de serviço gratuito e que pretende colocar em pausa um limite de utilização de recursos colocando todas as réplicas do serviço no mesmo processo.

O processo exclusivo que aloja o modelo não é coherent com um modelo de aplicação ter vários *ServiceTypes* por *ServicePackage*. Isto acontece porque vários *ServiceTypes* por *ServicePackage* foram concebidos para alcançar superior recursos entre as réplicas de partilha e permite a densidade de réplica superior por processo. O modelo de processo exclusivo foi concebido para alcançar os resultados diferentes.

Considere o caso de vários *ServiceTypes* por *ServicePackage*, com outra *CodePackage* registar cada *ServiceType*. Vamos supor que temos uma *ServicePackage* 'MultiTypeServicePackge', que tem dois *CodePackages*:

- 'MyCodePackageA', que regista *ServiceType* 'MyServiceTypeA'.
- 'MyCodePackageB', que regista *ServiceType* 'MyServiceTypeB'.

Agora, vamos supor que criamos uma aplicação, **fabric: / SpecialApp**. Dentro de **fabric: / SpecialApp**, iremos criar dois serviços com o modelo de processo exclusivo os seguintes:

- Serviço **fabric: / SpecialApp/ServiceA** do tipo 'MyServiceTypeA', com duas partições (por exemplo, **P1** e **P2**) e três réplicas por partição.
- Serviço **fabric: / SpecialApp/ServiceB** do tipo 'MyServiceTypeB', com duas partições (**P3** e **P4**) e três réplicas por partição.

Num determinado nó, ambos os serviços têm duas réplicas. Porque o modelo de processo exclusivo é utilizado para criar os serviços, o Service Fabric ativa uma nova cópia 'MyServicePackage' para cada réplica. Cada ativação de 'MultiTypeServicePackge' inicia uma cópia de 'MyCodePackageA' e 'MyCodePackageB'. No entanto, apenas um dos 'MyCodePackageA' ou 'MyCodePackageB' aloja a réplica para o qual 'MultiTypeServicePackge' foi ativado. O diagrama seguinte mostra a vista de nó:


![Diagrama de vista de nó da aplicação implementada][node-view-five]


Na ativação de 'MultiTypeServicePackge' para a réplica da partição **P1** do serviço **fabric: / SpecialApp/ServiceA**, 'MyCodePackageA' está a alojar a réplica. 'MyCodePackageB' está em execução. Da mesma forma, na ativação de 'MultiTypeServicePackge' para a réplica da partição **P3** do serviço **fabric: / SpecialApp/ServiceB**, 'MyCodePackageB' está a alojar a réplica. 'MyCodePackageA' está em execução. Por conseguinte, o maior número de *CodePackages* (registar diferentes *ServiceTypes*) por *ServicePackage*, quanto maior for a redundante utilização de recursos. 
 
 No entanto, se criamos os serviços **fabric: / SpecialApp/ServiceA** e **recursos de infraestrutura: SpecialApp/ServiceB** com o modelo de processo partilhado, o Service Fabric ativa apenas uma cópia de ' MultiTypeServicePackge' para a aplicação **fabric: / SpecialApp**. 'MyCodePackageA' aloja todas as réplicas para o serviço **fabric: / SpecialApp/ServiceA**. 'MyCodePackageB' aloja todas as réplicas para o serviço **fabric: / SpecialApp/ServiceB**. O diagrama seguinte mostra a vista de nó nesta definição: 


![Diagrama de vista de nó da aplicação implementada][node-view-six]


No exemplo anterior, se pensa que se 'MyCodePackageA' regista 'MyServiceTypeA' e 'MyServiceTypeB' e não há nenhum MyCodePackageB, em seguida, há não redundante *CodePackage* em execução. Apesar de Isto está correto, este modelo de aplicação não estão alinhados com o processo exclusivo modelo de alojamento. Se o objetivo é colocar cada réplica num processo próprio dedicado, não terá de registar ambos *ServiceTypes* do mesmo *CodePackage*. Em vez disso, basta coloca cada *ServiceType* no seu próprio *ServicePackage*.

## <a name="next-steps"></a>Passos Seguintes
[Uma aplicação do pacote] [ a4] e prepará-lo a implementar.

[Implementar e remover aplicações][a5]. Este artigo descreve como utilizar o PowerShell para gerir instâncias da aplicação.

<!--Image references-->
[node-view-one]: ./media/service-fabric-hosting-model/node-view-one.png
[node-view-two]: ./media/service-fabric-hosting-model/node-view-two.png
[node-view-three]: ./media/service-fabric-hosting-model/node-view-three.png
[node-view-four]: ./media/service-fabric-hosting-model/node-view-four.png
[node-view-five]: ./media/service-fabric-hosting-model/node-view-five.png
[node-view-six]: ./media/service-fabric-hosting-model/node-view-six.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[a1]: service-fabric-application-model.md
[a2]: service-fabric-guest-executables-introduction.md
[a3]: service-fabric-containers-overview.md
[a4]: service-fabric-package-apps.md
[a5]: service-fabric-deploy-remove-applications.md

[r1]: https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-createservice

[c1]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync
[c2]: https://docs.microsoft.com/dotnet/api/system.fabric.description.statelessservicedescription.instancecount

[p1]: https://docs.microsoft.com/powershell/servicefabric/vlatest/new-servicefabricservice
[p2]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricservicedescription
[p3]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedservicePackage
[p4]: https://docs.microsoft.com/powershell/servicefabric/vlatest/send-servicefabricdeployedservicepackagehealthreport
[p5]: https://docs.microsoft.com/powershell/servicefabric/vlatest/restart-servicefabricdeployedcodepackage
[p6]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedservicetype
[p7]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedreplica
[p8]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedcodepackage
