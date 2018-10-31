---
title: Distribuir dados globalmente com o Azure Cosmos DB | Documentos da Microsoft
description: Mais informações sobre recuperação de dados, ativação pós-falha, com vários mestres e georreplicação de escala planetária com bases de dados global do Azure Cosmos DB, um serviço de base de dados com múltiplos modelos distribuída globalmente.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: mjbrown
ms.openlocfilehash: 68ac603964593892dfcbc3488b4e6f2c91a0eb92
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50238279"
---
# <a name="global-data-distribution-with-azure-cosmos-db"></a>Distribuição de dados global com o Azure Cosmos DB

Muitos dos aplicativos atuais executam ou tem ambições para executar à escala planetária. Eles estão sempre ativos e estão acessíveis aos utilizadores em todo o mundo. Gerir a distribuição global de dados que são utilizados por esses aplicativos, fornecendo alto desempenho e elevada disponibilidade é um problema difícil. O cosmos DB é um serviço de base de dados globalmente distribuída que tem sido concebido de raiz cópia de segurança para atender a esses desafios.

O cosmos DB é um serviço do Azure fundamental e está disponível em todos os [regiões do Azure](https://azure.microsoft.com/global-infrastructure/regions/) por predefinição. A Microsoft centros de dados do Azure em mais de 50 regiões em todo o mundo e continua a expandir para satisfazer as necessidades de cada vez mais dos clientes. O Cosmos DB, a Microsoft opera 24 x 7, do serviço para que se possa focar nas suas aplicações. Quando cria uma conta do Cosmos DB, decidir quais regiões devem ser implementada no.

Os clientes do cosmos DB, podem configurar seus bancos de dados globalmente distribuída e disponível em qualquer lugar a partir de 1 a 50 + regiões do Azure. Para reduzir a latência, deve colocar os dados mais próximos para a localização dos seus utilizadores, então, decidir o número de regiões e no qual a região para executar depende global alcance da sua aplicação e onde estão os utilizadores. O cosmos DB replica de forma transparente todos os dados na sua conta do Cosmos para todas as regiões configuradas. O cosmos DB fornece uma única imagem do sistema de base de dados do Cosmos e contentores, para que seu aplicativo pode ler e escrever localmente. Com o Cosmos DB, pode adicionar ou remover regiões associadas à sua conta em qualquer altura. Seu aplicativo não precisa de ser colocada em pausa ou novamente implementada e continua a ser de elevada disponibilidade, que serve dados durante todo o tempo, graças às capacidades multi homing que fornece o serviço.

## <a name="key-benefits-of-global-distribution"></a>Principais benefícios da distribuição global

**Criar aplicações globais de ativo-ativo, facilmente**: com a capacidade de vários mestre, todas as regiões é gravável (além de ser legível), permitindo elástico ilimitada escalabilidade, a leitura de 99,999% de escrever a disponibilidade e escrita, em todo o mundo, e a garantia de leituras rápidas e gravações servidas em menos de 10 milissegundos, no percentil 99.  

Com o Azure Cosmos DB APIs multi-homing, seu aplicativo sempre sabe a região mais próxima e envia pedidos para essa região. Região mais próxima é identificada sem quaisquer alterações de configuração. Como adicionar e remover regiões da sua conta do Cosmos DB, seu aplicativo não precisa de ser novamente implementada e continua a ser de elevada disponibilidade.

**Crie aplicações altamente responsivos**: seu aplicativo pode ser facilmente desenvolvido para executar quase em tempo real leituras e escritas, com latências de milissegundo de dígito, em relação a todas as regiões que escolheu para a base de dados.  O Azure Cosmos DB processa internamente a replicação de dados entre regiões de forma que garante o nível de consistência escolhido para a conta do Cosmos.

Muitos aplicativos irão se beneficiar as melhorias de desempenho que vêm com a capacidade de executar gravações (locais) de várias regiões. Alguns aplicativos, como aqueles que exigem uma consistência forte, preferirá para todas as gravações de funil de uma única região. O cosmos DB suporta configurações, região única e várias regiões.

**Crie aplicações altamente disponíveis**: executar uma base de dados em várias regiões melhora a disponibilidade da base de dados. Se uma região não estiver disponível, outras regiões manipula automaticamente os pedidos de aplicações. O Azure Cosmos DB oferece 99,999% de leitura e escrita de disponibilidade para bases de dados de várias regiões.

**Continuidade do negócio durante falhas regionais**: Azure Cosmos DB suporta [a ativação pós-falha automática](how-to-manage-database-account.md#enable-automatic-failover-for-your-cosmos-account) durante uma falha regional. Além disso, durante uma falha regional, o Cosmos DB continua a manter a latência, disponibilidade, consistência e SLAs de débito. Para ajudar a garantir que todo o seu aplicativo está altamente disponível, o Azure Cosmos DB oferece API de ativação pós-falha manual para simular uma falha regional. Ao utilizar esta API, pode realizar testes de continuidade comercial normal e estar pronto.

**Global de leitura e escrita escalabilidade**: com capacidade de vários mestre, pode dimensionar leitura e escrita débito elástica em todo o mundo. Capacidade de vários mestre garante que o débito que configura o seu aplicativo num banco de dados do Azure Cosmos DB ou um contentor é entregue em todas as regiões e protegido por [apoiado financeiramente SLAs](https://aka.ms/acdbsla).

**Vários modelos de consistência bem definidos**: protocolo de replicação do Azure Cosmos DB foi criado para oferecer cinco bem definidos, práticos e modelos de consistência intuitiva, cada um com uma compensação clara entre a consistência e desempenho. Estes modelos de consistência permitem-lhe criar aplicações distribuídas globalmente corretas com facilidade.

## <a id="Next Steps"></a>Passos seguintes

Leia mais sobre a distribuição global nos seguintes artigos:

* [Distribuição global - sob definições avançadas](global-dist-under-the-hood.md)
* [Como configurar clientes para multi-homing](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [Como adicionar ou remover regiões da sua base de dados](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [Como criar uma política de resolução de conflito personalizado para contas da API de SQL](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)