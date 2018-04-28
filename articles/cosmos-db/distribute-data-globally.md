---
title: Distribuir dados globalmente com a base de dados do Azure Cosmos | Microsoft Docs
description: Saiba mais sobre a recuperação de dados, ativação pós-falha e a georreplicação de planet escala bases de dados global do Azure Cosmos DB, um serviço de base de dados globalmente distribuídas, o modelo de estiverem a utilizar.
services: cosmos-db
documentationcenter: ''
author: SnehaGunda
manager: kfile
ms.assetid: ba5ad0cc-aa1f-4f40-aee9-3364af070725
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2018
ms.author: sngun
ms.openlocfilehash: 967e7458d43dccd4601440138b7445eb876b9f01
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
---
# <a name="how-to-distribute-data-globally-with-azure-cosmos-db"></a>Como distribuir dados globalmente com o Azure Cosmos DB
Azure ubíquo - tem um requisitos de espaço global em 50 + regiões geográficas e continuamente está a expandir. Com a presença global, uma das capacidades de diferenciadas que Azure oferece aos respetivos programadores é a capacidade de criar, implementar e gerir facilmente aplicações distribuídas global. 

O [Azure Cosmos DB](../cosmos-db/introduction.md) é um serviço de bases de dados com vários modelos e distribuído globalmente da Microsoft para aplicações críticas para atividades. BD do Azure do Cosmos fornece chave na mão distribuição global, [dimensionamento elástico de débito e armazenamento](../cosmos-db/partition-data.md) latências milissegundo em todo o mundo, dígito em percentil 99th, [cinco modelos de consistência bem definidos](consistency-levels.md)e garantida elevada disponibilidade, todas as cópia por [SLAs abrangentes líder da indústria](https://azure.microsoft.com/support/legal/sla/cosmos-db/). BD do Azure do Cosmos [indexa automaticamente todos os seus dados](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) sem necessidade de lidar com a gestão de esquema ou o índice. Este é um serviço com múltiplos modelo e suporta o documento, chave-valor, gráfico e modelos de dados de família de colunas. Como um nativamente born no serviço em nuvem, base de dados do Azure Cosmos é cuidadosamente foi desenvolvido com distribuição de vários inquilinos e global a partir do zero cópias de segurança.


![Coleção do Azure do Cosmos DB particionado e distribuídos três regiões](./media/distribute-data-globally/global-apps.png)

**Um único contentor de base de dados do Azure Cosmos particionado e distribuídos por várias regiões do Azure**

Como podemos ter aprendidas durante a criação da base de dados do Azure Cosmos, distribuição global adição não pode ser posteriori. Não pode ser "bolted-ativado" visível um sistema de base de dados "único site". As capacidades oferecidas por uma base de dados globalmente distribuída span se para além dos que de desastre geográfica tradicional recuperação (Georreplicação DR) oferecidas pelas bases de dados "site único". Bases de dados de site único oferece a capacidade de DR Georreplicação são um subconjunto restrito de bases de dados globalmente distribuídas. 

Com distribuição global chave na mão da BD do Cosmos do Azure, os programadores não tem de criar os seus próprios andaime de replicação ao empregar uma o padrão de Lambda (por exemplo, [AWS DynamoDB replicação](https://github.com/awslabs/dynamodb-cross-region-library/blob/master/README.md)) através do registo da base de dados ou pelo efetuar "escritas duplas" em várias regiões. Fazemos *não* Recomendamos estas abordagens, uma vez que é impossível Certifique-se a correção dessas abordagens e fornecer os SLAs de som. 

Neste artigo, podemos fornecer uma descrição geral das capacidades de distribuição global da BD do Cosmos do Azure. Podemos também descrevem a abordagem de exclusiva da BD do Cosmos do Azure para fornecer SLAs abrangentes. 

## <a id="EnableGlobalDistribution"></a>Ativar distribuição global chave na mão
BD do Azure do Cosmos fornece as seguintes funcionalidades para ativar a escrever facilmente aplicações distribuídas global. Estas capacidades estão disponíveis através do recurso do Cosmos BD do Azure baseados em fornecedores [REST APIs](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/) , bem como o portal do Azure.

Veja o vídeo seguinte para ver a funcionalidade de chave na mão distribuição global do BD Azure Cosmos em ação.

> [!VIDEO https://www.youtube.com/embed/1D06yjTVxt8]
>

### <a id="RegionalPresence"></a>Presença regional ubíqua 
Azure está constantemente a crescer a presença geográfica colocando [regiões novo](https://azure.microsoft.com/regions/) online. BD do Azure do Cosmos é classificado como um *serviço fundamentais sobre* no Azure e está disponível em todas as regiões do Azure nova por predefinição. Isto permite-lhe associar uma região geográfica com a sua conta de base de dados de base de dados do Azure Cosmos assim que a Azure abre-se a região novo para empresas.

### <a id="UnlimitedRegionsPerAccount"></a>Associar um número ilimitado de regiões com a sua conta de base de dados de base de dados do Azure Cosmos
BD do Cosmos do Azure permite-lhe associar qualquer número de regiões do Azure com a sua conta de base de dados de base de dados do Azure Cosmos. Fora geométrico restrições (por exemplo, China, Datacenters), existem não existem limitações no número de regiões que podem ser associados a sua conta de base de dados de base de dados do Azure Cosmos. A figura seguinte mostra uma conta de base de dados configurada para span em 25 regiões do Azure.  

![Conta de base de dados do Azure do Cosmos DB expansão 25 regiões do Azure](./media/distribute-data-globally/spanning-regions.png)

**Conta de expansão 25 regiões do Azure da base de dados do Azure Cosmos DB um inquilino**


### <a id="PolicyBasedGeoFencing"></a>Baseado na política barreiras geográficas
BD do Azure do Cosmos foi concebido para suportar baseadas em políticas barreiras geográficas. Geométrico é um componente importante para garantir que as restrições de governação e conformidade de dados e pode impedir a associação de uma região específica com a sua conta. Exemplos de geométrico incluir (mas não estarem limitados aos) distribuição global para as regiões dentro de uma nuvem sovereign (por exemplo, China e na Alemanha) ou numa zona de taxation government (por exemplo, da Austrália) de controlo de âmbito. As políticas são controladas através de metadados da sua subscrição do Azure.

### <a id="DynamicallyAddRegions"></a>Adicionar e remover regiões dinamicamente
BD do Cosmos do Azure permite-lhe adicionar (associar) ou remover (desassociar) regiões da sua conta de base de dados em qualquer altura (consulte [figura anterior](#UnlimitedRegionsPerAccount)). Em virtude da replicação paralela dos dados nos partições, a base de dados do Azure Cosmos garante que quando é adicionada uma nova região, está disponível para operações de 30 minutos em qualquer local no mundo (pressupondo que os seus dados é 100 TBs ou menos). 

### <a id="FailoverPriorities"></a>Prioridades de ativação pós-falha
Para controlar a sequência exata de ativações pós-falha regional em casos de indisponibilidade, o Azure Cosmos DB permite-lhe associar um *prioridade* com várias regiões associadas com a base de dados de conta (consulte a figura abaixo). BD do Azure do Cosmos garante que a sequência de ativação pós-falha automática ocorre a ordem de prioridade que especificou. Para obter mais informações sobre as ativações pós-falha regional, consulte [automáticas ativações pós-falha regional para continuidade do negócio do BD Azure Cosmos](regional-failover.md).


![Configuração de ativação pós-falha de prioridades, com base de dados do Azure Cosmos](./media/distribute-data-globally/failover-priorities.png)

**Um inquilino do Azure Cosmos DB pode configurar a ordem de prioridade de ativação pós-falha (painel direito) para regiões associadas com uma conta de base de dados**

### <a id="ConsistencyLevels"></a>Vários modelos de consistência bem definidos para as bases de dados globalmente distribuídas
BD do Azure do Cosmos suporta [vários modelos de consistência bem definidos, intuitiva e práticas](consistency-levels.md) SLAs de segurança. Pode escolher um modelo de consistência específico (da lista de opções disponíveis) consoante os carga de trabalho/cenários. 

### <a id="TunableConsistency"></a>Consistência ajustáveis para bases de dados globalmente replicadas
BD do Cosmos do Azure permite-lhe substituir e reduzir a escolha de consistência predefinida numa base por pedido durante a execução programaticamente. 

### <a id="DynamicallyConfigurableReadWriteRegions"></a>Leitura dinamicamente configurável e regiões de escrita
BD do Cosmos do Azure permite-lhe configurar as regiões (associada à base de dados) para "leitura", "escrever" ou "leitura/escrita" regiões. 

### <a id="ElasticallyScaleThroughput"></a>Débito aprovisionadas dimensionamento em regiões do Azure
Pode dimensionar um contentor de base de dados do Azure Cosmos pelo débito aprovisionamento através de programação. O débito é aplicado a todas as regiões que o contentor de base de dados do Azure Cosmos é distribuído no.

### <a id="GeoLocalReadsAndWrites"></a>Georreplicação local lê e escreve
A principal vantagem de uma base de dados globalmente distribuída é que oferece acesso de latência baixa aos dados em qualquer lugar do mundo. BD do Azure do Cosmos oferece leituras de latência baixa e escreve no percentil 99th em todo o mundo. Assegura que todas as leituras são servidas a partir da região mais próxima (local). Para efetuar um pedido de leitura, é utilizado o quórum local para a região em que é emitida a leitura. O mesmo se aplica a escritas. Uma operação de escrita é confirmada apenas depois de a maioria das réplicas de forma durável ter consolidada da operação de escrita localmente, mas sem ser gated em réplicas remotas para confirmar as escritas. Para colocá-la de forma diferente, o protocolo de replicação de base de dados do Azure Cosmos funciona no pressuposto de que a leitura e escrita quorums são sempre locais para a região em que o pedido foi emitido.

### <a id="ManualFailover"></a>Ativação pós-falha manual
BD do Cosmos do Azure permite-lhe acionar a ativação pós-falha de uma conta de base de dados para validar o *ponto a ponto* propriedades de disponibilidade da aplicação completa (para além da base de dados). Uma vez que a segurança e as propriedades de liveness de eleição falha de deteção e leader garantidas, garante que a base de dados do Azure Cosmos *perda de dados de zero* para uma operação de ativação pós-falha manual iniciadas por inquilino.

### <a id="AutomaticFailover"></a>Ativação pós-falha automática
BD do Cosmos do Azure suporta a ativação pós-falha automática no caso de um ou mais falhas regionais. Durante uma ativação pós-falha regional, base de dados do Azure Cosmos mantém a leitura, disponibilidade de tempo de atividade, consistência, débito e latência SLAs. BD do Cosmos do Azure fornece um limite superior para a duração de uma operação de ativação pós-falha automática para concluir. Esta é a janela de potencial perda de dados durante uma falha regional.

### <a id="GranularFailover"></a>Concebido para granularidades diferentes de ativação pós-falha
Atualmente, as capacidades de ativação pós-falha automática e manual estão expostas a granularidade da conta de base de dados. Tenha em atenção, internamente BD do Azure Cosmos foi concebida para oferecer *automática* ativação pós-falha granularidade melhorar de uma base de dados, um contentor ou mesmo uma partição (de um contentor proprietária de um intervalo de chaves). 

### <a id="MultiHomingAPIs"></a>Multi homing no Azure Cosmos DB
BD do Cosmos do Azure permite-lhe interagir com uma base de dados utilizando o *lógica* (região agnóstico) ou *físico* pontos finais de (específico da região). Utilizar pontos finais lógicos garante que a aplicação de forma transparente pode ser multihomed em caso de uma ativação pós-falha. A última opção, um ponto final físico, fornece um controlo detalhado para a aplicação para redirecionar leituras e escritas para regiões específicas.

Pode encontrar informações sobre como configurar as preferências de leitura para o [API do SQL Server](../cosmos-db/tutorial-global-distribution-sql-api.md), [Gremlin API](../cosmos-db/tutorial-global-distribution-graph.md), [API de tabela](../cosmos-db/tutorial-global-distribution-table.md), e [MongoDB API](../cosmos-db/tutorial-global-distribution-mongodb.md) no Estes artigos.

### <a id="TransparentSchemaMigration"></a>Migração de esquema e o índice da base de dados transparente e consistente 
BD do Azure do Cosmos é totalmente [desconhecidas do esquema](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf). A estrutura exclusiva do motor de base de dados permite a BD do Cosmos Azure automaticamente e índice todos os dados após a inserção em sincronia sem necessidade de qualquer esquema ou índices secundários do utilizador. Isto permite-lhe iterar a aplicação distribuída globalmente rapidamente sem ter de se preocupar sobre a migração de esquema e o índice da base de dados ou coordenar a implementação de aplicação de fase multi de alterações ao esquema. BD do Azure do Cosmos garante que as alterações efetuadas nas políticas que tiver realizadas explicitamente a indexação não resultar numa degradação do desempenho ou disponibilidade.  

### <a id="ComprehensiveSLAs"></a>SLAs abrangentes (para além das apenas elevada disponibilidade)
Como um serviço de base de dados globalmente distribuída, base de dados do Azure Cosmos oferece SLAs bem definidos e abrangentes para **disponibilidade**, **latência**, **débito** e **consistência** para a base de dados em execução à escala global, independentemente do número de regiões associada com a base de dados.  

## <a id="LatencyGuarantees"></a>Garantias de latência
A principal vantagem de um serviço de base de dados globalmente distribuída como base de dados do Azure Cosmos é a oferta de acesso de latência baixa aos seus dados em qualquer lugar do mundo. BD do Azure do Cosmos oferece garantida baixa latência, o percentil 99th para várias operações de base de dados. O protocolo de replicação que utiliza a base de dados do Azure Cosmos assegura que as operações de base de dados (leituras e escritas) são sempre executadas na região local para que o cliente. A latência de BD do Cosmos SLA do Azure fornece garantias no percentil 99th para leituras, escritas (forma síncrona) indexadas e consultas para vários tamanhos de pedido e resposta. As garantias de latência para escritas incluem consolidações de quórum maioria duráveis numa região local.

### <a id="LatencyAndConsistency"></a>Relação da latência com consistência 
Para um serviço globalmente distribuído oferecer a consistência forte para uma configuração global distribuída, necessita de forma síncrona replicar as escritas ou para executar em sincronia de leituras por várias regiões. A velocidade do leve e de área alargada rede fiabilidade da sua organização ditarem que a consistência forte irá resultar no latências e disponibilidade reduzida de operações de base de dados. Por conseguinte, para oferecer garantida baixas latências no percentil 99th e 99,99% de disponibilidade para todas as contas de única região e todas as contas de multirregião com consistência simples e 99.999% de disponibilidade em todas as contas de base de dados de multirregião, o serviço tem de utilizar replicação assíncrona. Esta por sua vez requer que o serviço têm também oferecem [model(s) de consistência bem definidos, simples](consistency-levels.md) – mais fraco que seguro (para oferecer garantias de disponibilidade e de latência baixas) e Idealmente mais forte do que "eventual" consistência (com uma intuitivo modelo de programação).

BD do Azure do Cosmos garante que uma operação de leitura não é necessário contactar réplicas em várias regiões para fornecer uma garantia de nível de consistência específico. Da mesma forma, garante que uma operação de escrita não obter bloqueada enquanto os dados que está a ser replicados em todas as regiões (ou seja, escritas no modo assíncrono replicadas em regiões). Para contas de base de dados de multirregião seguras, bem como de vários níveis de consistência simples ambos estão disponíveis. 

### <a id="LatencyAndAvailability"></a>Relação da latência com disponibilidade 
Disponibilidade e latência são dois lados de coin o mesmo. Iremos falar sobre latência de uma operação na disponibilidade na presença de falhas e de partições de rede e estado de repouso. A partir do ponto de vista da aplicação uma operação de base de dados em execução lenta é indistinguishable da base de dados que não está disponível. 

Para distinguir latência elevada de indisponibilidade, base de dados do Azure Cosmos fornece um limite superior absoluto para a latência das várias operações de base de dados. Se a operação de base de dados demora mais que o limite superior para concluir, a base de dados do Azure Cosmos devolve um erro de tempo limite. O SLA de disponibilidade de base de dados do Azure Cosmos garante que os tempos limite são contados contra o SLA de disponibilidade. 

### <a id="LatencyAndThroughput"></a>Relação da latência com débito
BD do Azure do Cosmos não fazer com que escolher entre a latência e débito. Este honra o SLA para ambos os latência no percentil 99th e oferece o débito que aprovisionou. 

## <a id="ConsistencyGuarantees"></a>Garantias de consistência
Enquanto o [modelo consistência forte](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) é o padrão de ouro de programação para de dados, é fornecido um preço muito acentuada de latência superior (em estado de repouso) e reduzido disponibilidade (face a falhas). 

BD do Azure do Cosmos oferece um modelo de programação bem definido por si para razão sobre consistência de dados replicados. Para ativar facilmente criar aplicações distribuídas globalmente com capacidade multi homing, os modelos de consistência expostos pela base de dados do Azure Cosmos foram concebidos para serem região agnóstico e independentes da região de onde estão a ser o leituras e escritas servido. 

Azure DB de Cosmos consistência SLA garante que 100% de pedidos de leitura irá satisfazer a garantia de consistência para o modelo de consistência especificado por si (à conta de base de dados ou o nível de pedido). Um pedido de leitura é considerado atingiu a consistência de SLA, se forem satisfeitas todas as garantias de consistência associadas com o nível de consistência. A tabela seguinte captura de garantias de consistência que correspondem aos modelos de consistência específico oferecidos pelo Azure Cosmos DB.

<table>
    <tr>
        <td><strong>Modelo de consistência</strong></td>
        <td><strong>Características de consistência</strong></td>
        <td><strong>SLA</strong></td>
    </tr>
        <tr>
        <td>Forte</td>
        <td>Linearizable</td>
        <td>100%</td>
    </tr>
    <tr>
        <td rowspan="3">Obsoletismo</td>
        <td>Ler Monotonic (dentro de uma região)</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Prefixo consistente</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Vinculada vinculada &lt; K, T</td>
        <td>100%</td>
    </tr>
<tr>
        <td rowspan="3">Sessão</td>
        <td>Ler a suas próprias escrita</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Leitura monotonic</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Prefixo consistente</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Prefixo consistente</td>
        <td>Prefixo consistente</td>
        <td>100%</td>
    </tr>
</table>

**Garantias de consistência associadas um modelo de consistência indicado do BD Azure Cosmos**


### <a id="ConsistencyAndAvailability"></a>Relação da consistência com a disponibilidade
O [resultado impossibility](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf) do [theorem extremidade](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf) comprova que é realmente impossível para um sistema a permanecem disponíveis e a oferecer consistência linearizable face a falhas. O serviço de base de dados tem de escolher a CP ou AP, onde os sistemas de CP potente disponibilidade favor consistência linearizable enquanto os sistemas da Ásia-Pacífico potente [consistência linearizable](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) favor de disponibilidade. BD do Azure do Cosmos nunca viola o modelo de pedido de consistência, que torna formally um sistema de CP. No entanto, na prática, consistência não é uma proposta todas ou nada; Existem vários modelos de consistência bem definidos ao longo do espetro de consistência entre consistência linearizable e eventual. Na base de dados do Azure Cosmos ter tentar identificar vários modelos de consistência simples que forem aplicáveis aos cenários no mundo real e são intuitiva a utilizar. BD do Azure do Cosmos navega fala disponibilidade de consistência, oferecendo um [vários flexibilizadas modelos de consistência ainda bem definidos](consistency-levels.md) e um 99,99% de disponibilidade para todas as únicas contas de base de dados de região e 99.999% leitura e escrita disponibilidade de todas as contas de multirregião base de dados. 

### <a id="ConsistencyAndAvailability"></a>Relação da consistência com latência
Uma variação mais abrangente de theorem extremidade denomina [PACELC](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf), que também as contas do fala latência e consistência no estado de repouso. -Estados que um Estado de repouso, um sistema de base de dados tem de escolher entre a consistência e latência. Com vários modelos de consistência simples (associados a replicação assíncrona e local leitura e escrita quorums), a base de dados do Azure Cosmos garante que todas as leituras e escritas são locais para a leitura e escrita regiões, respetivamente. Isto permite que a base de dados do Azure Cosmos oferecer garantias de latência baixa na região para os modelos de consistência indicado.  

### <a id="ConsistencyAndThroughput"></a>Relação da consistência com débito
Uma vez que a implementação de um modelo de consistência específico depende a escolha de um [tipo quórum](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf), débito também varia com base na escolha de um modelo de consistência. Por exemplo, Azure Cosmos DB, a taxa RU leituras vivamente consistentes é aproximadamente *duplo* de leituras eventualmente consistentes. Neste caso, terá de aprovisionar duplo as RUs para conseguir o mesmo débito.


![Relação entre a consistência e débito](./media/distribute-data-globally/consistency-and-throughput.png)

**Relação de capacidade de leitura para um modelo específico de consistência na base de dados do Azure Cosmos**

## <a id="ThroughputGuarantees"></a>Garantias de débito 
BD do Cosmos do Azure permite-lhe o débito de escala (como, armazenamento), aprovisionadas através de qualquer número de regiões, consoante as suas necessidades ou a pedido. 

![BD do Azure do Cosmos distribuída e coleções de partições](../cosmos-db/media/introduction/azure-cosmos-db-global-distribution.png)

**Um único contentor de base de dados do Azure Cosmos horizontalmente particionado (entre três partições de recursos dentro de uma região) e, em seguida, globalmente distribuída em três regiões do Azure**

Um contentor de base de dados do Azure Cosmos obtém distribuído duas dimensões (i) numa região e (ii) em regiões. Eis como: 

* **Distribuição local**: numa única região, um contentor de base de dados do Azure Cosmos é horizontalmente ampliar em termos de *partições de recurso*. Cada partição de recursos gere um conjunto de chaves é vivamente consistente e a elevada fisicamente a ser representado por réplicas de quatro também designado por um *conjunto de réplicas* e replicação da máquina de estado entre as réplicas. BD do Cosmos do Azure é um sistema totalmente regida de recursos, em que uma partição de recursos é responsável para fornecer a partilha de débito para a atribuição de recursos do sistema atribuído ao mesmo. A escala de um contentor de base de dados do Azure Cosmos é completamente transparente para os utilizadores. BD do Azure do Cosmos gere as partições de recursos e divide e intercala-os conforme necessário, como o armazenamento e alterar os requisitos de débito. 
* **Distribuição global**: se se tratar de uma base de dados de multirregião, cada uma das partições de recursos, em seguida, é distribuída por essas regiões. O proprietário do mesmo conjunto de chaves entre vários formulário de regiões de partições de recurso *partição conjunto* (consulte [figura anterior](#ThroughputGuarantees)).  As partições de recursos dentro de um conjunto de partição são coordenadas utilizando a replicação da máquina de estado em várias regiões associadas com a base de dados. Consoante o nível de consistência configurado, as partições de recursos dentro de um conjunto de partição estão configuradas dinamicamente com diferentes topologias (por exemplo, Estrela daisy cadeia, árvore etc.). 

Em virtude de uma partição extremamente reativo gestão, o balanceamento de carga e governação de recursos restrito, base de dados do Azure Cosmos permite-lhe aprovisionadas débito de escala em várias regiões do Azure associadas um contentor de BD do Cosmos Azure ou a base de dados. Alterar o débito aprovisionado é uma operação de tempo de execução na base de dados do Azure Cosmos. Semelhante para outras operações de base de dados, o Azure Cosmos DB garante que o limite superior absoluto de latência para o pedido de alteração débito aprovisionado. Por exemplo, a figura seguinte mostra o contentor de um cliente com débito aprovisionado aprovisionadas (entre 1 milhão - 10M pedidos/seg em duas regiões) com base no pedido.

![BD do Azure do Cosmos aprovisionadas aprovisionado débito](./media/distribute-data-globally/elastic-throughput.png)

**Contentor de um cliente com débito aprovisionado aprovisionadas (vários de 1M - 10M pedidos/seg)**

### <a id="ThroughputAndConsistency"></a>Relação de débito com consistência 
É igual à descrita no [relação da consistência com débito](#ConsistencyAndThroughput).

### <a id="ThroughputAndAvailability"></a>Relação de débito com disponibilidade
BD do Azure do Cosmos continua a manter a elevada disponibilidade do mesmo quando são efetuadas alterações ao débito aprovisionado. BD do Azure do Cosmos transparente gere as partições de recursos (e executa operações de divisão, intercalação e clone) e assegura que as operações não degradar o desempenho ou disponibilidade, enquanto a aplicação aprovisionadas aumenta ou diminui o débito. 

## <a id="AvailabilityGuarantees"></a>Garantias de disponibilidade
BD do Azure do Cosmos oferece um SLA de disponibilidade de 99,99% de todas as contas de base de dados única região e todas as contas de multirregião com consistência simples e 99.999% de disponibilidade em todas as contas de multirregião base de dados. Tal como descrito anteriormente, garantias de disponibilidade da BD do Azure Cosmos incluem um limite superior absoluto para latência para operações de plane todos os dados e o controlo. Não alteram as garantias de disponibilidade com o número de regiões ou a distância entre regiões geográfica. As garantias de disponibilidade são aplicáveis no que respeita às ativações pós-falha os manuais, bem como automáticas. BD do Azure do Cosmos oferece transparentes APIs multi homing que certifique-se de que a aplicação pode funcionar nos pontos finais lógicos e transparente pode encaminhar pedidos para uma região de novo em caso de uma ativação pós-falha. A aplicação não é necessário voltar a implementar no caso de uma ativação pós-falha e a disponibilidade SLAs são preservadas permanente.

### <a id="AvailabilityAndConsistency"></a>Relação de disponibilidade com consistência, débito e latência
Relação de disponibilidade com consistência, débito e latência é descrita nas secções [relação da consistência com disponibilidade](#ConsistencyAndAvailability), [relação da latência com disponibilidade](#LatencyAndAvailability) e [Relação do débito com disponibilidade](#ThroughputAndAvailability). 

## <a id="CustomerFacingSLAMetrics"></a>Métricas do SLA orientado para o cliente
BD do Azure do Cosmos transparente expõe as métricas de débito, latência, consistência e disponibilidade. Estas métricas estão acessíveis através de programação e através do portal do Azure (consulte a figura abaixo). Também pode definir alertas em vários limiares de utilizar o Azure Application Insights.
 

![Azure métricas SLA visíveis para o cliente Cosmos DB](./media/distribute-data-globally/customer-slas.png)

**Métricas de disponibilidade, débito, latência e consistência estão transparente disponíveis para cada inquilino**

## <a id="Next Steps"></a>Passos seguintes
* Para implementar a replicação global na sua conta de base de dados do Azure Cosmos no portal do Azure, consulte [como efetuar a replicação de base de dados global do Azure Cosmos DB no portal do Azure](tutorial-global-distribution-sql-api.md).
* Para saber mais sobre como implementar arquiteturas multi-mestre com BD do Cosmos do Azure, consulte [arquiteturas multi-mestre de base de dados com base de dados do Azure Cosmos](multi-region-writers.md).
* Para saber mais sobre as ativações pós-falha de forma automáticas e manual de trabalho do BD Azure Cosmos, consulte [as ativações pós-falha Regional do BD Azure Cosmos](regional-failover.md).

## <a id="References"></a>Referências
1. Eric Brewer. [Para sistemas distribuídos robustos](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf)
2. Eric Brewer. [CAP doze anos mais tarde – como as regras foram alterados](http://informatik.unibas.ch/fileadmin/Lectures/HS2012/CS341/workshops/reportsAndSlides/PresentationKevinUrban.pdf)
3. Gilbert, Lynch. - [Brewer&#39;s Conjecture e viabilidade da consistente, disponíveis, de serviços Web de tolerância a falhas de partição](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf)
4. Daniel Abadi. [Consistência fala no moderna distribuídas Design de sistemas de base de dados](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf)
5. Martin Kleppmann. [Pare a chamar bases de dados CP ou da Ásia-Pacífico](https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html)
6. Peter Bailis ter usados. [Probabilistic Obsoletismo (. PBS) para práticas Quorums parciais](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
7. Naor e Wool. [Carga, a capacidade e a disponibilidade em sistemas de quórum](http://www.cs.utexas.edu/~lorenzo/corsi/cs395t/04S/notes/naor98load.pdf)
8. Herlihy e satisfeitas. [Lineralizability: Uma correcção condição para objetos em simultâneo](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf)
9. [SLA de BD do Cosmos do Azure](https://azure.microsoft.com/support/legal/sla/cosmos-db/)
