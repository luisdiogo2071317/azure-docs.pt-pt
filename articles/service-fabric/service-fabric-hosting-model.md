---
title: "Modelo de alojamento de infraestrutura de serviço do Azure | Microsoft Docs"
description: "Descreve a relação entre réplicas (ou instâncias) de um serviço de recursos de infraestrutura Servic implementado e o processo de anfitrião do serviço."
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
ms.openlocfilehash: 0206a9a486e3511834a23b3cc3f20f236a1cc261
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="service-fabric-hosting-model"></a>Modelo de alojamento de Service Fabric
Este artigo fornece uma descrição geral da aplicação que aloja modelos fornecidos pelo serviço de recursos de infraestrutura e descreve as diferenças entre o **processo partilhado** e **processo exclusivo** modelos. Descreve como uma aplicação implementada procura num nó de Service Fabric e a relação entre réplicas (ou instâncias) do serviço e o processo de anfitrião do serviço.

Antes de prosseguir, certifique-se de que está familiarizado com [modelo de aplicação do serviço de recursos de infraestrutura] [ a1] e compreender as várias conceitos e relação entre elas. 

> [!NOTE]
> Neste artigo, de simplicidade, a menos que explicitamente mencionado:
>
> - Todas as utilizações dos palavra *réplica* refere-se para ambos os uma réplica de um serviço com monitorização de estado ou de uma instância de um serviço sem estado.
> - *CodePackage* é tratada equivalente para *ServiceHost* processo regista um *ServiceType* e as réplicas de anfitriões de serviços que *ServiceType*.
>

Para compreender o modelo de alojamento, informe-nos guiá-lo através de um exemplo. Diga-nos temos um *ApplicationType* 'MyAppType', que tem um *ServiceType* 'MyServiceType'.  'MyServiceType' fornecido pelo *ServicePackage* 'MyServicePackage', que tem um *CodePackage* 'MyCodePackage'. Regista 'MyCodePackage' *ServiceType* MyServiceType quando é executada.

Vamos supor que temos um cluster de três nós e criamos uma *aplicação* **fabric: / App1** do tipo 'MyAppType'. Dentro de isto *aplicação* **recursos de infraestrutura: / App1** vamos criar um serviço **recursos de infraestrutura: App1/ServiceA** do tipo 'MyServiceType' que tem duas partições (diga **P1**   &  **P2**) e de três réplicas por partição. O diagrama seguinte mostra a vista desta aplicação dado que acaba por ficar implementado num nó.

<center>
![Vista de nó da aplicação implementada][node-view-one]
</center>

Service Fabric ativado 'MyServicePackage' que foi iniciado 'MyCodePackage' que está a alojar réplicas ambas as partições de.  Por exemplo, **P1** & **P2**. Todos os nós do cluster tem a mesma vista uma vez que escolhemos o número de réplicas por partição seja igual ao número de nós no cluster. Vamos criar outro serviço **recursos de infraestrutura: App1/ServiceB** na aplicação **fabric: / App1**, que tem uma partição (diga **P3**) e de três réplicas por partição. O diagrama seguinte mostra a vista de novo no nó:

<center>
![Vista de nó da aplicação implementada][node-view-two]
</center>

Como é possível ver o Service Fabric colocado o novo réplica para a partição **P3** do serviço **fabric: / App1/ServiceB** na ativação existente de 'MyServicePackage'. Agora permite criar outro *aplicação* **fabric: / App2** do tipo 'MyAppType'. Dentro de **fabric: / App2**, criar um serviço **recursos de infraestrutura: App2/ServiceA** que tem duas partições (diga **P4** & **P5**) e três réplicas por partição. Os diagramas seguintes mostra a nova vista de nó:

<center>
![Vista de nó da aplicação implementada][node-view-three]
</center>

Service Fabric ativa uma nova cópia 'MyServicePackage', que inicia uma nova cópia 'MyCodePackage'. As réplicas de ambas as partições do serviço **fabric: / App2/ServiceA** (por exemplo, **P4** & **P5**) são colocados nesta nova cópia 'MyCodePackage'.

## <a name="shared-process-model"></a>Partilhado do modelo de processo
O que vimos acima a predefinição é o modelo fornecido pelo serviço de recursos de infraestrutura de alojamento e é referido como **processo partilhado** modelo. Neste modelo, para um determinado *aplicação*, apenas um copiar de um indicado *ServicePackage* está ativado num *nó* (que inicia a todos os o *CodePackages* nele contidos) e todas as réplicas de todos os serviços de um indicado *ServiceType* são colocados no *CodePackage* que regista que *ServiceType*. Por outras palavras, todas as réplicas de todos os serviços num nó de um determinado *ServiceType* partilhar o mesmo processo.

