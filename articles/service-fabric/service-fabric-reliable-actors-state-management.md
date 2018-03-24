---
title: Gestão de estado de Reliable Actors | Microsoft Docs
description: Descreve como Estado Reliable Actors gerido, persistente e replicado para elevada disponibilidade.
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
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: d5d38e7fa80db3484c397d9840bbc6092e4f18bb
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="reliable-actors-state-management"></a>Gestão de estado do Reliable Actors
Reliable Actors são single-threaded objetos que podem encapsular a lógica e estado. Porque atores executam Reliable Services, estes podem manter o estado da forma fiável com o mesma persistência e mecanismos de replicação. Desta forma, atores não percam o respetivo estado após falhas, após a reativação após a recolha de lixo ou quando são movidos entre nós num cluster devido ao balanceamento de recurso ou atualizações à volta.

## <a name="state-persistence-and-replication"></a>Estado persistência e replicação
Todos os Reliable Actors são considerados *com monitorização de estado* porque cada instância de ator está mapeado para um ID exclusivo. Isto significa que repetido chamadas para o mesmo ID de actor são encaminhadas para a mesma instância de atores. Num sistema sem monitorização de estado, por outro lado, as chamadas de cliente não são garantidas ser encaminhados para o mesmo servidor sempre. Por este motivo, serviços de atores são sempre serviços com monitorização de estado.

Apesar de atores são considerados com monitorização de estado, não significa que eles devem armazenar o estado de forma fiável. Atores podem escolher o nível de persistência do Estado e a replicação com base nos seus dados requisitos de armazenamento:

* **Persistente estado**: estado é persistente para disco e são replicadas para três ou mais réplicas. Estado persistente é a opção de armazenamento de estado mais durável, onde pode manter o estado através de falha completa do cluster.
* **Estado volátil**: estado é replicado para três ou mais réplicas e apenas é mantido na memória. Estado de volátil fornece resiliência contra falhas de nó e de ator e durante a atualização e balanceamento de recurso. No entanto, o estado não é continuado no disco. Por isso, se todas as réplicas são perdidas em simultâneo, o estado é perdido bem.
* **Sem estado persistente**: Estado não é replicado ou escritos no disco, utilize apenas para os atores que não precisam de manter o estado de forma fiável.

Cada nível de persistência é simplesmente uma diferente *fornecedor de estado* e *replicação* configuração do seu serviço. Se pretende ou não é escrito Estado disco depende do fornecedor de estado--o componente de um serviço fiável que armazena o estado. Depende da replicação em réplicas quantos um serviço é implementado com. Tal como acontece com Reliable Services, o fornecedor de estado e a contagem de réplicas podem facilmente ser definidas manualmente. A estrutura de ator fornece um atributo que, quando utilizada numa ator, seleciona automaticamente um fornecedor de estado predefinido e gera automaticamente as definições da contagem de réplicas para alcançar uma destas três definições de persistência. O atributo StatePersistence não é herdado pelos classe derivada, cada tipo de Ator tem de fornecer o nível de StatePersistence.

### <a name="persisted-state"></a>Estado persistente
```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl  extends FabricActor implements MyActor
{
}
```  
Esta definição utiliza um fornecedor de estado que armazena dados no disco e define automaticamente a contagem de réplicas do serviço para 3.

### <a name="volatile-state"></a>Estado volátil
```csharp
[StatePersistence(StatePersistence.Volatile)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Volatile)
class MyActorImpl extends FabricActor implements MyActor
{
}
```
Esta definição utiliza um fornecedor de estado na memória-só e define o número de réplica para 3.

### <a name="no-persisted-state"></a>Sem estado persistente
```csharp
[StatePersistence(StatePersistence.None)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.None)
class MyActorImpl extends FabricActor implements MyActor
{
}
```
Esta definição utiliza um fornecedor de estado na memória-só e define o número de réplica para 1.

### <a name="defaults-and-generated-settings"></a>As predefinições e definições geradas
Quando estiver a utilizar o `StatePersistence` atributo, um fornecedor de estado é selecionado automaticamente para si no tempo de execução quando inicia o serviço de atores. A contagem de réplicas, no entanto, está definida no momento da compilação pelas ferramentas de compilação de atores de Visual Studio. As ferramentas de compilação geram automaticamente um *serviço predefinido* para o serviço de atores no ApplicationManifest.xml. Os parâmetros são criados para **réplica mín. Defina o tamanho** e **réplica de destino definir tamanho**.

