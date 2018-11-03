---
title: Distribuição do Cosmos DB de global do Azure - sob definições avançadas
description: Este artigo fornece detalhes técnicos relacionados com a distribuição global do Azure Cosmos DB
services: cosmos-db
author: dharmas-cosmos
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: dharmas
ms.reviewer: sngun
ms.openlocfilehash: 656742727b2bd85ac93211c74d82fe11d0bc0f46
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50963902"
---
# <a name="azure-cosmos-db-global-distribution---under-the-hood"></a>Distribuição do Cosmos DB de global do Azure - sob definições avançadas

O Azure Cosmos DB é um serviço fundamental do Azure, para que ele é implantado em todas as regiões do Azure em todo o mundo incluindo o público, soberanas, departamento de defesa (DoD) e clouds de administração pública. No Centro de dados, implementar e gerir o Azure Cosmos DB em enormes carimbos de máquinas, cada um deles com armazenamento local dedicado. Dentro de um centro de dados do Azure Cosmos DB é implementado em muitos clusters, cada qual potencialmente executando várias gerações de hardware. Máquinas de um cluster normalmente são distribuídas por domínios de falha de 10 a 20. A imagem seguinte mostra a topologia de sistema de distribuição global do Cosmos DB:

![Topologia do sistema](./media/global-dist-under-the-hood/distributed-system-topology.png)

**Distribuição global no Azure Cosmos DB é a chave na mão:** em qualquer altura, com alguns cliques ou programaticamente com uma única chamada de API, pode adicionar ou remover regiões geográficas, associados com a base de dados do Cosmos. Uma base de dados do Cosmos por sua vez consiste num conjunto de contentores do Cosmos. No Cosmos DB, os contentores servem como as unidades lógicas de distribuição e a escalabilidade. As coleções, tabelas e gráficos que cria são (internamente) apenas os contentores de Cosmos. Os contentores são completamente independente de esquema e fornecer um âmbito para uma consulta. Dados num contentor do Cosmos são indexados automaticamente após a ingestão. Indexação automática permite aos utilizadores consultar os dados sem ter de lidar com o esquema ou preocupações de gestão de índices, especialmente num programa de configuração distribuído globalmente.  

- Numa determinada região, os dados dentro de um contêiner são distribuídos ao utilizar uma chave de partição, que proporcionam e geridos de forma transparente pelas partições de recursos subjacentes (distribuição local).  

- Cada partição de recursos também é replicada nas regiões geográficas (distribuição global). 

Quando uma aplicação com Cosmos DB de forma elástica dimensiona o débito (ou consome mais armazenamento) num contêiner de Cosmos, Cosmos DB transparente lida com operações de gestão de partição (dividir, clonar, eliminar) em todas as regiões. Independente da escala, distribuição ou falhas, o Cosmos DB continua a fornecer uma imagem de sistema único dos dados dentro de contentores, o que são distribuídos globalmente em qualquer número de regiões.  

Conforme mostrado na imagem seguinte, os dados dentro de um contêiner são distribuídos duas dimensões:  

![Partições de recursos](./media/global-dist-under-the-hood/distribution-of-resource-partitions.png)

Uma partição de recursos é implementada por um grupo de réplicas, chamado de um conjunto de réplicas. Cada máquina aloja centenas de réplicas que correspondem às várias partições de recursos dentro de um conjunto fixo de processos, conforme mostrado na imagem anterior. Réplicas correspondente para as partições de recursos são colocadas de forma dinâmica e balanceamento de carga em máquinas dentro de um cluster e datacenters dentro de uma região.  

