---
title: Ativação pós-falha no Azure Cosmos DB | Documentos da Microsoft
description: Saiba mais sobre como manual e automático funciona a ativação pós-falha com o Azure Cosmos DB.
services: cosmos-db
author: kanshiG
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/27/2018
ms.author: govindk
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 697be3a1eb07b2f2650f3dd94fd835b9431aec6b
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/10/2018
ms.locfileid: "42054551"
---
# <a name="automatic-regional-failover-for-business-continuity-in-azure-cosmos-db"></a>Ativação pós-falha automática de regional para continuidade do negócio no Azure Cosmos DB
O Azure Cosmos DB simplifica a distribuição global de dados, oferecendo totalmente gerido e de [contas de base de dados de várias regiões](distribute-data-globally.md) que fornecem claras as responsabilidades entre consistência, disponibilidade e desempenho, tudo com correspondente garantias. Contas cosmos DB oferecem elevada disponibilidade, latências de ms de único dígito, [níveis de consistência bem definidos](consistency-levels.md), ativação pós-falha regional transparente com APIs multi-homing e a capacidade de dimensionar de forma elástica o débito e o armazenamento em todo o mundo. 

O cosmos DB suporta tanto explícitas e políticas controladas por ativações pós-falha que permitem controlar o comportamento do sistema de ponta a ponta em caso de falhas. Neste artigo, vamos ver:

* Como funcionam as ativações pós-falha manuais no Cosmos DB?
* Como as ativações pós-falha automáticas de trabalho no Cosmos DB e o que acontece quando um data center vai para baixo?
* Como pode utilizar as ativações pós-falha manual em arquiteturas de aplicações?

Também pode aprender sobre as ativações pós-falha regionais neste vídeo, o Azure Cosmos DB gerente Andrew Liu, que demonstra as funcionalidades de distribuição global, incluindo a ativação pós-falha.

