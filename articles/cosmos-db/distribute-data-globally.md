---
title: Distribuir dados globalmente com o Azure Cosmos DB | Documentos da Microsoft
description: Saiba mais sobre a recuperação de dados, ativação pós-falha, com vários mestres e georreplicação de escala planetária através da utilização de bases de dados global do Azure Cosmos DB, um serviço de base de dados com múltiplos modelos distribuída globalmente.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: mjbrown
ms.openlocfilehash: 6849574b9d16a9d76fffd4d69742c85941300e89
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/30/2018
ms.locfileid: "52679482"
---
# <a name="global-data-distribution-with-azure-cosmos-db"></a>Distribuição de dados global com o Azure Cosmos DB

Aplicações atuais são necessários para estar sempre online e elevada capacidade de resposta. Para alcançar a baixa latência e elevada disponibilidade, instâncias desses aplicativos precisam ser implantados nos centros de dados que estão próximas dos seus usuários. Esses aplicativos são normalmente implementados em vários datacenters e são chamados e distribuídos globalmente. Aplicações distribuídas globalmente tem uma base de dados distribuído globalmente que transparente pode replicar os dados em qualquer parte do mundo para permitir que os aplicativos operar numa cópia dos dados que está próximo de seus usuários. 

O Azure Cosmos DB é um serviço de base de dados globalmente distribuída que foi concebido para fornecer a baixa latência, escalabilidade elástica de débito, semântica bem definida para consistência dos dados e elevada disponibilidade. Em resumo, se seu aplicativo precisa tempo de resposta rápida garantido em qualquer lugar do mundo, caso seja necessário para estar sempre online e necessita de uma escalabilidade ilimitada e elástica de débito e armazenamento, considere a criação de aplicativos com o Azure Cosmos DB.

Pode configurar seus bancos de dados para ser globalmente distribuídas e disponível em qualquer uma das regiões do Azure. Para reduzir a latência, coloque os dados mais próximos para onde estão os utilizadores. Escolher as regiões necessárias depende o alcance global da sua aplicação e onde estão localizados os seus utilizadores. O Azure Cosmos DB replica de forma transparente os dados na sua conta para todas as regiões à sua conta. Ele fornece uma única imagem do sistema de distribuídas globalmente base de dados do Cosmos do Azure e contentores que seu aplicativo pode ler e escrever localmente. 

Com o Azure Cosmos DB, pode adicionar ou remover regiões associadas à sua conta em qualquer altura. Seu aplicativo não precisa ser colocada em pausa ou implantados novamente para adicionar ou remover uma região. Continua a ser de elevada disponibilidade o tempo todo por causa dos recursos em multihoming, que fornece o serviço.

## <a name="key-benefits-of-global-distribution"></a>Principais benefícios da distribuição global

**Crie aplicações globais de ativo-ativo.** Com a funcionalidade de vários mestre, todas as regiões são uma região de escrita. Também é legível. Também garante a funcionalidade de vários mestre:

- Escalabilidade de escrita de elástico ilimitado. 
- 99,999% de leitura e escrita a disponibilidade em todo o mundo.
- A garantia de leituras e gravações servidas em menos de 10 milissegundos no percentil 99.

Ao utilizar a APIs de multihoming do Azure Cosmos DB, a aplicação está atento a região mais próxima. Em seguida, pode enviar pedidos para essa região. A região mais próxima é identificada sem quaisquer alterações de configuração. Como adicionar e remover regiões da sua conta do Azure Cosmos DB, seu aplicativo não precisa de voltar a implementar. A aplicação continua a ser de elevada disponibilidade.

**Crie aplicações de elevada capacidade de resposta.** Seu aplicativo pode ser facilmente concebido para efetuar quase em tempo real leituras e gravações. Ele pode usar latências de milissegundo de dígito contra todas as regiões que escolheu para a sua base de dados. O Azure Cosmos DB processa internamente a replicação de dados entre regiões. Como resultado, é garantido que o nível de consistência selecionado para a conta do Azure Cosmos DB.

Muitos aplicativos se beneficiam com as melhorias de desempenho que vêm com a capacidade de executar gravações (locais) de várias regiões. Alguns aplicativos que exigem uma consistência forte preferem todas as gravações de funil de uma única região. Para estas aplicações, o Azure Cosmos DB suporta a região única e configurações de várias regiões.

**Crie aplicações de elevada disponibilidade.** Executar uma base de dados em várias regiões, aumenta a disponibilidade da base de dados. Se uma região não estiver disponível, outras regiões processam automaticamente pedidos de aplicações. O Azure Cosmos DB oferece 99,999% de leitura e escrita de disponibilidade para bases de dados de várias regiões.

**Manter a continuidade do negócio durante falhas regionais.** Azure Cosmos DB suporta [a ativação pós-falha automática](how-to-manage-database-account.md#automatic-failover) durante uma falha regional. Durante uma falha regional, o Azure Cosmos DB continua manter SLAs, disponibilidade, consistência, débito e latência. Para ajudar a tornar-se de que todo o seu aplicativo está altamente disponível, o Azure Cosmos DB oferece uma ativação pós-falha manual API para simular uma falha regional. Ao utilizar esta API, pode executar testes de continuidade comercial normal.

**Escala de leitura e escrita débito globalmente.** Com a funcionalidade de vários mestre, pode dimensionar leitura e escrita débito elástica em todo o mundo. A funcionalidade de vários mestre garante o débito que configura o seu aplicativo num banco de dados do Azure Cosmos DB ou um contentor é entregue em todas as regiões. O débito também é protegido pelo [apoiado financeiramente SLAs](https://aka.ms/acdbsla).

**Escolha entre vários modelos de consistência bem definidos.** O protocolo de replicação do Azure Cosmos DB oferece cinco modelos de consistência bem definidos, práticos e intuitivos. Cada modelo tem um compromisso entre a consistência e desempenho. Utilize estes modelos de consistência para criar aplicações distribuídas globalmente com facilidade.

## <a id="Next Steps"></a>Passos seguintes

Leia mais sobre a distribuição global nos seguintes artigos:

* [Distribuição global - sob definições avançadas](global-dist-under-the-hood.md)
* [Configurar os clientes de multihoming](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [Adicionar ou remover regiões a partir da sua conta do Azure Cosmos DB](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [Criar uma política de resolução de conflito personalizado para contas da API de SQL](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)