Pode alterar manualmente estes parâmetros. Mas sempre que o `StatePersistence` atributo é alterado, os parâmetros estão definidos para os valores de tamanho de conjunto de réplica predefinidos para selecionado `StatePersistence` atributo, substituir quaisquer valores anteriores. Por outras palavras, os valores que definiu na ServiceManifest.xml são *apenas* substituída no momento da compilação quando altera o `StatePersistence` valor de atributo.

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application12Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="MyActorService_PartitionCount" DefaultValue="10" />
      <Parameter Name="MyActorService_MinReplicaSetSize" DefaultValue="3" />
      <Parameter Name="MyActorService_TargetReplicaSetSize" DefaultValue="3" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyActorPkg" ServiceManifestVersion="1.0.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MyActorService" GeneratedIdRef="77d965dc-85fb-488c-bd06-c6c1fe29d593|Persisted">
         <StatefulService ServiceTypeName="MyActorServiceType" TargetReplicaSetSize="[MyActorService_TargetReplicaSetSize]" MinReplicaSetSize="[MyActorService_MinReplicaSetSize]">
            <UniformInt64Partition PartitionCount="[MyActorService_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
         </StatefulService>
      </Service>
   </DefaultServices>
</ApplicationManifest>
```

## <a name="state-manager"></a>Gestor de estado
Todas as instâncias de ator tem o seu próprio gestor de estado: pares chave/valor uma estrutura semelhante de dicionário de dados que armazena de forma fiável. O Gestor de estado é um wrapper em torno de um fornecedor de estado. Pode utilizá-lo para armazenar dados, independentemente da que é utilizada a definição de persistência. Não fornece quaisquer garantias que um serviço de atores em execução pode ser alterado de uma definição de estado (na memória-só) volátil para uma definição de estado persistente através de uma atualização sem interrupção, mantendo os dados. No entanto, é possível alterar a contagem de réplicas para um serviço em execução.

Chaves de Gestor de estado têm de ser cadeias. Os valores são genéricos e pode ser qualquer tipo, incluindo tipos personalizados. Valores armazenados no Gestor de estado tem de ser serializável de contrato de dados porque que podem ser transmitidas através da rede para outros nós durante a replicação e podem ser gravados no disco, consoante a definição de persistência de estado de um ator.

O Gestor de estado expõe métodos de dicionário comuns para gerir o estado, semelhante às foi encontrado no dicionário fiável.

Para obter exemplos de gerir o estado do ator, leia o artigo [acesso, guardar e remover Reliable Actors estado](service-fabric-reliable-actors-access-save-remove-state.md).

## <a name="best-practices"></a>Melhores práticas
Aqui estão algumas práticas sugeridas e sugestões de resolução de problemas para gerir o estado de ator.

### <a name="make-the-actor-state-as-granular-as-possible"></a>Se o estado de ator como granulares quanto possível
Isto é essencial para o desempenho e a utilização de recursos da sua aplicação. Sempre que não há qualquer atualização/escrita para o "Estado de chamada" de um ator, o valor todo correspondente a essa "Estado denominado" é serializado e enviado através da rede para réplicas secundárias.  As bases de dados secundárias escrevem-para disco local e a resposta de volta para a réplica primária. Quando o site primário recebe confirmações de um quórum de réplicas secundárias, escreve o estado do seu disco local. Por exemplo, suponha que o valor é uma classe que tem 20 membros e um tamanho de 1 MB. Mesmo que só modificado um dos membros de classe que é do tamanho de 1 KB, fim se pagar o custo da serialização e escritas de disco e rede de 1 MB completa. Da mesma forma, se o valor é uma coleção (por exemplo, uma lista, matriz ou dicionário), paga o custo para a coleção completa, mesmo se modificar um dos membros do mesmo. A interface StateManager da classe de atores é como um dicionário. Deve sempre modelar a estrutura de dados que representa o estado de ator em cima este dicionário.
 
### <a name="correctly-manage-the-actors-life-cycle"></a>Gerir o ciclo de vida de ator corretamente
É necessário ter limpar política sobre como gerir o tamanho do Estado de cada partição de um serviço de atores. O serviço de atores deve ter um número fixo de atores e reutilize-as um tanto que possível. Se criar continuamente atores novo, tem de as eliminar depois de terminar com o seu trabalho. A estrutura de ator armazena alguns metadados sobre cada ator que existe. Eliminar todos os Estados de um ator não remove os metadados sobre esse ator. Tem de eliminar o ator (consulte [eliminação atores e o respetivo estado](service-fabric-reliable-actors-lifecycle.md#manually-deleting-actors-and-their-state)) para remover todas as informações sobre esta armazenada no sistema. Como uma verificação adicional, deve consultar o serviço de atores (consulte [enumerar atores](service-fabric-reliable-actors-platform.md)) uma vez num algum para se certificar de que o número atores estão dentro do intervalo esperado.
 
Se vir alguma vez que o tamanho de ficheiro de base de dados de um serviço de Atores é aumentar tamanho esperado, certifique-se de que está a seguir as diretrizes anteriores. Se estiver a seguir estas diretrizes e ainda são base de dados de problemas de tamanho de ficheiro, deve [abrir um pedido de suporte](service-fabric-support.md) com a equipa de produto para obter ajuda.

## <a name="next-steps"></a>Passos Seguintes

Estado que é armazenado em Reliable Actors deve ser serializado antes do respetivo escrito no disco e replicados para elevada disponibilidade. Saiba mais sobre [serialização do tipo de Ator](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).

Em seguida, saiba mais sobre [monitorização de desempenho e diagnóstico de Ator](service-fabric-reliable-actors-diagnostics.md).