## <a name="exclusive-process-model"></a>Modelo de processo exclusivo
O modelo alojamento fornecido pelo Service Fabric é **processo exclusivo** modelo. Neste modelo, num determinado *nó*, para posicionar cada réplica, o Service Fabric ativa uma nova cópia do *ServicePackage* (que inicia a todos os o *CodePackages* nele contidos) e de réplica é colocada no *CodePackage* registados a *ServiceType* do serviço de ao qual réplica pertence. Por outras palavras, cada réplica se encontra no seu próprio processo dedicado. 

Este modelo é suportado a partir de versão 5.6 do Service Fabric. **Processo exclusivo** modelo pode ser selecionado no momento da criação do serviço (utilizando [PowerShell][p1], [REST][r1], ou [FabricClient][c1]) especificando **ServicePackageActivationMode** como 'ExclusiveProcess'.

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

Se tiver um serviço predefinido no seu manifesto de aplicação, pode escolher **processo exclusivo** modelo especificando **ServicePackageActivationMode** atributo conforme mostrado abaixo:

```xml
<DefaultServices>
  <Service Name="MyService" ServicePackageActivationMode="ExclusiveProcess">
    <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
      <SingletonPartition/>
    </StatelessService>
  </Service>
</DefaultServices>
```
Continuar com o exemplo anterior, permite criar outro serviço **recursos de infraestrutura: App1/ServiceC** na aplicação **fabric: / App1** que tem duas partições (diga **P6**  &  **P7**) e de três réplicas por partição com **ServicePackageActivationMode** definido como 'ExclusiveProcess'. Diagrama seguinte mostra a nova vista no nó:

<center>
![Vista de nó da aplicação implementada][node-view-four]
</center>

Como pode ver, o Service Fabric ativado duas novas cópias dos 'MyServicePackage' (um para cada réplica da partição **P6** & **P7**) e colocá-la cada réplica na respetiva cópia dedicada de *CodePackage*. Outra coisa salientar aqui, quando **processo exclusivo** modelo é utilizado, para um determinado *aplicação*, copia múltiplo de um indicado *ServicePackage* podem estar ativas num *nó*. No acima exemplo, Vemos que três cópias dos 'MyServicePackage' estão ativas para **fabric: / App1**. Cada uma destas cópias Active Directory de 'MyServicePackage' tem um **ServicePackageActivationId** associada com a qual identifica essa cópia no *aplicação* **recursos de infraestrutura: / App1**.

Quando apenas **processo partilhado** modelo é utilizado para um *aplicação*, como **fabric: / App2** acima exemplo, existe é apenas uma cópia ativa de *ServicePackage*  num *nó* e **ServicePackageActivationId** para esta ativação de *ServicePackage* é 'cadeia vazia'.

> [!NOTE]
>- **Partilhado processo** modelo de alojamento corresponde à **ServicePackageActivationMode** igual **SharedProcess**. Esta é a predefinição de modelo de alojamento e **ServicePackageActivationMode** não tem de ser especificado no momento da criação do serviço.
>
>- **Processo exclusivo** modelo de alojamento corresponde à **ServicePackageActivationMode** definido como **ExclusiveProcess** e tem de ser especificado explicitamente no momento da criação do serviço. 
>
>- Modelo de alojamento de um serviço pode ser conhecido consultando o [descrição do serviço] [ p2] e observar o valor de **ServicePackageActivationMode**.
>
>

## <a name="working-with-deployed-service-package"></a>Trabalhar com o pacote de serviço implementado
Uma cópia ativa um *ServicePackage* num nó é referida como [implementado o pacote de serviço][p3]. Conforme mencionado anteriormente, quando **processo exclusivo** modelo é utilizado para criar serviços, para um determinado *aplicação*, podem existir vários pacotes de serviço implementado para os mesmos  *ServicePackage*. Ao executar operações específicas para um pacote de serviço implementado (por exemplo, [relatórios de estado de funcionamento de um pacote de serviço implementado] [ p4] ou [reiniciar o pacote do código de um serviço implementado pacote][p5]), **ServicePackageActivationId** tem de ser fornecido identificar específico implementado o pacote de serviço.

**ServicePackageActivationId** de um serviço implementado o pacote pode ser obtido ao consultar a lista de [implementar pacotes service] [ p3] num nó. Ao consultar [implementado tipos de serviço][p6], [implementado réplicas][p7], e [implementar pacotes de código] [ p8] num nó, o resultado da consulta contém também o **ServicePackageActivationId** do pacote de serviço principal implementada.