>[!VIDEO https://www.youtube.com/embed/1D06yjTVxt8]
>

## <a id="ConfigureMultiRegionApplications"></a>Configurar aplicações de várias regiões
Antes de mergulhar nas modos de ativação pós-falha, vamos ver como pode configurar uma aplicação para tirar partido da disponibilidade de várias regiões e ser resiliente em caso de ativações pós-falha regionais.

* Em primeiro lugar, implemente a sua aplicação em várias regiões
* Para garantir o acesso de baixa latência em cada região que a aplicação é implementada, configure o correspondente [lista de regiões preferencial](https://msdn.microsoft.com/library/microsoft.azure.documents.client.connectionpolicy.preferredlocations.aspx#P:Microsoft.Azure.Documents.Client.ConnectionPolicy.PreferredLocations) para cada região através de um dos SDKs suportados.

O fragmento seguinte mostra como inicializar uma aplicação de várias regiões. Aqui, a conta do Azure Cosmos DB `contoso.documents.azure.com` está configurada com duas regiões - E.U.A. oeste e Europa do Norte. 

* A aplicação é implementada na região E.U.A. oeste (usando serviços aplicacionais do Azure por exemplo) 
* Configurado com `West US` como a primeira região preferencial de baixa latência lê
* Configurado com `North Europe` como da segunda região preferencial (para elevada disponibilidade durante falhas regionais)

Na API do SQL, esta configuração é como o seguinte fragmento:

```cs
ConnectionPolicy usConnectionPolicy = new ConnectionPolicy 
{ 
    ConnectionMode = ConnectionMode.Direct,
    ConnectionProtocol = Protocol.Tcp
};

usConnectionPolicy.PreferredLocations.Add(LocationNames.WestUS);
usConnectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope);

DocumentClient usClient = new DocumentClient(
    new Uri("https://contosodb.documents.azure.com"),
    "<Fill your Cosmos DB account's AuthorizationKey>",
    usConnectionPolicy);
```

A aplicação for também implementada na região Europa do Norte, com a ordem das regiões preferenciais invertido. Ou seja, a região Europa do Norte é especificado pela primeira vez, para leituras de latência baixa. Em seguida, a região EUA oeste está especificada como a segunda região preferencial para elevada disponibilidade durante falhas regionais.

O diagrama de arquitetura seguinte mostra uma implementação de aplicação de várias regiões em que o Cosmos DB e a aplicação estão configurados para estar disponível em quatro regiões geográficas do Azure.  

![Implementação de aplicações distribuídas globalmente com o Azure Cosmos DB](./media/regional-failover/app-deployment.png)

Agora, vamos dar uma olhada em como o serviço Cosmos DB lida com falhas regionais por meio de ativações pós-falha automática. 

## <a id="AutomaticFailovers"></a>Ativações pós-falha automáticas
Na rara eventualidade de uma falha regional do Azure ou de uma indisponibilidade do Centro de dados, o Cosmos DB aciona automaticamente as ativações pós-falha de todas as contas do Cosmos DB com uma presença na região afetado. 

**O que acontece se uma região de leitura tem um período de indisponibilidade?**

Contas do cosmos DB com uma região de leitura de uma das regiões afetados são automaticamente desligadas da sua região de escrita e marcada como offline. Os SDKs do Cosmos DB implementam um protocolo de deteção regionais que permite-lhes para detetar automaticamente quando uma região está disponível e redirecionamento de chamadas para a próxima região disponível na lista de região preferencial de leitura. Se nenhuma das regiões na lista de região preferencial estiver disponível, chamadas automaticamente revertam para a região de escrita atual. Sem alterações são necessárias no código da aplicação para processar as ativações pós-falha regionais. Durante todo este processo, garantias de consistência continuam a ser cumpridas do Cosmos DB.

![Falhas de região de leitura no Azure Cosmos DB](./media/regional-failover/read-region-failures.png)

Assim que a região afetada recupera da falha, as contas do Cosmos DB afetadas na região são recuperadas automaticamente pelo serviço. Contas cosmos DB que tinha uma região de leitura na região afetado, em seguida, sincronizar com a região de escrita atual e automaticamente ativar online. Os SDKs do Cosmos DB detetar a disponibilidade da região novo e avaliar se a região deve ser selecionada como a região de leitura atual com base na lista de região preferencial configurada pelo aplicativo. Leituras subseqüentes são redirecionadas para a região recuperada sem exigir alterações ao código da aplicação.

**O que acontece se uma região de escrita tem um período de indisponibilidade?**

Se a região afetada é a região de escrita atual e a ativação pós-falha automática está ativada para a conta do Azure Cosmos DB, isto, em seguida, que a região é automaticamente marcada como offline. Em seguida, uma região alternativa é promovida como a região de escrita para a conta do Azure Cosmos DB afetada. Pode ativar a ativação pós-falha automática e totalmente controlar a ordem de seleção de região para as suas contas do Azure Cosmos DB através do portal do Azure ou [programaticamente](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/databaseaccounts#DatabaseAccounts_FailoverPriorityChange). 

![Prioridades de ativação pós-falha para o Azure Cosmos DB](./media/regional-failover/failover-priorities.png)

Durante as ativações pós-falha automática, o Azure Cosmos DB escolhe automaticamente a próxima escrever região para uma conta do Azure Cosmos DB especificada com base na ordem de prioridade especificada. Aplicativos podem usar a propriedade de WriteEndpoint da classe de DocumentClient para detetar a alteração na região de escrita.

![Falhas de região de gravação no Azure Cosmos DB](./media/regional-failover/write-region-failures.png)

Assim que a região afetada recupera da falha, as contas do Cosmos DB afetadas na região são recuperadas automaticamente pelo serviço. 

* Dados presentes na região de escrita anterior que não foram replicado para regiões de leitura durante a falha for publicados como um conflito de feed. Aplicações podem ler o feed de conflito, resolver conflitos com base na lógica específica da aplicação e escrever os dados atualizados de volta para a conta do Azure Cosmos DB, conforme apropriado. 
* A região de escrita anterior é recriada como uma região de leitura e novamente colocada online automaticamente. 
* Pode reconfigurar a região de leitura foi recolocado online automaticamente como a região de escrita ao realizar uma ativação pós-falha manual através do portal do Azure ou [programaticamente](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/databaseaccounts#DatabaseAccounts_CreateOrUpdate).

O fragmento de código seguinte ilustra como processar conflitos depois da região afetada recupera da falha.

```cs
string conflictsFeedContinuationToken = null;
do
{
    FeedResponse<Conflict> conflictsFeed = client.ReadConflictFeedAsync(collectionLink,
        new FeedOptions { RequestContinuation = conflictsFeedContinuationToken }).Result;

    foreach (Conflict conflict in conflictsFeed)
    {
        Document doc = conflict.GetResource<Document>();
        Console.WriteLine("Conflict record ResourceId = {0} ResourceType= {1}", conflict.ResourceId, conflict.ResourceType);

        // Perform application specific logic to process the conflict record / resource
    }

    conflictsFeedContinuationToken = conflictsFeed.ResponseContinuation;
} while (conflictsFeedContinuationToken != null);
```

## <a id="ManualFailovers"></a>Ativações pós-falha manual

Além de ativações pós-falha automática, a região de escrita atual de uma determinada conta do Cosmos DB pode ser manualmente alterada dinamicamente para uma das regiões de leitura existentes. As ativações pós-falha manual podem ser iniciadas através do portal do Azure ou [programaticamente](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/databaseaccounts#DatabaseAccounts_CreateOrUpdate). 

Certifique-se de ativações pós-falha manuais **zero perda de dados** e **zero disponibilidade** perda e corretamente o estado de escrita de transferência da antiga região de escrita para a nova para a conta do Cosmos DB especificada. Como as ativações pós-falha automática, o SDK do Cosmos DB automaticamente manipula as alterações de região de escrita durante as ativações pós-falha manuais e garante que as chamadas são automaticamente redirecionadas para a nova região de escrita. Sem alterações de configuração ou de código são necessários na sua aplicação para gerir as ativações pós-falha. 

![Ativações pós-falha manuais no Azure Cosmos DB](./media/regional-failover/manual-failovers.png)

Alguns dos cenários comuns em que a ativação pós-falha manual pode ser útil são:

**Siga o modelo de relógio**: se seus aplicativos têm padrões de tráfego previsíveis com base na hora do dia, pode alterar periodicamente o estado de escrita para a região geográfica mais ativa, com base na hora do dia.

**A atualização do serviço**: determinada implementação de aplicação globalmente distribuída pode envolver a reencaminhar tráfego para uma região diferente através do Gestor de tráfego durante a atualização de seus planejado de manutenção. Essa implementação de aplicações agora pode utilizar a ativação pós-falha manual para manter o estado de escrita para a região onde haverá ser tráfego ativo durante a janela de atualização de serviço.

**Continuidade do negócio e recuperação após desastre (BCDR) e explorações de elevada disponibilidade e recuperação após desastre (HADR)**: maioria dos aplicativos empresariais incluir testes de continuidade de negócio como parte do seu processo de desenvolvimento e liberação. BCDR e HADR testes, muitas vezes, é uma etapa importante na certificações de conformidade e garantindo a disponibilidade do serviço no caso de falhas regionais. Pode testar a preparação BCDR das suas aplicações que utilizam o Cosmos DB para o armazenamento ao acionar uma ativação pós-falha manual da sua conta do Cosmos DB e/ou adicionar e remover dinamicamente uma região.

Neste artigo, revimos como manual e automático as ativações pós-falha de trabalho no Cosmos DB, e como pode configurar suas contas do Cosmos DB e aplicativos para ser globalmente disponível. Ao utilizar o suporte de replicação global do Cosmos DB, pode melhorar a latência de ponto-a-ponto e certifique-se de que eles são de elevada disponibilidade mesmo em caso de falhas de região. 

## <a id="NextSteps"></a>Passos Seguintes
* Saiba mais sobre como o Cosmos DB suporta [distribuição global](distribute-data-globally.md)
* Saiba mais sobre [consistência global com o Azure Cosmos DB](consistency-levels.md)
* Desenvolver com várias regiões com o Azure Cosmos DB [API de SQL](tutorial-global-distribution-sql-api.md)
* Aprenda a criar [arquiteturas de escritor de várias regiões](multi-region-writers.md) com o Azure Cosmos DB

