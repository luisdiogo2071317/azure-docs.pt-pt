---
title: Distribuir dados globalmente com o Azure Cosmos DB | Documentos da Microsoft
description: Saiba mais sobre a recuperação de dados, georreplicação e ativação pós-falha de escala planetária utilizar bases de dados global do Azure Cosmos DB, um serviço de base de dados distribuída globalmente, modelo de estiverem.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: sngun
ms.openlocfilehash: 19e47e0dba1a89ea32f42ef0bafc26f8c59b4ad7
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43288307"
---
# <a name="how-to-distribute-data-globally-with-azure-cosmos-db"></a>Como distribuir dados globalmente com o Azure Cosmos DB
O Azure é onipresente – ele tem uma presença global em 50 + regiões geográficas e está continuamente a expandir. Com sua presença global, um dos recursos diferenciados que Azure oferece aos seus desenvolvedores é a capacidade de criar, implementar e gerir facilmente aplicações distribuídas globalmente. 

O [Azure Cosmos DB](../cosmos-db/introduction.md) é um serviço de bases de dados com vários modelos e distribuído globalmente da Microsoft para aplicações críticas para atividades. O Azure Cosmos DB oferece distribuição global chave na mão, [dimensionamento elástico de débito e armazenamento](../cosmos-db/partition-data.md) latências de milissegundos de todo o mundo, de um só dígito no percentil 99, [cinco modelos de consistência bem definidos](consistency-levels.md)e de elevada disponibilidade garantida, tudo apoiada por [SLAs abrangentes líderes da indústria](https://azure.microsoft.com/support/legal/sla/cosmos-db/). O Azure Cosmos DB [indexa automaticamente todos os seus dados](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) sem ter de lidar com a gestão de índices ou esquemas. Ele é um serviço com vários modelo e oferece suporte a documentos, chave-valor, gráfico e modelos de dados de família de colunas. Como um nativamente born no serviço cloud, Azure Cosmos DB cuidadosamente foi concebido com a distribuição global e de vários inquilinos, desde o backup.


![Contentor do Azure Cosmos DB particionado e distribuídos em três regiões](./media/distribute-data-globally/global-apps.png)

**Um único contentor do Azure Cosmos DB particionado e distribuídos em várias regiões do Azure**

À medida que aprendemos durante a criação do Azure Cosmos DB, a adição de distribuição global não pode ser uma reflexão tardia. Não pode ser "bolted em" sobre um sistema de banco de dados de "site único". Os recursos oferecidos por uma base de dados distribuído globalmente ir além do que de desastre geográfico tradicional recuperação (Geo-DR) oferecidos pelas bases de dados de "site único". Bases de dados de site único oferecendo a capacidade Geo-DR são um subconjunto estrito das bases de dados distribuídos globalmente. 

Com a distribuição global chave na mão do Azure Cosmos DB, os desenvolvedores não precisam criar seus próprios estruturais de replicação ao empregar o o padrão de Lambda (por exemplo, [replicação de AWS DynamoDB](https://github.com/awslabs/dynamodb-cross-region-library/blob/master/README.md)) sobre o registo de base de dados ou pelo efetuar "operações de escrita duplas" em várias regiões. Fazemos *não* Recomendamos essas abordagens, uma vez que é impossível garantir a correção dessas abordagens e fornecer SLAs de som. 

Neste artigo, fornecemos uma visão geral das capacidades de distribuição global do Azure Cosmos DB. Também descreveremos abordagem exclusiva do Azure Cosmos DB ao fornecimento de SLAs abrangentes. 

## <a id="EnableGlobalDistribution"></a>Ativar a distribuição global chave na mão
O Azure Cosmos DB fornece as seguintes capacidades para permitir que a escrever facilmente aplicações distribuídas globalmente. Estas capacidades estão disponíveis por meio de recursos do Azure Cosmos DB baseado em provedor [REST APIs](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/) , bem como o portal do Azure.

### <a id="RegionalPresence"></a>Onipresente presença regional 
Azure está constantemente a aumentar sua presença geográfica ao trazer [novas regiões](https://azure.microsoft.com/regions/) online. O Azure Cosmos DB é classificado como um *serviço fundamental* no Azure e está disponível em todas as regiões do Azure novo por predefinição. Isto permite-lhe associar uma região geográfica com a sua conta de base de dados do Azure Cosmos DB, assim que o Azure abre-se a nova região para empresas.

### <a id="UnlimitedRegionsPerAccount"></a>Associação de um número ilimitado de regiões com a sua conta de base de dados do Azure Cosmos DB
O Azure Cosmos DB permite-lhe associar qualquer número de regiões do Azure à sua conta de base de dados do Azure Cosmos DB. Fora do perímetro geográfico restrições (por exemplo, China, Alemanha), existem sem limites no número de regiões que podem ser associados com a sua conta de base de dados do Azure Cosmos DB. A figura seguinte mostra uma conta de base de dados configurada para distribuir por 25 regiões do Azure.  

![Conta de base de dados do Azure do Cosmos DB em 25 regiões do Azure](./media/distribute-data-globally/spanning-regions.png)

**Regiões do Azure 25 extensão conta da base de dados do Azure Cosmos DB um inquilino**


### <a id="PolicyBasedGeoFencing"></a>Perímetro geográfico baseada em políticas
O Azure Cosmos DB foi concebido para suportar o perímetro geográfico baseada em políticas. Perímetro geográfico é um componente importante para garantir que as restrições de governação e conformidade de dados e pode impedir que a associação de uma região específica a sua conta. Exemplos de perímetro geográfico incluem (mas não se limitam a) a distribuição global para as regiões dentro de uma cloud soberana (por exemplo, China e Alemanha) ou num limite de tributação do Governo (por exemplo, Austrália) de âmbito. As políticas são controladas utilizando os metadados da sua subscrição do Azure.

### <a id="DynamicallyAddRegions"></a>Adicionar e remover regiões dinamicamente
O Azure Cosmos DB permite-lhe adicionar (associar) ou remover (desassociar) regiões da sua conta de base de dados em qualquer altura (consulte [figura anterior](#UnlimitedRegionsPerAccount)). Devido à replicação paralela de dados entre partições, o Azure Cosmos DB garante que quando é adicionada uma nova região, está disponível para operações dentro de 30 minutos em qualquer lugar do mundo (supondo que os seus dados é 100 TB ou menos). 

### <a id="FailoverPriorities"></a>Prioridades de ativação pós-falha
Para controlar a seqüência exata de ativações pós-falha regionais em casos de uma falha, o Azure Cosmos DB permite-lhe associar um *prioridade* com várias regiões associadas com a base de dados de conta (veja a figura abaixo). O Azure Cosmos DB garante que a sequência de ativação pós-falha automática ocorre a ordem de prioridade que especificou. Para obter mais informações sobre as ativações pós-falha regionais, consulte [ativações pós-falha automáticas de regional para continuidade do negócio no Azure Cosmos DB](regional-failover.md).


![Configurar prioridades de ativação pós-falha com o Azure Cosmos DB](./media/distribute-data-globally/failover-priorities.png)

**Um inquilino do Azure Cosmos DB pode configurar a ordem de prioridade de ativação pós-falha (painel direito) para regiões associadas a uma conta de base de dados**

### <a id="ConsistencyLevels"></a>Vários modelos de consistência bem definidos para bases de dados distribuídos globalmente
Azure Cosmos DB suporta [vários modelos de consistência bem definidos, intuitivo e prático](consistency-levels.md) suporte dos SLAs. Pode escolher um modelo de consistência específico (da lista de opções disponíveis), dependendo da carga de trabalho/cenários. 

### <a id="TunableConsistency"></a>Consistência otimizável para bases de dados replicadas globalmente
O Azure Cosmos DB permite-lhe substituir e reduzir a escolha de consistência predefinido numa base por solicitação em tempo de execução através de programação. 

### <a id="DynamicallyConfigurableReadWriteRegions"></a>Leitura configurável dinamicamente e regiões de escrita
O Azure Cosmos DB permite-lhe configurar as regiões (associadas a base de dados) para "leitura", "gravação" ou "leitura/escrita" regiões. 

### <a id="ElasticallyScaleThroughput"></a>Dimensionar de forma elástica o débito entre regiões do Azure
Pode dimensionar um contentor do Azure Cosmos DB pela taxa de transferência de aprovisionamento por meio de programação. O débito é aplicado a todas as regiões que o contentor do Azure Cosmos DB é distribuído em.

### <a id="GeoLocalReadsAndWrites"></a>Leituras e escritas de geo-local
A principal vantagem de uma base de dados globalmente distribuída é que ele oferece acesso de baixa latência para dados em qualquer lugar no mundo. O Azure Cosmos DB oferece leituras de latência baixa e escreve no percentil de 99 em todo o mundo. Ele garante que todas as leituras são atendidas a partir da região mais próxima (local). Para atender a uma solicitação de leitura, é utilizado o quórum do local para a região em que a leitura é emitida. O mesmo se aplica a escritas. Uma gravação é reconhecida somente depois que a maioria das réplicas de maneira duradoura realizaram a gravação localmente, mas sem a ser Check controlado em réplicas remotas para confirmar as gravações. Colocá-lo de forma diferente, o protocolo de replicação do Azure Cosmos DB opera no pressuposto de que o quorums de leitura e escrita são sempre locais para a região em que o pedido foi emitido.

### <a id="ManualFailover"></a>Ativação pós-falha manual
O Azure Cosmos DB permite-lhe acionar a ativação pós-falha de uma conta de base de dados para validar a *ponto a ponto* propriedades de disponibilidade de toda a aplicação (além da base de dados). Uma vez que a segurança e as propriedades de liveness da eleição falha de deteção e líder são garantidas, o Azure Cosmos DB garante *perda de dados de zero* para uma operação de ativação pós-falha manual iniciadas por inquilino.

### <a id="AutomaticFailover"></a>Ativação pós-falha automática
Azure Cosmos DB suporta a ativação pós-falha automática durante um ou mais falhas regionais. Durante uma ativação pós-falha, o Azure Cosmos DB mantém sua latência de leitura, disponibilidade de tempo de atividade, consistência e SLAs de débito. O Azure Cosmos DB fornece um limite superior para a duração de uma operação de ativação pós-falha automática para concluir. Esta é a janela de potencial perda de dados durante uma falha regional.

### <a id="GranularFailover"></a>Concebido para granularidades de ativação pós-falha diferente
Atualmente, as capacidades de ativação pós-falha automática e manual são expostas na granularidade do que a conta de base de dados. Tenha em atenção, internamente o Azure Cosmos DB foi criada para oferecer *automática* ativação pós-falha na granularidade mais de uma base de dados, um contentor ou até mesmo uma partição (de um contentor de proprietário de um intervalo de chaves). 

### <a id="MultiHomingAPIs"></a>Multi-homing no Azure Cosmos DB
O Azure Cosmos DB permite-lhe interagir com uma base de dados através de um *lógico* (região desconhecida) ou *físico* (região) os pontos finais. A utilização de pontos de extremidade lógicos assegura que a aplicação de forma transparente pode ser multihomed em caso de uma ativação pós-falha. O último, um ponto de final físico, fornece um controlo detalhado para a aplicação para redirecionar as leituras e escritas para regiões específicas.

Pode encontrar informações sobre como configurar as preferências de leitura para o [API do SQL](../cosmos-db/tutorial-global-distribution-sql-api.md), [API de tabelas](../cosmos-db/tutorial-global-distribution-table.md), e [API do MongoDB](../cosmos-db/tutorial-global-distribution-mongodb.md) nestes artigos.

### <a id="TransparentSchemaMigration"></a>Migração de esquemas e índices de base de dados transparente e consistente 
O Azure Cosmos DB é totalmente [sem esquema](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf). O design exclusivo do motor de base de dados permite o Azure Cosmos DB para automaticamente e Indexe todos os dados após a ingestão, sem necessidade de qualquer esquema ou índices secundários do usuário. Isto permite-lhe iterá-la à aplicação distribuída globalmente rapidamente sem se preocupar sobre a migração de esquemas e índices de base de dados ou a coordenação de implementações de aplicações de múltiplas fases de alterações de esquema. O Azure Cosmos DB garante que quaisquer alterações feitas explicitamente por si de políticas de indexação não resultar em degradação de desempenho ou disponibilidade.  

### <a id="ComprehensiveSLAs"></a>SLAs abrangentes (além de elevada disponibilidade)
Como um serviço de base de dados globalmente distribuída, o Azure Cosmos DB oferece SLAs bem definidos e abrangentes para **disponibilidade**, **latência**, **débito**e **consistência** para a base de dados em execução à escala global, independentemente do número de regiões associadas a base de dados.  

## <a id="LatencyGuarantees"></a>Garantias de latência
A principal vantagem de um serviço de base de dados globalmente distribuído, como o Azure Cosmos DB é oferecer acesso de latência baixa aos seus dados em qualquer lugar no mundo. O Azure Cosmos DB oferece baixa latência garantida no percentil de 99 para várias operações de base de dados. O protocolo de replicação que utiliza o Azure Cosmos DB garante que as operações de base de dados (leituras e escritas) são sempre efetuadas na região local do cliente. A latência de SLA do Azure Cosmos DB fornece garantias no percentil de 99 para leituras, escritas indexadas (de forma síncrona) e consultas de vários tamanhos de solicitação e resposta. As garantias de latência de escrita de incluem confirmações de quórum maioria durável dentro da região local.

### <a id="LatencyAndConsistency"></a>Relação da latência com consistência 
Para um serviço globalmente distribuído oferecer consistência forte na configuração de distribuição global, ele precisa para replicar as gravações de modo síncrono ou para executar sincronizadamente leituras entre regiões. A velocidade da luz e a fiabilidade de rede de longa distância dita que a consistência forte resultará em latências superiores e disponibilidade reduzida de operações de base de dados. Por conseguinte, para lhe proporcionar a garantia de latências baixas no percentil de 99 e disponibilidade de 99,99% para todas as contas de região única e todas as contas de várias regiões com consistência flexível e 99,999% de disponibilidade em todas as contas de base de dados de várias regiões, o serviço devem empregar os replicação assíncrona. No folheio requer que o serviço também deve oferecer [modelo (s) de consistência bem definidos, relaxados](consistency-levels.md) – mais fraco do que forte (para oferecer garantias de latência e disponibilidade baixas) e o ideal é que mais forte do que "eventual" consistência (com um modelo de programação intuitivo).

O Azure Cosmos DB garante que uma operação de leitura não é necessário entrar em contacto réplicas em várias regiões para fornecer uma garantia de nível de consistência específico. Da mesma forma, garante que uma operação de escrita não é bloqueada enquanto os dados que está a ser replicados em todas as regiões ou seja, escritas são replicadas assincronamente em várias regiões). Ambos forte, bem como vários níveis de consistência flexível para contas de base de dados de várias regiões estão disponíveis. 

### <a id="LatencyAndAvailability"></a>Relação da latência com disponibilidade 
Latência e disponibilidade são os dois lados da mesma moeda. Falando sobre a latência de uma operação no estado estável e a disponibilidade na presença de falhas e as partições de rede. Do ponto de vista do aplicativo, uma operação de base de dados em execução lenta é distinguir de uma base de dados que não está disponível. 

Para distinguir latência elevada de indisponibilidade, o Azure Cosmos DB oferece um limite absoluto de latência das várias operações de base de dados. Se leva mais tempo do que o limite superior para concluir a operação de base de dados, o Azure Cosmos DB devolve um erro de tempo limite. O SLA de disponibilidade do Azure Cosmos DB garante que os tempos limite são contabilizados contra o SLA de disponibilidade. 

### <a id="LatencyAndThroughput"></a>Relação da latência com débito
O Azure Cosmos DB não faz escolha entre a latência e débito. Que respeita o SLA para os dois latência no percentil de 99 e que oferece o débito que aprovisionou. 

## <a id="ConsistencyGuarantees"></a>Garantias de consistência
Embora o [modelo de consistência forte](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) é o padrão de ouro de programação de dados, ele tem um preço acentuado de latência superior (em estado de repouso) e disponibilidade (em caso de falhas) reduzida. 

O Azure Cosmos DB oferece um modelo de programação bem definido por si de ponderar sobre a consistência de dados replicados. Para permitir que crie facilmente aplicações distribuídas globalmente com capacidade de multi homing, os modelos de consistência expostos pelo Azure Cosmos DB foram projetados para ser independente de região e independente de região de onde estão a ser as leituras e gravações servido. 

SLA de consistência do Azure Cosmos DB garante que 100% de pedidos de leitura irão cumprir a garantia de consistência para o modelo de consistência especificado por (seja a conta de base de dados ou o nível de pedidos). Uma solicitação de leitura é considerada cumpriu a consistência de SLA, se todas as garantias de consistência associadas com o nível de consistência são cumpridas. A tabela seguinte mostra as garantias de consistência que correspondem aos modelos de consistência específicos oferecidos pelo Azure Cosmos DB.

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
        <td rowspan="3">Estagnação limitada</td>
        <td>Leitura Monotónica (dentro de uma região)</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Prefixo consistente</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Envelhecimento vinculado &lt; K, T</td>
        <td>100%</td>
    </tr>
<tr>
        <td rowspan="3">Sessão</td>
        <td>Leia sua própria escrita</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Leitura monotónica</td>
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

**Garantias de consistência associadas a um modelo de consistência determinado no Azure Cosmos DB**


### <a id="ConsistencyAndAvailability"></a>Relação da consistência com a disponibilidade
O [resultado de possibilidade de pressionar](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf) da [Teorema CAP](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf) prova que é, de fato, é impossível para um sistema para continuam disponíveis e oferecer linearizable consistência de falhas. O serviço de base de dados tem de escolher a CP ou AP, onde os sistemas de CP abdicar de disponibilidade em favor de consistência linearizable enquanto os sistemas de AP abdicar [consistência linearizable](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) em favor de disponibilidade. O Azure Cosmos DB nunca viola o modelo de consistência pedido, o que torna formalmente um sistema de CP. No entanto, na prática consistência não é uma Proposição tudo ou nada; Existem vários modelos de consistência bem definidos ao longo do espetro de consistência entre linearizable e consistência eventual. No Azure Cosmos DB identifica vários modelos de consistência flexível que se aplicam a cenários reais e são mais intuitivo. O Azure Cosmos DB navega as compensações de disponibilidade de consistência, oferecendo uma [Relaxada de vários modelos de consistência ainda bem definidos](consistency-levels.md) e uma disponibilidade de 99,99% para todas as único contas de base de dados de região e 99,999% de leitura e escrita disponibilidade para todas as contas de base de dados de várias regiões. 

### <a id="ConsistencyAndAvailability"></a>Relação da consistência com latência
Denomina-se uma variação mais abrangente do Teorema CAP [PACELC](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf), que também representa as compensações de latência e consistência num estado estável. Ele declara que num estado estável, um sistema de banco de dados tem de escolher entre a consistência e a latência. Com vários modelos de consistência flexível (beneficia de replicação assíncrona e local leitura e escrita quorums), o Azure Cosmos DB garante que todas as leituras e gravações são locais para a leitura e escrita regiões, respetivamente. Isso permite que o Azure Cosmos DB para oferecem garantias de baixa latência dentro da região para os modelos de consistência determinado.  

### <a id="ConsistencyAndThroughput"></a>Relação da consistência com débito
Uma vez que a implementação de um modelo de consistência específica depende da escolha de um [tipo de quorum](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf), débito, também varia com base na escolha de um modelo de consistência. Por exemplo, no Azure Cosmos DB, a taxa de RU leituras fortemente consistentes é aproximadamente *duplo* que de leituras eventualmente consistentes. Neste caso, terá de aprovisionar o dobro o RUs para alcançar o mesmo débito.


![Relação entre a consistência e a taxa de transferência](./media/distribute-data-globally/consistency-and-throughput.png)

**Relação de capacidade de leitura para um modelo de consistência específico no Azure Cosmos DB**

## <a id="ThroughputGuarantees"></a>Garantias de débito 
O Azure Cosmos DB permite que dimensione o débito (como, armazenamento), elástica em qualquer número de regiões consoante as suas necessidades ou a pedido. 

![O Azure Cosmos DB distribuído e particionada contentores](../cosmos-db/media/introduction/azure-cosmos-db-global-distribution.png)

**Um único contentor do Azure Cosmos DB particionados horizontalmente (em três partições de recursos dentro de uma região) e, em seguida, distribuídos globalmente em três regiões do Azure**

Um contentor do Azure Cosmos DB é distribuído em duas dimensões (i) dentro de uma região e (ii) em várias regiões. Eis como: 

* **Distribuição local**: numa única região, um contentor do Azure Cosmos DB é horizontalmente aumentado horizontalmente em termos de *partições de recursos*. Cada partição de recursos gerencia um conjunto de chaves e é vivamente consistente e elevada disponibilidade fisicamente a ser representado por quatro réplicas também denominado um *conjunto de réplicas* e a replicação da máquina de estado entre essas réplicas. O Azure Cosmos DB é um sistema totalmente regida de recursos, em que uma partição de recursos é responsável para fornecer sua parte da taxa de transferência para o orçamento de recursos do sistema atribuídos ao mesmo. O dimensionamento de um contentor do Azure Cosmos DB é transparente para os utilizadores. O Azure Cosmos DB gerencia as partições de recursos e divide e intercala-las conforme necessário, como armazenamento e requisitos de débito se alteram. 
* **Distribuição global**: se é uma base de dados de várias regiões, cada uma das partições de recursos, em seguida, é distribuída entre essas regiões. Partições de recursos terem o mesmo conjunto de chaves entre vários formulários de regiões *conjunto de partição* (consulte [figura anterior](#ThroughputGuarantees)).  Partições de recursos dentro de um conjunto de partição são coordenadas com a replicação da máquina de estado em várias regiões associadas com a base de dados. Dependendo do nível de consistência configurado, as partições de recursos dentro de um conjunto de partição são configuradas dinamicamente usando diferentes topologias (por exemplo, star, cadeia ligados, a árvore etc.). 

Devido uma gestão de elevada capacidade de resposta de partição, balanceamento de carga e governação de recursos estrita, Azure Cosmos DB permite-lhe para débito de escala elástica em várias regiões do Azure associados a um contentor do Azure Cosmos DB ou a base de dados. Alterar o débito aprovisionado é uma operação de tempo de execução no Azure Cosmos DB. Assim como outras operações de base de dados, o Azure Cosmos DB garante o limite superior absoluto na latência para o seu pedido alterar o débito aprovisionado. Por exemplo, a figura seguinte mostra o contentor de um cliente com forma elástica o débito aprovisionado (que vão desde 1 milhão - 10 milhões de pedidos por segundo em duas regiões) com base na procura.

![O Azure Cosmos DB aprovisionado forma elástica o débito](./media/distribute-data-globally/elastic-throughput.png)

**Contentor de um cliente com um débito aprovisionado de forma elástica (variando de 1 milhão - 10 milhões de pedidos/seg)**

### <a id="ThroughputAndConsistency"></a>Relação do débito com consistência 
É igual à descrita no [relação da consistência com débito](#ConsistencyAndThroughput).

### <a id="ThroughputAndAvailability"></a>Relação do débito com disponibilidade
O Azure Cosmos DB continua a manter a sua disponibilidade elevada quando forem feitas alterações para o débito aprovisionado. O Azure Cosmos DB gere de forma transparente as partições de recursos (e efetua a divisão, intercalação e clone operações) e garante que as operações não degradar o desempenho ou disponibilidade, enquanto a aplicação de forma elástica aumenta ou diminui a taxa de transferência. 

## <a id="AvailabilityGuarantees"></a>Garantias de disponibilidade
O Azure Cosmos DB oferece um SLA de disponibilidade de 99,99% para todas as contas de base de dados de região única e todas as contas de várias regiões com consistência flexível e 99,999% de disponibilidade em todas as contas de base de dados de várias regiões. Conforme descrito anteriormente, garantias de disponibilidade do Azure Cosmos DB incluem um limite absoluto de latência para todas as operações do plano de dados e controle. As garantias de disponibilidade não são alterados com o número de regiões ou à distância geográfica entre regiões. Garantias de disponibilidade são aplicáveis em relação a ativações pós-falha manuais os e automáticas. O Azure Cosmos DB oferece APIs multi homing transparentes, que garantem que seu aplicativo pode funcionar nos pontos finais lógicos e transparente pode encaminhar os pedidos para uma nova região em caso de uma ativação pós-falha. Seu aplicativo não precisa de ser novamente implementada no caso de uma ativação pós-falha e a disponibilidade SLAs são preservados durante todo o tempo.

### <a id="AvailabilityAndConsistency"></a>Relação de disponibilidade com consistência, a latência e débito
Relação de disponibilidade com consistência, a latência e débito é descrita nas secções [relação da consistência com disponibilidade](#ConsistencyAndAvailability), [relação da latência com disponibilidade](#LatencyAndAvailability) e [Relação do débito com disponibilidade](#ThroughputAndAvailability). 

## <a id="CustomerFacingSLAMetrics"></a>Métricas de SLA do lado do cliente
O Azure Cosmos DB expõe forma transparente as métricas de débito, latência, consistência e disponibilidade. Estas métricas são acessíveis através do portal do Azure e através de programação (veja a figura abaixo). Também pode configurar alertas em vários limiares com o Azure Application Insights.
 

![Azure Cosmos DB visíveis para o cliente métricas do SLA](./media/distribute-data-globally/customer-slas.png)

**Métricas de consistência, latência, débito e disponibilidade estão transparente disponíveis para cada inquilino**

## <a id="Next Steps"></a>Passos seguintes
* Para implementar os replicação global na sua conta do Azure Cosmos DB no portal do Azure, veja [como efetuar a replicação de base de dados global do Azure Cosmos DB no portal do Azure](tutorial-global-distribution-sql-api.md).
* Para saber mais sobre como implementar arquiteturas de vários mestres com o Azure Cosmos DB, veja [arquiteturas de vários mestres de base de dados com o Azure Cosmos DB](multi-region-writers.md).
* Para saber mais sobre as ativações pós-falha como automáticas e manual de trabalho no Azure Cosmos DB, veja [ativações pós-falha regionais no Azure Cosmos DB](regional-failover.md).

## <a id="References"></a>Referências
1. Eric Brewer. [Em direção à robustas de sistemas distribuídos](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf)
2. Eric Brewer. [LIMITE de doze anos mais tarde – como as regras foram alterados](http://informatik.unibas.ch/fileadmin/Lectures/HS2012/CS341/workshops/reportsAndSlides/PresentationKevinUrban.pdf)
3. Gilbert, Lynch. - [Brewer&#39;s Conjecture e viabilidade de consistente e de disponibilidade, de serviços da Web de tolerância a falhas de partição](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf)
4. Daniel Abadi. [Vantagens e desvantagens de consistência no moderno distribuídas de Design de sistemas de banco de dados](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf)
5. Martin Kleppmann. [Pare a chamada de bases de dados CP ou Pacífico](https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html)
6. Peter Bailis e outros. [Probabilístico estagnação limitada (PBS) para práticos Quorums parciais](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
7. Naor e Wool. [Carga, a capacidade e a disponibilidade em sistemas de quórum](http://www.cs.utexas.edu/~lorenzo/corsi/cs395t/04S/notes/naor98load.pdf)
8. Herlihy e Guintes. [Lineralizability: Uma correção condição para objetos em simultâneo](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf)
9. [SLA do Azure Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/)