> [!NOTE]
>- Em **processo partilhado** modelo de alojamento, num determinado *nó*, para um determinado *aplicação*, apenas um copiar de uma *ServicePackage* está ativado. Tem **ServicePackageActivationId** igual a *uma cadeia vazia* e não precisa de ser especificado ao executar operações relacionadas com o pacote de serviço implementado. 
>
> - Em **processo exclusivo** modelo de alojamento, num determinado *nó*, para um determinado *aplicação*, um ou mais cópias de um *ServicePackage* podem estar ativas. Cada ativação tem um *vazios* **ServicePackageActivationId**, especificado ao executar operações relacionadas com o pacote de serviço implementado. 
>
> - Se **ServicePackageActivationId** for omitido, por predefinição 'string vazio'. Se um serviço implementado o pacote que foi ativado em **processo partilhado** modelo está presente, em seguida, será possível efetuar a operação no mesmo, caso contrário, a operação falhará.
>
> - Não consultar uma vez e cache **ServicePackageActivationId**, porque é gerada dinamicamente e podem ser alteradas para várias razões. Antes de efetuar uma operação que precisa de **ServicePackageActivationId**, primeiro, deve consultar a lista de [implementar pacotes service] [ p3] num nó e, em seguida, utilize  **ServicePackageActivationId** do resultado da consulta para efetuar a operação original.
>
>

## <a name="guest-executable-and-container-applications"></a>Aplicações de executável e contentor de convidado
Trata do Service Fabric [executável convidado] [ a2] e [contentor] [ a3] aplicações como serviços sem monitorização de estado, que são autónomo: Não há nenhum tempo de execução do Service Fabric no *ServiceHost* (um processo ou contentor). Uma vez que estes serviços são autónomo, número de réplicas por *ServiceHost* não se aplica a estes serviços. A configuração mais comuns utilizada com estes serviços é partição única com [InstanceCount] [ c2] igual a -1 (ou seja, uma cópia o código do serviço em execução em cada nó do cluster). 

A predefinição **ServicePackageActivationMode** para estes serviços é **SharedProcess**, caso em que o Service Fabric ativa apenas uma cópia *ServicePackage* num  *Nó* para um determinado *aplicação*.  Isto significa que apenas uma cópia do código do serviço será executado um *nó*. Se pretender várias cópias do seu código de serviço com um *nó* quando criar vários serviços (*Service1* para *ServiceN*) de *ServiceType* (especificado na *ServiceManifest*) ou quando o serviço está particionado em várias, deve especificar **ServicePackageActivationMode** como **ExclusiveProcess** no momento da criação do serviço.

## <a name="changing-hosting-model-of-an-existing-service"></a>Alterar modelo de alojamento de um serviço existente
Alterar modelo de alojamento de um serviço de **processo partilhado** para **processo exclusivo** e vice-versa através de atualizar o mecanismo de atualização (ou especificação no manifesto de aplicação de serviço no predefinido) não é atualmente suportada. Suporte para esta funcionalidade será colocada em versões futuras.

## <a name="choosing-between-shared-process-and-exclusive-process-model"></a>Escolher entre processo partilhado e o modelo de processo exclusivo
Ambos estes modelos de alojamento tem respetivo os profissionais de TI e contras e necessidades de utilizadores para avaliar a que um adequada aos requisitos do seu melhor. **Partilhado processo** modelo permite uma melhor utilização de recursos de SO porque processos menos são gerados, várias réplicas no mesmo processo podem partilhar portas, etc. No entanto, se uma das réplicas chegar a um erro em que tem de colocar para baixo o anfitrião do serviço, irá afetar todos os outras réplicas no mesmo processo.

 **Processo exclusivo** modelo fornece isolamento melhor com cada réplica num processo próprio e uma réplica funcionar incorretamente não irá afetar outras réplicas. Esta é apresentada no útil nos casos em que a partilha de porta não é suportado pelo protocolo de comunicação. -Facilita a capacidade de aplicar a governação de recursos ao nível de réplica. No entanto, **processo exclusivo** irá consumir mais recursos de SO cria um processo para cada réplica no nó.

## <a name="exclusive-process-model-and-application-model-considerations"></a>Considerações de modelo do modelo de processo exclusivo e aplicação
A forma recomendada para modelar a sua aplicação no Service Fabric é manter um *ServiceType* por *ServicePackage* e este modelo funciona bem para a maioria das aplicações. 

Concebida para certos casos de utilização, recursos de infraestrutura de serviço também permite que mais do que um *ServiceType* por *ServicePackage* (e um *CodePackage* pode registar mais do que um  *ServiceType*). Seguem-se alguns dos cenários em que estas configurações podem ser úteis:

- Pretende otimizar a utilização de recursos do SO ao gerar menos processos e ter superior densidade de réplica por processo.
- Réplicas de ServiceTypes diferentes têm de partilhar alguns dados comuns que tem um inicialização elevada ou o custo de memória.
- Tiver uma oferta de serviço gratuito e que pretende colocar em pausa um limite de utilização de recursos colocando todas as réplicas do serviço no mesmo processo.