Uma réplica pertence exclusivamente a um inquilino do Azure Cosmos DB. Cada réplica aloja uma instância do Cosmos DB [motor de base de dados](https://www.vldb.org/pvldb/vol8/p1668-shukla.pdf), que gere os recursos, bem como os índices associados. O motor de base de dados do Cosmos DB opera num sistema de tipo baseada em registo sequência atom (ARS). O motor é agnóstico para o conceito de um esquema e desfocar os limites entre os valores de estrutura e a instância de registos. O cosmos DB alcança agnosticism de esquema completo ao indexar automaticamente tudo o que após a ingestão de uma forma eficiente, que permite aos utilizadores consultar seus dados distribuídos globalmente sem ter de lidar com a gestão de índices ou esquemas.

O motor de base de dados do Cosmos DB consiste em componentes, incluindo a implementação de vários primitivos de coordenação, runtimes de linguagens, o processador de consultas e o armazenamento e indexação subsistemas responsáveis pelo armazenamento transacional e indexação de dados, respectivamente. Para fornecer durabilidade e elevada disponibilidade, o motor de base de dados persistir os dados e índice SSDs e replica-a entre as instâncias de motor de base de dados dentro da réplica-conjuntos, respetivamente. Inquilinos maiores correspondem a uma escala maior de débito e armazenamento e têm maiores ou mais o número de réplicas ou ambos. Cada componente do sistema é totalmente assíncrono – nenhum thread nunca bloqueia e cada thread faz o trabalho de curta duração sem incorrer em quaisquer comutadores de thread desnecessários. Limitação de velocidade e a pressão de back são inseridas em toda a pilha de controle de admissão para todos os caminhos de e/s. Nosso motor de base de dados foi concebido para explorar a simultaneidade refinada e para fornecer alto débito enquanto opera no gastar quantidades de recursos do sistema.

Distribuição global do cosmos DB baseia-se em duas chaves abstrações – conjuntos de réplicas e conjuntos de partição. Um conjunto de réplicas é um bloco modular de Lego para coordenação e um conjunto de partição é uma sobreposição de dinâmica de uma ou mais partições de recursos distribuídos geograficamente. Para entender a distribuição global como funciona, é preciso compreender estas duas abstrações de chave. 

## <a name="replica-sets"></a>Conjuntos de réplicas

Uma partição de recursos é materializada como um grupo de gestão automática centrado no e dinamicamente com balanceamento de carga das réplicas distribuídas por vários domínios de falha, chamados de um conjunto de réplicas. Este conjunto coletivamente implementa o protocolo de máquina de estado replicadas para tornar os dados dentro da partição de recursos de elevada disponibilidade, durável e consistente. A associação de conjunto de réplicas N é dinâmica – ela mantém a flutuar entre nmín e nmáx com base nas falhas, operações administrativas e a hora para réplicas com falha para voltar a gerar/recuperar. Com base nas alterações de associação, a replicação de protocolo também reconfigura o tamanho de leitura e escrita quorums. Para distribuir uniformemente a taxa de transferência que é atribuída a uma partição de recursos específico, o que podemos utilizar duas idéias: primeiro, o custo de processamento de pedidos de escrita no coordenador é superior ao custo de aplicar as atualizações descritos abaixo. Do mesmo modo, o coordenador é orçado mais recursos do sistema que o seguidores. Em segundo lugar, máximo possível, o quórum de leitura para um nível de consistência determinado é composto exclusivamente as réplicas de descritos abaixo. Podemos evitar entrar em contato com o líder para satisfazer as necessidades leituras, a menos que necessário. Empregamos ideias de pesquisa feita na relação de diversas [carga e capacidade](http://www.cs.utexas.edu/~lorenzo/corsi/cs395t/04S/notes/naor98load.pdf) nos sistemas baseados em quórum para a consistência de cinco modelos que Cosmos DB suporta.  

## <a name="partition-sets"></a>Conjuntos de partição

Um grupo de partições de recursos, uma de cada um dos configurada com as regiões de base de dados do Cosmos, é composta por para gerir o mesmo conjunto de chaves replicados em todas as regiões configuradas. Este primitivo de coordenação superior é chamado de uma partição-set - uma sobreposição distribuída geograficamente dinâmica de gerenciamento de um determinado conjunto de chaves de partições de recursos. Embora uma partição de recursos específico (uma réplica-set) tem um âmbito num cluster, um conjunto de partição pode abranger clusters, centros de dados e regiões geográficas, conforme mostrado na imagem seguinte:  

![Conjuntos de partição](./media/global-dist-under-the-hood/dynamic-overlay-of-resource-partitions.png)
**conjunto de partição é uma sobreposição de dinâmica de partições de recursos**

Pode pensar um conjunto de partição como um geograficamente disperso "super-conjunto de réplicas", que é composto de vários conjuntos de réplicas proprietário o mesmo conjunto de chaves. Semelhante a um conjunto de réplicas, a associação uma partição-set é também dinâmica – ele varia com base nas operações de gestão de partição de recursos implícito para adicionar/remover novas partições de/para um determinado conjunto de partição (por exemplo, quando aumenta horizontalmente débito num contentor, adicionar ou remover uma região para a base de dados do Cosmos ou quando ocorrem falhas) por terem cada uma das partições (de um conjunto de partição) gerir a associação de conjunto de partição dentro de seu próprio conjunto de réplicas, a associação é totalmente descentralizada e altamente está disponível. Durante a reconfiguração de um conjunto de partição, a topologia da sobreposição entre partições de recursos também é estabelecida. A topologia dinamicamente é selecionada com base no nível de consistência, a distância geográfica e a largura de banda de rede disponível entre a origem e as partições de recursos de destino.  

O serviço permite-lhe configurar as bases de dados do Cosmos com uma região de escrita única ou várias regiões de escrita e, consoante a opção, os conjuntos de partição estão configurados para aceitarem escritas em exatamente uma ou todas as regiões. O sistema utiliza um protocolo de consenso dois níveis, aninhada – um nível opera em réplicas de um conjunto de réplicas de uma partição de recursos aceitar as gravações e o outro funciona no nível de um conjunto de partição para fornecer garantias de ordenação completas para todos as gravações consolidadas dentro do conjunto de partição. Este consenso em várias camadas, aninhado é essencial para a implementação dos SLAs rigorosas para elevada disponibilidade, bem como a implementação dos modelos de consistência, Cosmos DB oferece aos seus clientes.  

## <a name="conflict-resolution"></a>Resolução de conflitos

Nosso design para a propagação de atualização, resolução de conflitos e de controlo de causalidade é inspirado de trabalho anterior no [algoritmos epidemic](http://www.cs.utexas.edu/~lorenzo/corsi/cs395t/04S/notes/naor98load.pdf) e o [Bayou](http://zoo.cs.yale.edu/classes/cs422/2013/bib/terry95managing.pdf) system. Embora os kernels das idéias tem Sobreviveu e fornecem uma arquitetura de referência conveniente para comunicar o design de sistema do Cosmos DB, eles têm também passou por transformações significativas como Aplicamos no sistema de Cosmos DB. Isso era necessária, porque os sistemas anteriores foram projetados nem com a governação de recursos, nem com a escala em que o Cosmos DB precisa operar nem para fornecer os recursos (por exemplo, a consistência de estagnação limitada) e o rigorosas e abrangente SLAs do Cosmos DB oferece aos seus clientes.  

Lembre-se de que um conjunto de partição é distribuído em várias regiões e segue o protocolo de replicação de Cosmos BD (com vários mestres) para replicar dados entre as partições de recursos que consiste num determinado conjunto de partição. Cada partição de recursos (de um conjunto de partição) aceita escritas e serve de leituras, normalmente, para os clientes que são locais para essa região. Aceite por uma partição de recursos dentro de uma região de escrita é maneira duradoura consolidada e altamente disponível dentro da partição de recursos antes de eles serem reconhecidos no cliente. Estas são escritas provisórias e são propagadas para outras partições de recursos no conjunto de partição utilizando um canal de entropia anti. Os clientes podem pedir escritas provisórias ou consolidadas, passando um cabeçalho de pedido. A propagação de entropia anti (incluindo a frequência de propagação) é dinâmica, com base na topologia de proximidade partição-set, regional das partições de recursos e a consistência, nível configurado. Dentro de um conjunto de partição, o Cosmos DB segue um esquema de consolidação primário com uma partição do arbiter dinamicamente selecionado. A seleção do arbiter é dinâmica e é uma parte integral da reconfiguração do conjunto de partição com base na topologia de sobreposição. As gravações confirmadas (incluindo várias-row/batches criados de atualizações) são garantidas para ser ordenada. 

Empregamos relógios de vetor com codificação (que contém o ID de região e relógios lógicos correspondentes a cada nível de consenso no conjunto de réplicas e o conjunto de partição, respectivamente) para controlo de causalidade e vetores para detetar e resolver conflitos de atualização de versão. A topologia e o algoritmo de seleção de elemento de rede foram concebidos para garantir fixo e um mínimo de armazenamento e a sobrecarga de rede mínima de vetores de versão. O algoritmo garante a propriedade de convergência rigorosa.  

Para as bases de dados Cosmos configurados com várias regiões de escrita, o sistema oferece vários automática de conflitos flexível políticas de resolução para os desenvolvedores à sua escolha, incluindo: 

- Última-escrita-Wins (LWW) que, por predefinição, utiliza uma propriedade de definidos pelo sistema timestamp (que é baseada no protocolo de sincronização de hora de relógio). Também o cosmos DB permite-lhe especificar qualquer outra propriedade personalizada numérica a ser utilizado para resolução de conflitos.  
- Definido pelo aplicativo política personalizada do conflito resolução (expressada por meio de procedimentos de intercalação) que foi concebida para reconciliação de semântica definida pelo aplicativo de conflitos. Estes procedimentos são invocados após a deteção de conflitos de escrita-escrita dos auspícios de uma transação de base de dados no lado do servidor. O sistema fornece exatamente garantir uma vez para a execução de um procedimento de mesclagem como parte do protocolo de compromisso. Existem vários exemplos disponíveis experimentar.  

## <a name="consistency-models"></a>Modelos de consistência

Se configurar a base de dados do Cosmos com uma única ou várias regiões de escrita, pode escolher entre cinco modelos de consistência bem definidos. Com o recentemente adicionado suporte para permitir várias regiões de escrita, seguem-se alguns dos aspectos notáveis dos níveis de consistência:  

Como antes, a consistência de estagnação limitada garante que todas as leituras será dentro de prefixos k ou segundos da t da operação de escrita mais recente em qualquer uma das regiões. Além disso, leituras com consistência de estagnação limitada são garantidas monotónica e com garantias de prefixo consistente. O protocolo de entropia anti funciona de forma limitada de taxa e garante que os prefixos não acumular e backpressure nas gravações não tem de ser aplicada. Como antes, a sessão monotónica garante a consistência de leitura, escrita monotónica, leia suas próprias escritas, escrita-forma-leitura e de prefixo consistente garante em todo o mundo. Para as bases de dados configuradas com consistência forte, os comutadores de sistema para uma única região de escrita ao designar um líder de dentro de cada um dos conjuntos de partição. 

A semântica dos modelos de cinco consistência no Cosmos DB é descrita [aqui](consistency-levels.md) e matematicamente mostrado, usando um alto nível TLA + especificações [aqui](https://github.com/Azure/azure-cosmos-tla).

## <a name="next-steps"></a>Passos Seguintes

Em seguida, saiba como configurar a distribuição global utilizando os seguintes artigos:

* [Como configurar clientes para multi-homing](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [Como adicionar ou remover regiões da sua base de dados](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [Como criar uma política de resolução de conflito personalizado para contas da API de SQL](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)
