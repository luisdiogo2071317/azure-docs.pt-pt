---
title: Níveis de consistência no Azure Cosmos DB | Documentos da Microsoft
description: Azure Cosmos DB tem cinco níveis de consistência para o ajudar a equilibrar eventual consistência, disponibilidade e latência vantagens e desvantagens.
keywords: a consistência eventual, do azure cosmos db, do azure, do Microsoft azure
services: cosmos-db
author: aliuy
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/27/2018
ms.author: andrl
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8d95790dc09f6d26c6ae749ed0cd386053c5cb35
ms.sourcegitcommit: 7b845d3b9a5a4487d5df89906cc5d5bbdb0507c8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/14/2018
ms.locfileid: "42060231"
---
# <a name="tunable-data-consistency-levels-in-azure-cosmos-db"></a>Níveis de consistência sincronizáveis dados no Azure Cosmos DB
O Azure Cosmos DB foi concebido desde o backup com a distribuição global, lembre-se para cada modelo de dados. Foi concebido para fornecer garantias de latência baixa previsível e vários modelos de consistência bem definidos de flexível. Atualmente, o Azure Cosmos DB oferece cinco níveis de consistência: sólido, prescrição vinculada, sessão, prefixo de consistência e eventual. Prescrição vinculada, sessão, prefixo consistente e eventual são referidas como "modelos de consistência relaxados" que fornecem menos consistência forte, que o que é o modelo a maioria dos consistente altamente disponível. 

Além do **forte** e **consistência eventual** modelos oferecidos normalmente pelas bases de dados distribuídas, Azure Cosmos DB oferece três modelos de consistência cuidadosamente alcançarmos e operacionalizado mais:  **estagnação limitada**, **sessão**, e **prefixo consistente**. A utilidade de cada um destes níveis de consistência foi validada em relação a casos de utilização do mundo real. Coletivamente, estes níveis de cinco consistência permitem-lhe fazer compensações bem manter entre consistência, disponibilidade e latência. 

O vídeo seguinte, Andrew Liu gerente de programa do DB Cosmos Azure demonstra os recursos de distribuição global chave na mão.