**Processo exclusivo** alojamento modelo não é coherent com o modelo de aplicação ter vários *ServiceTypes* por *ServicePackage*. Isto acontece porque vários *ServiceTypes* por *ServicePackage* foram concebidos para alcançar partilha entre as réplicas e permite réplica densidade superior por processo de recursos superior. Este é contrária ao disposto que **processo exclusivo** modelo foi concebido para alcançar.

Considere o caso de vários *ServiceTypes* por *ServicePackage* com diferentes *CodePackage* registar cada *ServiceType*. Vamos supor que temos uma *ServicePackage* 'MultiTypeServicePackge', que tem dois *CodePackages*:

- 'MyCodePackageA', que regista *ServiceType* 'MyServiceTypeA'.
- 'MyCodePackageB', que regista *ServiceType* 'MyServiceTypeB'.

Agora, diga, criamos um *aplicação* **recursos de infraestrutura: / SpecialApp** e dentro **recursos de infraestrutura: / SpecialApp** iremos criar dois serviços com os seguintes **processo exclusivo** modelo:

- Serviço **fabric: / SpecialApp/ServiceA** do tipo 'MyServiceTypeA' com duas partições (diga **P1** e **P2**) e três réplicas por partição.
- Serviço **fabric: / SpecialApp/ServiceB** do tipo 'MyServiceTypeB' com duas partições (diga **P3** e **P4**) e três réplicas por partição.

Num determinado nó, ambos os serviços tem duas réplicas. Uma vez que utilizámos **processo exclusivo** modelo para criar serviços, recursos de infraestrutura de serviço irá ativar uma nova cópia 'MyServicePackge' para cada réplica. Cada ativação de 'MultiTypeServicePackge' iniciará uma cópia de 'MyCodePackageA' e 'MyCodePackageB'. No entanto, apenas um dos 'MyCodePackageA' ou 'MyCodePackageB' irá alojar a réplica para o qual 'MultiTypeServicePackge' foi ativado. Diagrama seguinte mostra a vista de nó:

<center>
![Vista de nó da aplicação implementada][node-view-five]
</center>

Na ativação de 'MultiTypeServicePackge' para a réplica da partição **P1** do serviço **fabric: / SpecialApp/ServiceA**, 'MyCodePackageA' está a alojar a réplica e 'MyCodePackageB' está a funcionar . Da mesma forma, na ativação de 'MultiTypeServicePackge' para a réplica da partição **P3** do serviço **fabric: / SpecialApp/ServiceB**, 'MyCodePackageB' está a alojar a réplica e 'MyCodePackageA' é de apenas cópia de segurança e em execução e assim sucessivamente. Por conseguinte, mais o número de *CodePackages* (registar diferentes *ServiceTypes*) por *ServicePackage*, maior será a utilização de recursos redundante. 
 
 No entanto, se criamos serviços **recursos de infraestrutura: SpecialApp/ServiceA** e **recursos de infraestrutura: SpecialApp/ServiceB** com **processo partilhado** ativados de recursos de infraestrutura de modelo, é apenas um cópia de MultiTypeServicePackge para *aplicação* **fabric: / SpecialApp** (uma vez que vimos anteriormente). 'MyCodePackageA' irá alojar todas as réplicas para o serviço **fabric: / SpecialApp/ServiceA** (ou de qualquer serviço do tipo 'MyServiceTypeA' para ser mais precisas). 'MyCodePackageB' irá alojar todas as réplicas para o serviço **fabric: / SpecialApp/ServiceB** (ou de qualquer serviço do tipo 'MyServiceTypeB' para ser mais precisas). O diagrama seguinte mostra a vista de nó nesta definição: 

<center>
![Vista de nó da aplicação implementada][node-view-six]
</center>

No exemplo anterior, se pensa se 'MyCodePackageA' regista 'MyServiceTypeA' e 'MyServiceTypeB' e não há nenhum MyCodePackageB, em seguida, existirá não redundante *CodePackage* em execução. Estiver correto, no entanto, como mencionadas anteriormente. Este modelo de aplicação não estão alinhados com **processo exclusivo** modelo de alojamento. Se o objetivo é colocar cada réplica num processo próprio dedicado, em seguida, registar ambos *ServiceTypes* do mesmo *CodePackage* não é necessária. Colocar cada *ServiceType* no seu próprio *ServicePacakge* é uma opção mais natural.

## <a name="next-steps"></a>Passos Seguintes
[Uma aplicação do pacote] [ a4] e prepará-lo a implementar.

[Implementar e remover aplicações] [ a5] descreve como utilizar o PowerShell para gerir instâncias da aplicação.

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
