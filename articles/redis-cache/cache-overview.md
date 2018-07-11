---
title: O que é a Cache de Redis do Azure? | Microsoft Docs
description: Saiba o que é a Cache de Redis do Azure e como é normalmente utilizada.
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: overview
ms.date: 03/26/2018
ms.author: wesmc
ms.custom: mvc
ms.openlocfilehash: 585dcd120c42562b1520d4454f9d04e445553101
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37096197"
---
# <a name="what-is-azure-redis-cache"></a>O que é a Cache de Redis do Azure?

A Cache de Redis do Azure é baseada na popular [Cache de Redis](https://redis.io/) de código aberto. Normalmente, é utilizada como uma cache para melhorar o desempenho e a escalabilidade dos sistemas que dependem bastante de arquivos de dados de back-end. O desempenho é melhorado ao copiar temporariamente os dados acedidos com mais frequência para o armazenamento rápido localizado próximo da aplicação. Com a [Cache de Redis](https://redis.io/), este armazenamento rápido está localizado na memória com a Cache de Redis, em vez de ser carregado a partir do disco por uma base de dados.

A Cache de Redis do Azure também pode ser utilizada como um arquivo da estrutura de dados na memória, uma base de dados não relacional distribuída e um mediador de mensagens. O desempenho da aplicação é melhorado ao tirar partido do desempenho da latência baixa e do alto débito do motor de Redis.

A Cache de Redis do Azure dá-lhe acesso a uma cache de Redis segura e dedicada, gerida pela Microsoft, e acessível a partir de qualquer aplicação dentro ou fora do Azure.

## <a name="why-use-azure-redis-cache"></a>Porquê utilizar a Cache de Redis do Azure?

Existem muitos padrões comuns em que a Cache de Redis serve para suportar a arquitetura da aplicação ou para melhorar o desempenho da aplicação. Alguns dos mais comuns incluem o seguinte:

| Padrão      | Descrição                                        |
| ------------ | -------------------------------------------------- |
| [Cache-Aside](cache-web-app-cache-aside-leaderboard.md) | Uma vez que uma base de dados pode ser grande, o carregamento de uma base de dados completa numa cache não é uma abordagem recomendada. É comum utilizar o padrão [cache-aside](https://docs.microsoft.com/azure/architecture/patterns/cache-aside) para carregar os itens de dados para a cache apenas quando necessários. Quando o sistema realiza alterações aos dados de back-end, este também pode a qualquer momento atualizar a cache, que é distribuída com outros clientes. Além disso, o sistema pode definir uma validade para os itens de dados ou utilizar uma política de expulsão para fazer com que as atualizações de dados sejam recarregadas para a cache.|
| [Colocação em Cache de Conteúdos](cache-aspnet-output-cache-provider.md) | A maioria das páginas Web é gerada a partir de modelos com cabeçalhos, rodapés, barras de ferramentas, menus, etc. Não são alteradas frequentemente nem devem ser geradas dinamicamente. Ao utilizar uma cache na memória, como a Cache de Redis do Azure, obtém um acesso rápido aos servidores Web para este tipo de conteúdo estático comparativamente aos arquivos de dados de back-end. Este padrão reduz o tempo de processamento e a carga do servidor que seria precisa para gerar conteúdos dinamicamente. Tal permite que os servidores Web sejam mais reativos e pode permitir reduzir o número de servidores necessários para processar as cargas. A Cache de Redis do Azure disponibiliza o Fornecedor de Cache de Saída de Redis para ajudar a suportar este padrão com ASP.NET.|
| [Colocação em cache da sessão de utilizador](cache-aspnet-session-state-provider.md) | Este padrão é normalmente utilizado com carrinhos de compras e outras informações do tipo de histórico do utilizador que uma aplicação Web possa querer associar aos cookies de utilizador. Armazenar demasiada informação num cookie pode ter um impacto negativo no desempenho, uma vez que o tamanho deste cresce à medida que é transmitido e validado com cada pedido. Uma solução típica passa por utilizar o cookie como uma chave para consultar dados numa base de dados de back-end. Utilizar uma cache na memória, como a Cache de Redis do Azure, para associar informações a um utilizador é muito mais rápido do que interagir com uma base de dados totalmente relacional. |
| Colocação em fila de tarefas e mensagens | Quando as aplicações recebem pedidos, muitas vezes, as operações associadas ao pedido demoram mais tempo a executar. Uma prática comum é o diferimento de operações mais longas em execução ao adicionar as mesmas numa fila, sendo estas processadas mais tarde e possivelmente por outro servidor. Este método de diferimento do trabalho é denominado colocação em fila de tarefas. Existem muitos componentes de software concebidos para suportar as filas de tarefas. A Cache de Redis também cumpre bem esta finalidade como uma fila distribuída.|
| Transações distribuídas | Um requisito comum das aplicações é a sua capacidade para executar uma série de comandos num arquivo de dados de back-end como uma única operação (atómica). Todos os comandos têm de ser realizados com êxito, caso contrário, devem ser todos revertidos para o estado inicial. A Cache de Redis suporta a execução de um lote de comandos como uma única operação sob a forma de [Transações](https://redis.io/topics/transactions). |

## <a name="azure-redis-cache-offerings"></a>Ofertas de Cache de Redis do Azure

A Cache de Redis do Azure está disponível nos seguintes escalões:

| Escalão | Descrição |
|---|---|
Básica | Uma cache de nó único. Este escalão suporta vários tamanhos de memória (250 MB a 53 GB). É um escalão ideal para desenvolvimento/teste e cargas de trabalho não críticas. O escalão Básico não tem nenhum contrato de nível de serviço (SLA) |
| Standard | Uma cache replicada numa configuração primária/secundária de dois nós gerida pela Microsoft, com um SLA de elevada disponibilidade (99,9%) |
| Premium | O escalão Premium é um escalão preparado para utilização empresarial. As Caches de escalão Premium suportam mais funcionalidades e têm um maior débito com menores latências. As Caches no escalão Premium são implementadas em hardware mais poderoso e fornecem um melhor desempenho comparativamente aos Escalões Básico ou Standard. Esta vantagem significa que o débito de uma cache do mesmo tamanho será superior no escalão Premium do que no Standard |

> [!TIP]
> Para obter mais informações sobre o tamanho, o débito e a largura de banda com caches Premium, veja [FAQ sobre a Cache de Redis do Azure](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).
>

Pode dimensionar a sua cache para um escalão superior depois de esta ter sido criada. A diminuição para um escalão inferior não é suportada. Para obter instruções passo a passo sobre o dimensionamento, veja [Como Dimensionar a Cache de Redis do Azure](cache-how-to-scale.md) e [Como automatizar uma operação de dimensionamento](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

### <a name="feature-comparision"></a>Comparação das Funcionalidades

A página [Preços da Cache de Redis](https://azure.microsoft.com/pricing/details/cache/) fornece uma comparação detalhada de cada escalão. A tabela seguinte ajuda a descrever algumas das funcionalidades suportadas pelo escalão:

| Descrição da Funcionalidade | Premium | Standard | Básica |
| ------------------- | :-----: | :------: | :---: |
| [Contrato de Nível de Serviço (SLA)](https://azure.microsoft.com/support/legal/sla/cache/v1_0/) |✔|✔|-|
| [Persistência de dados de Redis](cache-how-to-premium-persistence.md) |✔|-|-|
| [Cluster de Redis](cache-how-to-premium-clustering.md) |✔|-|-|
| [Segurança através de regras da Firewall](cache-configure.md#firewall) |✔|✔|✔|
| [Segurança e isolamento otimizados com VNet](cache-how-to-premium-vnet.md) |✔|-|-|
| [Importação/Exportação](cache-how-to-import-export-data.md) |✔|-|-|
| [Atualizações agendadas](cache-administration.md#schedule-updates) |✔|-|-|
| [Georreplicação](cache-how-to-geo-replication.md) |✔|-|-|
| [Reiniciar](cache-administration.md#reboot) |✔|✔|✔|

## <a name="next-steps"></a>Passos seguintes

* [Início Rápido da Aplicação Web ASP.NET](cache-web-app-howto.md) – crie uma aplicação Web ASP.NET simples que utiliza uma Cache de Redis do Azure.
* [Início Rápido de .NET](cache-dotnet-how-to-use-azure-redis-cache.md) – crie uma aplicação .NET que utiliza uma Cache de Redis do Azure.
* [Início Rápido de .NET Core](cache-dotnet-core-quickstart.md)Crie uma aplicação .NET Core que utiliza uma Cache de Redis do Azure.
* [Início Rápido de Node.js](cache-nodejs-get-started.md) – crie uma aplicação Node.js simples que utiliza uma Cache de Redis do Azure.
* [Início Rápido de Java](cache-java-get-started.md) – crie uma aplicação Java simples que utiliza uma Cache de Redis do Azure.
* [Início Rápido de Python](cache-python-get-started.md) – crie uma aplicação Python que utiliza uma Cache de Redis do Azure.