>[!VIDEO https://www.youtube.com/embed/-4FsGysVD14]

## <a name="distributed-databases-and-consistency"></a>Bases de dados distribuídas e consistência
Bases de dados comerciais distribuídas enquadram-se em duas categorias: bases de dados que não oferecem escolhas de consistência prováveis bem definidos de todo e bases de dados que oferecem duas opções de programação extremas (consistência fortes versus eventual). 

A primeira opção sobrecarrega os programadores de aplicações com a minúcia dos seus protocolos de replicação e exige que haja compromissos difíceis entre consistência, disponibilidade, latência e débito. A segunda opção obriga a que se tenha de escolher entre dois extremos. Não obstante a abundância de pesquisa e propostas para mais de 50 modelos de consistência, a comunidade da base de dados distribuída não conseguiu comercializar níveis de consistência para além da consistência forte e da consistência eventual. O cosmos DB permite aos desenvolvedores escolher entre cinco modelos de consistência bem definidos ao longo do espetro de consistência – forte, estagnação limitada, [sessão](http://dl.acm.org/citation.cfm?id=383631), prefixo consistente e eventual. 

![O Azure Cosmos DB proporciona vários modelos de consistência (flexíveis) bem definidos à escolha](./media/consistency-levels/five-consistency-levels.png)

A tabela seguinte ilustra as garantias específicas proporcionadas por cada nível de consistência.
 
**Níveis de Consistência e garantias**

| Nível de Consistência | Garantias |
| --- | --- |
| Forte | Transação atómica. As leituras são garantidas para devolver a versão mais recente de um item.|
| Estagnação Limitada | Prefixo Consistente. Desfasamento de leituras escritas por no máximo os prefixos k ou intervalo t |
| Sessão   | Prefixo Consistente. Leituras monotónicas, escritas monotónicas, leitura das próprias escritas, escrita de acordo com leituras |
| Prefixo Consistente | As atualizações devolvidas são alguns prefixos de todas as atualizações, sem intervalos |
| Eventual  | Leituras fora de ordem |

Pode configurar o nível predefinido de consistência na sua conta do Cosmos DB (e, mais tarde, substituir a consistência num pedido de leitura específico). Internamente, o nível predefinido de consistência aplica-se aos dados dentro dos conjuntos de partição, que podem distribuir as regiões. Cerca de 73% de consistência de sessão de utilização de inquilinos do Azure Cosmos DB e 20% preferem estagnação limitada. Aproximadamente 3% dos clientes do Azure Cosmos DB experimentar vários níveis de consistência inicialmente antes de optar por uma escolha específica de consistência da respetiva aplicação. Apenas 2% de inquilinos do Azure Cosmos DB substituir os níveis de consistência numa base por solicitação. 

No Cosmos DB, fornecidas leituras em sessão, prefixo consistente e a consistência eventual duas vezes são tão barato como leituras com consistência forte ou estagnação limitada. O cosmos DB tem SLAs abrangentes, incluindo garantias de consistência, juntamente com a disponibilidade, débito e latência de líder da indústria. O Azure Cosmos DB emprega um [Verificador de transação atómica](http://dl.acm.org/citation.cfm?id=1806634), que opera continuamente sobre a telemetria de serviço e abertamente reporta quaisquer violações de consistência para. Para estagnação limitada, o Azure Cosmos DB monitoriza e relata quaisquer violações de limites k e t. Para todos os cinco níveis de consistência flexível, Azure Cosmos DB também relata o [roleta estagnação métrica de envelhecimento](http://dl.acm.org/citation.cfm?id=2212359) diretamente para.  

## <a name="service-level-agreements"></a>Contratos de nível de serviço

O Azure Cosmos DB oferece abrangente 99,99% [SLAs](https://azure.microsoft.com/support/legal/sla/cosmos-db/) quais garantia de débito, consistência, disponibilidade e latência para o Azure Cosmos DB da base de dados no âmbito de uma única região do Azure configurado com qualquer uma da consistência de cinco de contas níveis ou contas de base de dados em diversas regiões do Azure configurados com qualquer um dos quatro níveis de consistência flexível. Além disso, é independente da escolha de um nível de consistência, Azure Cosmos DB oferece um SLA de 99,999% de disponibilidade de leitura para contas de base de dados em duas ou mais regiões do Azure.

## <a name="scope-of-consistency"></a>Âmbito de consistência
A granularidade de consistência é confinada para um pedido de utilizador único. Uma solicitação de gravação pode corresponder a inserir, substituir, upsert, ou eliminar a transação. Tal como acontece com escritas, também tem um âmbito uma transação de leitura/consulta a um pedido de utilizador único. O utilizador poderá ser necessário para paginar através de um grande conjunto de resultados, que abrangem várias partições, mas cada transação está no âmbito de uma única página e servida a partir de uma única partição de leitura.

## <a name="consistency-levels"></a>Níveis de consistência
Pode configurar um nível predefinido de consistência na sua conta de base de dados que se aplica a todos os contentores (e as bases de dados) na sua conta do Cosmos DB. Por predefinição, todas as leituras e consultas emitidas relativamente aos recursos definidos pelo utilizador utilizam o nível de consistência predefinido, especificado na conta de base de dados. Pode reduzir o nível de consistência de um pedido de consulta/leitura específica através em cada uma das APIs suportadas. Existem cinco tipos de níveis de consistência suportadas pelo protocolo de replicação do Azure Cosmos DB, que fornecem uma compensação clara entre garantias de consistência específico e o desempenho, conforme descrito nesta secção.

<a id="strong"></a>
**Forte**: 

* Consistência forte oferece uma [transação atómica](https://aphyr.com/posts/313-strong-consistency-models) garantir com leituras garantidas para devolver a versão mais recente de um item. 
* Consistência forte garante que uma gravação só é visível depois submetê-los maneira duradoura pela maioria do quórum de réplicas. Uma gravação é síncrona confirmada maneira duradoura pelo principal e o quórum de réplicas secundárias, ou foi abortada. Uma leitura é sempre confirmada pela maioria de leitura do quórum, um cliente nunca pode ver uma gravação não consolidada ou parcial e é sempre garantido ao ler a escrita mais recente reconhecida. 
* As contas do Azure Cosmos DB que estão configuradas para utilizar a consistência forte não é possível associar mais do que uma região do Azure com a respetiva conta do Azure Cosmos DB.  
* O custo de uma operação de leitura (em termos de [unidades de pedido](request-units.md) consumido) com consistência forte é maior do que a sessão e eventual, mas o mesmo que estagnação limitada.

<a id="bounded-staleness"></a>
**Estagnação limitada**: 

* Estagnação limitada garantias de consistência que as leituras podem ficar atrás de escritas por no máximo *K* versões ou prefixos de um item ou *t* intervalo de tempo. 
* Por conseguinte, quando escolher estagnação limitada, o "envelhecimento" pode ser configurado de duas formas: o número de versões *K* do item pelo qual as desfasamento de leituras de gravações e o intervalo de tempo *t* 
* Estagnação limitada ofertas total global do pedido, exceto dentro da "janela limitada". As garantias de leitura monotónica existem dentro de uma região dentro e fora da "limitada janela." 
* Estagnação limitada proporciona uma maior garantia de consistência de sessão, prefixo consistente ou eventual consistência. Para aplicações distribuídas globalmente, recomendamos que utilize estagnação limitada para cenários em que gostaria de ter consistência forte, mas também querem 99,99% de disponibilidade e baixa latência.   
* As contas do Azure Cosmos DB que estão configuradas com consistência de estagnação limitada podem associar qualquer número de regiões do Azure com a respetiva conta do Azure Cosmos DB. 
* O custo de uma operação de leitura (em termos de RUs consumidos) com estagnação limitada é maior do que a sessão e a consistência eventual, mas o mesmo que a consistência forte.

<a id="session"></a>
**Sessão**: 

* Ao contrário dos modelos de consistência global oferecidos pelos níveis de consistência forte e estagnação limitada, a consistência da sessão é confinada a uma sessão de cliente. 
* É ideal para todos os cenários em que uma sessão de dispositivo ou utilizador está envolvida, uma vez que esta ação garante leituras monotónicas, escritas monotónica e leia suas próprias escritas (RYW) garante a consistência da sessão. 
* Consistência de sessão fornece consistência previsível para uma sessão, e o máximo débito de leitura ao ofertas de leituras e escritas de latência mais baixa. 
* As contas do Azure Cosmos DB que estão configuradas com consistência da sessão podem associar qualquer número de regiões do Azure com a respetiva conta do Azure Cosmos DB. 
* O custo de uma operação de leitura (em termos de RUs consumidos) com o nível de consistência da sessão é inferior a forte e estagnação limitada, mas a consistência eventual mais do que.

<a id="consistent-prefix"></a>
**Prefixo consistente**: 

* Prefixo consistente garante que na ausência de qualquer escrita adicional, as réplicas dentro do grupo, eventualmente, convergem. 
* Prefixo consistente garante que leituras nunca veem escritas fora de ordem. Se escritas foram executadas na ordem `A, B, C`, em seguida, um cliente vê um `A`, `A,B`, ou `A,B,C`, mas nunca fora de ordem, como `A,C` ou `B,A,C`.
* As contas do Azure Cosmos DB que estão configuradas com consistência de prefixo consistente podem associar qualquer número de regiões do Azure com a respetiva conta do Azure Cosmos DB. 

<a id="eventual"></a>
**Eventual**: 

* A consistência eventual garante que na ausência de qualquer escrita adicional, as réplicas dentro do grupo, eventualmente, convergem. 
* A consistência eventual é a forma mais fraco de consistência em que um cliente pode obter os valores que são mais antigos do que os que ele tinha visto antes.
* A consistência eventual fornece a consistência de leitura mais fraco, mas oferece a menor latência de leitura e escrita.
* As contas do Azure Cosmos DB que estão configuradas com a consistência eventual podem associar qualquer número de regiões do Azure com a respetiva conta do Azure Cosmos DB. 
* O custo de uma operação de leitura (em termos de RUs consumidos) com a consistência eventual nível é a mais baixa de todos os níveis de consistência do Azure Cosmos DB.

## <a name="configuring-the-default-consistency-level"></a>Configurar o nível de consistência predefinido
1. Na [portal do Azure](https://portal.azure.com/), na barra de atalhos, clique em **do Azure Cosmos DB**.
2. Na página **Azure Cosmos DB**, selecione a conta de base de dados a modificar.
3. Na página da conta, clique em **consistência predefinida**.
4. Na **consistência predefinida** página, selecione o novo nível de consistência e clique em **guardar**.
   
    ![Captura de ecrã, realce o ícone de definições e a entrada de consistência predefinida](./media/consistency-levels/database-consistency-level-1.png)

## <a name="consistency-levels-for-queries"></a>Níveis de consistência para consultas
Por predefinição, para os recursos definidos pelo utilizador, o nível de consistência para consultas é o mesmo que o nível de consistência para leituras. Por predefinição, o índice é atualizado forma síncrona em cada inserir, substituir ou eliminação de um item no contentor do Cosmos DB. Isto permite que as consultas que respeite o mesmo nível de consistência do ponto de leituras. Enquanto o Azure Cosmos DB está escrita otimizada e suporta volumes constante de escritas, a manutenção do índice síncrona e a publicação consultas consistentes, pode configurar certos contêineres para atualizar o respetivo índice lentamente. Ainda mais lento de indexação aumenta o desempenho de escrita e é ideal para cenários de ingestão em massa, quando uma carga de trabalho é principalmente de leitura intensiva.  

| Modo de indexação | Leituras | Consultas |
| --- | --- | --- |
| Consistência (predefinição) |Selecione entre forte, estagnação limitada, sessão, prefixo consistente ou eventual |Selecione entre forte, estagnação limitada, sessão, ou eventual |
| Lento |Selecione entre forte, estagnação limitada, sessão, prefixo consistente ou eventual |Eventual |
| Nenhuma |Selecione entre forte, estagnação limitada, sessão, prefixo consistente ou eventual |Não aplicável |

Como com solicitações de leitura, pode reduzir o nível de consistência de um pedido de consulta específica em cada API.

## <a name="consistency-levels-for-the-mongodb-api"></a>Níveis de consistência para a API do MongoDB

O Azure Cosmos DB implementa atualmente MongoDB versão 3.4, que tem duas definições de consistência, eventual e fortes. Uma vez que o Azure Cosmos DB possui várias API, as definições de consistência são aplicáveis ao nível da conta e a imposição da consistência é controlada por cada API.  Até à versão 3.6 do MongoDB, não existia nenhum conceito de consistência de sessão, pelo que, se definir uma conta de API do MongoDB para utilizar a consistência de sessão, esta é mudada para uma versão anterior, para eventual, quando utilizar as API do MongoDB. Se precisar de uma garantia de leitura-da-própria-escrita para uma conta de API do MongoDB, o nível de consistência predefinido da conta deve ser definido como forte ou de estagnação limitada.

## <a name="next-steps"></a>Passos Seguintes
Se deseja ler mais sobre os níveis de consistência e os compromissos, recomendamos os seguintes recursos:

* [Consistência de dados replicados explicada através do basebol (vídeo) por Doug Tiago](https://www.youtube.com/watch?v=gluIh8zd26I)
* [Consistência de dados replicados explicada através do basebol (documento técnico), por Doug Tiago](http://research.microsoft.com/pubs/157411/ConsistencyAndBaseballReport.pdf)
* [Garantias de sessão para os dados replicados consistentes tem rigidez](http://dl.acm.org/citation.cfm?id=383631)
* [Vantagens e desvantagens de consistência no moderno Design de sistemas de banco de dados distribuído: limite é apenas uma parte da história](https://www.computer.org/web/csdl/index/-/csdl/mags/co/2012/02/mco2012020037-abs.html)
* [Probabilístico estagnação limitada (PBS) para práticos Quorums parciais](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
* [Eventual consistentes – Revisitada](http://allthingsdistributed.com/2008/12/eventually_consistent.html)
* [A carga, a capacidade e a disponibilidade dos sistemas de quórum, diário SIAM sobre computação](http://epubs.siam.org/doi/abs/10.1137/S0097539795281232)
* [Line-up: um transação atómica de automática e completa verificador, judiciais da conference 2010 ACM SIGPLAN na implementação e design de linguagem de programação](http://dl.acm.org/citation.cfm?id=1806634)
* [Roleta estagnação limitada para práticos quorums parciais](http://dl.acm.org/citation.cfm?id=2212359)
