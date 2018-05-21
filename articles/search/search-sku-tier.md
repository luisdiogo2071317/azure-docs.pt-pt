---
title: Escolha um escalão de preço ou SKU de serviço da Azure Search | Microsoft Docs
description: 'A pesquisa do Azure pode ser aprovisionada nestes SKUs: gratuita, básico e padrão, está disponível em vários níveis de capacidade e configurações de recursos onde padrão.'
services: search
author: HeidiSteen
manager: cgronlun
tags: azure-portal
ms.service: search
ms.topic: conceptual
ms.date: 05/12/2018
ms.author: heidist
ms.openlocfilehash: bbf535c5b446fd654331374d29c106b6e43d55f5
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="choose-a-pricing-tier-for-azure-search"></a>Escolha um escalão de preço para pesquisa do Azure

Na Azure Search, um [serviço é aprovisionado](search-create-service-portal.md) num escalão de preço específico ou SKU. As opções incluem **livres**, **básico**, ou **padrão**, onde **padrão** está disponível em várias configurações e as capacidades. 

O objetivo deste artigo é ajudar a escolher uma camada. Complementa o [página de preços](https://azure.microsoft.com/pricing/details/search/) e [limites de serviço](search-limits-quotas-capacity.md) página com um resumo de faturação conceitos e padrões de consumo associados com várias camadas. Também recomenda uma abordagem iterativa para compreender que camada melhor satisfaz as suas necessidades. 

Camadas de determinam a capacidade, não as funcionalidades. Se a capacidade de uma camada fica ser demasiado baixo, terá de aprovisionar um novo serviço na camada superior e, em seguida, [recarregar os índices](search-howto-reindex.md). Não há nenhuma atualização no local do mesmo serviço de um SKU para outro.

Disponibilidade de funcionalidade não é uma consideração de camada principal. Todas as camadas, incluindo o **livres** camada, oferecem paridade de funcionalidades, com exceção do suporte de indexador para S3HD. No entanto, as restrições de indexação e de recursos eficazmente podem limitar a extensão de utilização da funcionalidade. Por exemplo, [pesquisa cognitivos](cognitive-search-concept-intro.md) indexação tem competências de longa execução que o limite de tempo num serviço gratuito, a menos que o conjunto de dados acontece ser muito pequenos.

> [!TIP]
> A maioria dos clientes começam com o **livres** camada para avaliação e, em seguida, graduate para **padrão** para o desenvolvimento. Depois de escolher um escalão e [aprovisionar um serviço de pesquisa](search-create-service-portal.md), pode [aumentar o número de partição e réplica](search-capacity-planning.md) para otimização de desempenho. Para obter mais informações sobre quando e por que motivo seria ajustar capacidade, consulte [considerações de desempenho e a otimização de](search-performance-optimization.md).
>

## <a name="billing-concepts"></a>Conceitos de faturação

Conceitos que tem de compreender da seleção do escalão incluem definições de capacidade, limites de serviço e unidades de serviço. 

### <a name="capacity"></a>Capacidade

Capacidade está estruturada como *réplicas* e *partições*. As réplicas são instâncias do serviço de pesquisa, onde cada réplica aloja uma cópia de um índice com balanceamento de carga. Por exemplo, um serviço com 6 réplicas tem 6 cópias de cada índice carregado no serviço. As partições armazenar índices e automaticamente dividir dados pesquisáveis: duas partições dividir o índice do meio, três partições em thirds e assim sucessivamente. Em termos de capacidade, *tamanho de partição* é a principal funcionalidade differentiating em camadas.

> [!NOTE]
> Todos os **padrão** camadas suporte [réplica combinações flexível e partições](search-capacity-planning.md#chart) , de modo a que possa [o sistema de armazenamento ou de velocidade de importância](search-performance-optimization.md) alterando o equilíbrio. **Básico** oferece segurança de três réplicas de disponibilidade elevada, mas tem apenas uma partição. **Livre** camadas fornece os recursos dedicados: informática recursos são partilhados por vários serviços livres.

### <a name="limits"></a>Limites

Serviços de recursos de anfitrião, como índices, indexadores e assim sucessivamente. Cada camada impõe [os limites de serviço](search-limits-quotas-capacity.md) sobre a quantidade de recursos, pode criar. Como tal, um limite no número de índices (e outros objetos) é a funcionalidade differentiating segundo em camadas. Como rever cada opção no portal, tenha em atenção os limites no número de índices. Outros recursos, tais como indexadores, origens de dados e skillsets, são pegged limites de índice.

### <a name="search-units"></a>Unidades de pesquisa

O conceito de faturação mais importante para compreender um *unidade de pesquisa* (SU), que é a unidade de faturação do Azure Search. Porque a Azure Search depende das réplicas e de partições para a função, este não fazer sentido faturado por um ou outro. Em vez disso, a faturação é baseada num compostos de ambos. Formulaically, um SU é o produto de réplica e partições utilizadas por um serviço: (P X de R = SU). No mínimo, todos os serviço começa com 1 SU (uma réplica multiplicada por uma partição), mas um modelo mais realista poderá ser um serviço de réplica de 3, 3 partição-lhe cobrado como 9 SUs. 

Apesar de cada camada oferece progressivamente maior capacidade, que pode colocar uma parte da capacidade total online, que contém o resto na reserva. Em termos de faturação, é o número de partições e réplicas poder colocar online, calculada utilizando a fórmula SU, que determina o que, na verdade, de pagar.

Taxa de faturação é numa base horária por SU, com cada escalão de uma taxa diferentes. Classifica para cada camada pode ser encontrada no [detalhes dos preços](https://azure.microsoft.com/pricing/details/search/).

## <a name="consumption-patterns"></a>Padrões de consumo

A maioria dos clientes começam com o **livres** de serviço, que são manter indefinidamente e, em seguida, escolha uma do **padrão** camadas para cargas de trabalho graves desenvolvimento ou de produção. 

![Camadas de pesquisa do Azure](./media/search-sku-tier/tiers.png "escalões de preço de pesquisa do Azure")

Em cada extremidade **básico** e **S3 HD** existem padrões de consumo importantes mas atípica. **Básico** é para cargas de trabalho de produção de pequena: oferece SLA, dedicado recursos, elevada disponibilidade, mas armazenamento modest, topping no total de 2 GB. Este escalão foi projetado para os clientes que consistentemente em sobreutilizado capacidade disponível. No final distante, **S3 HD** é para cargas de trabalho típico de ISV, parceiros, [soluções multi-inquilino](search-modeling-multitenant-saas-applications.md), ou qualquer configuração para um grande número de índices pequenos. É frequentemente self-evident quando **básico** ou **S3 HD** camada é a opção adequada, mas se pretender que a confirmação de pode publicar [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) ou [contactar o Azure Suporte](https://azure.microsoft.com/support/options/) para obter orientações adicionais.

Mudarem o foco para os escalões standard mais frequentemente utilizados, **S1 S3** são uma progressão de aumentar os níveis de capacidade, com os pontos de inflection no tamanho de partição e valores máximos em números de índices, indexadores e corollary recursos:

|  | S1 | S2 | S3 |  |  |  |  |
|--|----|----|----|--|--|--|--|
| Tamanho de partição|  25 GB | 100 GB | 250 GB |  |  |  |  |
| limites de índice e indexador| 50 | 200 | 200 |  |  |  |  |

**S1** é uma escolha de comuns quando recursos dedicados e várias partições tornar-se uma necessidade. Com partições de 25 GB para até 12 partições, por-serviço limite **S1** é de 300 GB total se maximizar a partições através de réplicas (consulte [alocar partições e réplicas](search-capacity-planning.md#chart) para balanceamento mais compositions.)

As páginas do portais e preços colocar o foco sobre o tamanho de partição e o armazenamento, mas para cada camada, todas as capacidades (capacidade de disco, velocidade, CPUs) de computação crescer de forma linear com preços. Um **S2** réplica é mais rápida do que **S1**, e **S3** é mais rápida do que **S2**. **S3** camadas interromper o padrão de preços de computação geralmente linear com disproportionately mais rápido e/s. Se prevê e/s como valor, um **S3** dá-lhe muito mais de IOPS de camadas inferiores.

**S3** e **S3 HD** são apoiados por infraestrutura idênticos capacidade elevada, mas cada um atinge o limite máximo de formas diferentes. **S3** está direcionada para um número mais pequeno índices muito grande. Como tal, o limite máximo está vinculado ao recurso (2.4 TB para cada serviço). **S3 HD** destina-se um grande número de índices muito pequenos. Em 1.000 índices, **S3 HD** atingir os limites na forma de restrições de índice. Se for um **S3 HD** cliente que requer mais de 1000 índices, contacte Support da Microsoft para obter informações sobre como proceder.

> [!NOTE]
> Anteriormente, os limites do documento foram uma consideração, mas já não são aplicáveis para a maioria dos serviços de pesquisa do Azure aprovisionados após de 2018 Janeiro. Para obter mais informações sobre as condições para o qual os limites do documento ainda são aplicáveis, consulte [os limites de serviço: limites de documento](search-limits-quotas-capacity.md#document-limits).
>

## <a name="evaluate-capacity"></a>Capacidade de avaliar

Capacidade e os custos de executar o serviço aceda à mão na mão. As camadas impõe limites que esteja dois níveis (armazenamento e recursos), pelo que é necessário pensar sobre ambos porque qualquer um aceder pela primeira vez é o limite em vigor. 

Requisitos de negócio ditarem, normalmente, o número de índices, terá de. Por exemplo, um índice global para um repositório de grandes dimensões de documentos ou talvez vários índices com base na região, aplicação ou niche de negócio.

Para determinar o tamanho de um índice, tem de [compilar um](search-create-index-portal.md). A estrutura de dados na Azure Search é principalmente um [inverted índice](https://en.wikipedia.org/wiki/Inverted_index), que tem características diferentes de origem de dados. Para um índice inverted, tamanho e complexidade são determinados pelo conteúdo, não necessariamente a quantidade de feed de dados-para-lo. Uma origem de dados de grande dimensão com redundância em massa e pode resultar em índices menores que um conjunto de dados mais pequeno, que contém conteúdo altamente variável.  Como tal, é raramente possível inferir o tamanho de índice com base no tamanho do conjunto de dados original.

### <a name="preliminary-estimates-using-the-free-tier"></a>Calcula preliminar utilizando o escalão gratuito

Uma abordagem para estimar a capacidade para começar é o **livres** camada. Recuperar que o **livres** service oferece até 3 índices, 50 MB de armazenamento e de indexação de tempo de 2 minutos. Pode ser um desafio para calcular um tamanho de índice prevista destas restrições, mas o exemplo a seguir ilustra uma abordagem:

+ [Criar um serviço gratuito](search-create-service-portal.md)
+ Preparar um conjunto de dados pequeno e representativo (partem do princípio de cinco thousand documentos e o tamanho da amostra dez por cento)
+ [Criar um índice inicial](search-create-index-portal.md) e anote o respetivo tamanho no portal (partem do princípio de 30 MB)

Partindo do princípio de que amostra foi representante e dez por cento da origem de dados completo, um índice de 30 MB torna-se aproximadamente 300 MB se todos os documentos são indexados. Armed com este número preliminar, poderá duplo esse valor a budget para dois índices (programação e produção), para um total de 600 MB nos requisitos de armazenamento. Isto facilmente for satisfeito pelo **básico** camada, pelo que deverá iniciar não existe.

### <a name="advanced-estimates-using-a-billable-tier"></a>Calcula avançadas através de uma camada facturável

Alguns clientes preferem começar a utilizar recursos dedicados que podem acomodar a amostragem maior e tempos de processamento e, em seguida, desenvolva realistas calcula a quantidade de índice, tamanho e volumes de consulta durante o desenvolvimento. Inicialmente, um serviço é aprovisionado com base numa estimativa de melhor estimado e, em seguida, como o projeto de desenvolvimento evoluiu, equipas, normalmente, lembre-se o serviço existente é sobre ou com capacidade para cargas de trabalho de produção prevista. 

1. [Reveja os limites de serviço em cada camada](https://docs.microsoft.com/en-us/azure/search/search-limits-quotas-capacity#index-limits) para determinar se as camadas inferiores podem suportar a quantidade de índices é necessário. Entre o **básico**-**S1**- **S2** camadas, limites de índice são 15-50-200, respetivamente.

1. [Criar um serviço de uma camada facturável](search-create-service-portal.md):

    + Iniciar insuficiente, **básico** ou **S1** se estiver no início da curva de aprendizagem.
    + Iniciar elevado, em **S2** ou mesmo **S3**, se em grande escala de indexação e consulta são self-evident.

1. [Criar um índice inicial](search-create-index-portal.md) para determinar como traduz os dados de origem para um índice. Esta é a única forma de estimar o tamanho do índice.

1. [Monitorizar o armazenamento, limites de serviço, volume de consulta e latência](search-monitor-usage.md) no portal. O portal mostra consultas por segundo, otimizadas consultas e a latência de pesquisa; todos os que podem ajudar a decidir se estiver na camada de direita. Para além de portais métricas, pode configurar a monitorização profunda, tais como análise clickthrough, ativando [pesquisar a análise de tráfego](search-traffic-analytics.md). 

Número de índice e o tamanho são igualmente relevantes para a análise porque os limites máximos são atingidos através da utilização total de armazenamento (partições) ou os limites máximos nos recursos (índices, indexadores e assim consecutivamente), o que ocorrer primeiro. O portal de ajuda a manter controlar dos ambos, que mostra a utilização atual e os limites máximos lado a lado na página de descrição geral.

> [!NOTE]
> Requisitos de armazenamento podem ser excessiva inflated se documentos contêm dados supérfluas. Idealmente, os documentos contêm apenas os dados que necessita para a experiência de pesquisa. Dados binários é não pesquisáveis e devem ser armazenados em separado (talvez no armazenamento do Azure de tabela ou blob) com um campo no índice para conter uma referência de URL para os dados externos. O tamanho máximo de um documento individuais é 16 MB (ou menos se estiver em massa carregar vários documentos num pedido). [Os limites na Azure Search de serviço](search-limits-quotas-capacity.md) tem mais informações.
>

**Considerações de volume de consulta**

As consultas por segundo (QPS) é uma métrica de obtiver prominence durante a otimização de desempenho, mas normalmente, não é uma consideração de camada, exceto se esperar volume muito elevado de consulta em proposto.

Todas as camadas standard podem fornecer um equilíbrio de réplicas para partições, que suporta uma resposta mais rápida consulta através de réplicas adicionais para carregar as partições de balanceamento e adicionais para processamento paralelo. Pode otimizar para desempenho depois do serviço está aprovisionado.

Cliente que esperava strong constante consulta volumes proposto devem considerar camadas superiores, associadas a mais poderosa hardware. Pode, em seguida, coloque partições e réplicas offline, ou até mesmo se muda para um serviço de camada inferior, esses volumes de consulta não conseguem materializar. Para obter mais informações sobre como calcular o débito de consulta, consulte [desempenho de pesquisa do Azure e a otimização de](search-performance-optimization.md).


**Contratos de nível de serviço**

O **livres** camada e pré-visualização de funcionalidades não são fornecidos com [(SLAs) de contratos de nível de serviço](https://azure.microsoft.com/support/legal/sla/search/v1_0/). Para todos os escalões faturáveis, SLAs entram em vigor quando aprovisionar redundância suficiente para o seu serviço. Dois ou mais réplicas são necessárias para o SLA de consulta (leitura). Três ou mais réplicas são necessárias para consulta e indexação SLA (leitura / escrita). O número de partições não é uma consideração de SLA. 

## <a name="tips-for-tier-evaluation"></a>Sugestões para a avaliação da camada

+ Saiba como criar índices eficiente e as metodologias de atualização são o impactful menos. Recomendamos [pesquisar a análise de tráfego](search-traffic-analytics.md) para as informações obtidas na atividade de consulta.

+ Permitir que as métricas compilar em torno de consultas e recolher dados em torno de padrões de utilização (consultas durante o horário comercial, indexação horas de ponta) e utilizar estes dados para informar o serviço futuro decisões de aprovisionamento. Enquanto não práticas ao nível de uma horário ou diário, pode ajustar dinamicamente partições e de recursos para acomodar planeadas alterações em volumes de consulta, ou não planeada, mas as alterações constante se níveis reter suficientemente longo para garantir a colocar a ação.

+ Lembre-se de que o downside apenas de aprovisionamento em é que poderá ter de fechar as um serviço se os requisitos reais são maiores do que o previsto. Para evitar a interrupção do serviço, tem de criar um novo serviço na mesma subscrição num escalão superior e execute-lado a lado até que todos os pedidos de aplicações e o novo ponto final de destino.

## <a name="next-steps"></a>Passos Seguintes

Começar com uma **livres** camada e criar um índice inicial a utilizar um subconjunto dos seus dados para compreender as características. A estrutura de dados na pesquisa do Azure é um índice inverted, em que o tamanho e complexidade de um índice inverted é determinada pelo conteúdo. Lembre-se de que o conteúdo altamente redundante tende a resultar num índice que conteúdo altamente dados mais pequeno. Como tal, é características conteúdas em vez do tamanho do conjunto de dados que determina os requisitos de armazenamento de índice.

Depois de ter uma ideia inicial do tamanho do índice, [aprovisionar um serviço sujeito a faturação](search-create-service-portal.md) em um dos escalões abordados neste artigo, quer **básico** ou um **padrão** camada. Reduzir as eventuais restrições artificial em subconjuntos de dados e [reconstrua o índice](search-howto-reindex.md) para incluir todos os dados que pretende, na verdade, de ser pesquisáveis.

[Alocar partições e réplicas](search-capacity-planning.md) conforme necessário para obter o desempenho e dimensionamento necessita.

Se desempenho e a capacidade, terminar. Caso contrário, volte a criar um serviço de pesquisa na camada de diferentes mais detalhadamente alinhar com as suas necessidades.

> [!NOTE]
> Para obter mais ajuda com as suas perguntas, publicar [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) ou [contacte o suporte do Azure](https://azure.microsoft.com/support/options/).