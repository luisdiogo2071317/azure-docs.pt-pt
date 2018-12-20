---
title: Introdução ao Azure Cosmos DB
description: Saiba mais sobre o Azure Cosmos DB. Esta base de dados de distribuição global com vários modelos foi concebida para baixa latência, escalabilidade elástica e elevada disponibilidade, e fornece suporte nativo para dados NoSQL.
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: overview
ms.date: 12/18/2018
ms.author: sngun
ms.custom: mvc
ms.openlocfilehash: 133f8e82d7a46bae11eb049b2d1c9393990c9042
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/19/2018
ms.locfileid: "53634580"
---
# <a name="welcome-to-azure-cosmos-db"></a>Bem-vindo ao Azure Cosmos DB

Aplicações atuais são necessários para estar sempre online e elevada capacidade de resposta. Para alcançar a baixa latência e elevada disponibilidade, instâncias desses aplicativos precisam ser implantados nos centros de dados que estão próximas dos seus usuários. Aplicativos precisam responder em tempo real para grandes alterações na utilização nas horas de pico, armazenar aumento constante dos volumes de dados e disponibilizar estes dados aos utilizadores em milissegundos.

O Azure Cosmos DB é um serviço da base de dados de vários modelos distribuída globalmente da Microsoft. Com o clique de um botão, o Azure Cosmos DB permite-lhe dimensionar de forma elástica e independente o débito e o armazenamento em qualquer número de regiões geográficas do Azure. Elástica pode dimensionar o débito e armazenamento e tirar partido de acesso de dados de milissegundos de dígito único de forma rápida usando a API de favorita entre o SQL, MongoDB, Cassandra, tabelas ou Gremlin. O cosmos DB fornece abrangente [contratos de nível de serviço](https://aka.ms/acdbsla) (SLAs) de débito, latência, disponibilidade e garantias de consistência, algo nenhum outro serviço de base de dados pode oferecer.

Pode [Experimentar o Azure Cosmos DB gratuitamente](https://azure.microsoft.com/try/cosmosdb/) sem uma subscrição do Azure, sem encargos nem compromissos.

> [!div class="nextstepaction"]
> [Experimentar o Azure Cosmos DB gratuitamente](https://azure.microsoft.com/try/cosmosdb/)

![O Azure Cosmos DB é um serviço de base de dados de distribuição global da Microsoft com aumento horizontal elástico, baixa latência garantida, cinco modelos de consistência e SLAs abrangentes garantidos](./media/introduction/azure-cosmos-db.png)

## <a name="key-benefits"></a>Principais vantagens

### <a name="turnkey-global-distribution"></a>Distribuição global chave na mão

O cosmos DB permite-lhe criar elevada capacidade de resposta e aplicações de elevada disponibilidade em todo o mundo. O cosmos DB replica de forma transparente os dados onde quer que estejam os seus utilizadores, para que os usuários podem interagir com uma réplica dos dados mais próximos da-los.

O cosmos DB permite-lhe adicionar ou remover qualquer uma das regiões do Azure à sua conta do Cosmos em qualquer altura, com um clique de um botão. O cosmos DB será perfeitamente replicar os seus dados para todas as regiões à sua conta do Cosmos enquanto a sua aplicação continua a ter elevada disponibilidade graças às capacidades do serviço de multi homing.

Para obter mais informações, consulte a [distribuição global](distribute-data-globally.md) artigo.

### <a name="always-on"></a>Sempre "ativada"

Devido uma integração profunda com a infraestrutura do Azure e [replicação de vários mestre transparente](global-dist-under-the-hood.md), Cosmos DB fornece 99,999% [elevada disponibilidade](high-availability.md) para as leituras e escritas. O cosmos DB também fornece a capacidade de forma programática (ou através do Portal) invocar a ativação pós-falha regional da sua conta do Cosmos. Esta capacidade ajuda a garantir que enquanto uma base de dados do Cosmos pode automaticamente de ativação pós-falha, o resto do seu aplicativo também foi desenvolvido para ativação pós-falha se houver um desastre regional.

### <a name="elastic-scalability-of-throughput-and-storage-worldwide"></a>Escalabilidade elástica de débito e armazenamento, em todo o mundo

Concebido com a criação de partições horizontais transparente e replicação multimestre, o Cosmos DB oferece uma escalabilidade elástica sem precedentes para o seu escritas e leituras, tudo em todo o mundo. Pode elasticamente ampliação de milhares a centenas de milhões de pedidos por segundo em todo o mundo, com uma única chamada de API e pague apenas pelo débito (e armazenamento) que precisa. Esta capacidade ajuda-o a lidar com picos inesperados em suas cargas de trabalho sem ter de aprovisionar excessivamente para o pico. Ver [criação de partições no Cosmos DB](partitioning-overview.md), [débito aprovisionado em contentores e bases de dados](set-throughput.md), e [dimensionar o débito aprovisionado globalmente](scaling-throughput.md).

### <a name="guaranteed-low-latency-at-99th-percentile-worldwide"></a>Baixa latência garantida no percentil 99, em todo o mundo

Com o Cosmos DB, pode criar aplicações altamente responsivo, à escala planetária. Com o protocolo de replicação de vários mestres novo e, em seguida, livre de bloqueios temporários e [motor de base de dados otimizado para escrita](index-policy.md), Cosmos DB garante latências inferiores a 10 ms para ambos, leituras e gravações no percentil 99, em todo o mundo (indexadas) . Esta capacidade permite constante ingestão de dados e consultas de incrivelmente rápida para aplicações de elevada capacidade de resposta.

### <a name="precisely-defined-multiple-consistency-choices"></a>Precisamente definido, várias escolhas de consistência

Já não é preciso fazer extremo [as responsabilidades entre consistência, disponibilidade, latência e programação](consistency-levels-tradeoffs.md). Protocolo de replicação de vários mestres do cosmos DB foi criado com cuidado para oferecer [cinco escolhas de consistência bem definidos](consistency-levels.md) – forte, estagnação limitada, prefixo consistente, sessão e eventual — para um modelo de programação intuitivo com baixa latência e elevada disponibilidade para a sua aplicação distribuída globalmente.

### <a name="no-schema-or-index-management"></a>Sem gestão de índices ou esquemas

Manter o esquema de base de dados e os índices em sincronia com o esquema de um aplicativo é particularmente dolorosa para aplicações distribuídas globalmente. No entanto, com o Cosmos DB, não terá de lidar com esquemas ou índices. O motor de base de dados é totalmente sem esquema.  Uma vez que sem gestão de esquemas e índices for necessário, também não precisa se preocupar sobre o tempo de inatividade da aplicação enquanto a migração de esquemas. O cosmos DB [indexa automaticamente todos os dados](index-policy.md) – sem esquema, sem índices necessários – e serve consultas rápidas.

### <a name="battle-tested-database-service"></a>Serviço de base de dados totalmente testada

O cosmos DB é um serviço fundamental no Azure. Há quase uma década, o Cosmos DB tem sido usado por muitos dos produtos da Microsoft para aplicativos de missão crítica à escala global, incluindo o Skype, Xbox, Office 365, Azure e muitos outros. Hoje em dia, o Cosmos DB é um dos serviços mais rápida crescentes no Azure utilizado por muitos clientes externos e aplicações que requerem escala elástica e/ou a replicação de várias datacenter/várias regiões, de vários mestre chave na mão para baixa latência e elevada disponibilidade de ambos leituras e gravações.

### <a name="ubiquitous-regional-presence"></a>Onipresente presença regional

O cosmos DB está disponível em todas as regiões do Azure em todo o mundo, incluindo 54 + regiões na cloud pública, o Azure China 21Vianet, o Azure Alemanha, o Azure Government e o Azure Government para o departamento de defesa. Ver [presença regional do Cosmos DB](regional-presence.md).

### <a name="secure-by-default-and-enterprise-ready"></a>Seguro por padrão e pronto para as empresas

O cosmos DB está certificado para um [vasto leque de normas de conformidade](compliance.md). Além disso, todos os dados no Cosmos DB são encriptados em descanso e em movimento. O cosmos DB fornece autorização ao nível da linha e em conformidade com normas de segurança restritos.

### <a name="significant-tco-savings"></a>Poupanças significativas de custo total de posse

Uma vez que o Cosmos DB é um serviço totalmente gerido, já não precisar de gerir e operar implementações de centro de dados de várias complexas e atualizações de software da base de dados, pagar o suporte, licenciamento ou operações. Ver [otimizar o custo com o Cosmos DB](total-cost-ownership.md).

### <a name="industry-leading-comprehensive-slas"></a>SLAs abrangentes de líder da indústria

O cosmos DB é o primeiro e único serviço a oferecer [SLAs abrangentes líderes da indústria](https://azure.microsoft.com/support/legal/sla/cosmos-db/) incluindo elevada disponibilidade de 99,999%, ler e latência no percentil 99, débito e consistência a garantia de escrita.

### <a name="apache-spark--cosmos-db--operational-analytics-at-global-scale"></a>Apache Spark + Cosmos DB = análise operacional à escala global

Pode executar [Spark](spark-connector.md) em dados armazenados no Cosmos DB. Esta capacidade permite-lhe efetuar análises operacionais de baixa latência, à escala global sem afetar as cargas de trabalho transacionais operacional diretamente no Cosmos DB.

### <a name="develop-applications-for-cosmos-db-using-popular-nosql-apis"></a>Desenvolver aplicativos para o Cosmos DB com APIs populares de NoSQL

O cosmos DB oferece uma opção de APIs para atualizar e consultar os dados armazenados no seu banco de dados do Cosmos. Por predefinição, [é possível usar SQL](how-to-sql-query.md) para atualizar e consultar os dados no seu banco de dados do Cosmos.

O cosmos DB também implementa [Cassandra](cassandra-introduction.md), [MongoDB](mongodb-introduction.md), [Gremlin](graph-introduction.md) e [Azure Table Storage](table-introduction.md) conectar protocolos diretamente no serviço. Isto permite-lhe do ponto de controladores do cliente (e ferramentas) para as APIs de NoSQL usadas diretamente à sua base de dados do Cosmos. Ao suportar os protocolos de transmissão de frequentemente utilizadas APIs de NoSQL, o Cosmos DB permite-lhe:

* Migre facilmente a sua aplicação para o Cosmos DB, preservando a partes significativas da lógica do aplicativo.
* Mantenha seu aplicativo portátil e continuará desconhecidas do fornecedor na cloud.
* Obtenha setor SLAs líderes do setor, com apoio financeiro para as APIs do NoSQL comuns. 
* Elástica dimensionar o débito aprovisionado e armazenamento para as bases de dados com base nas suas necessidades e pague apenas o débito e armazenamento de que precisa. Isso leva à redução significativa de custos.

## <a name="solutions-that-benefit-from-azure-cosmos-db"></a>Soluções que tiram partido do Azure Cosmos DB

Qualquer [aplicação Web, móvel, de jogos e de IoT](use-cases.md) que tenha de lidar com quantidades gigantescas de dados, leituras e escritas numa escala [global](distribute-data-globally.md) com tempos de resposta em tempo quase real para os mais variados dados tira partido da elevada disponibilidade [garantida](https://azure.microsoft.com/support/legal/sla/cosmos-db/), do elevado débito, da baixa latência e da consistência ajustável do Azure Cosmos DB. Saiba como o Azure Cosmos DB pode ser aplicado a [IoT e telemática](use-cases.md#iot-and-telematics), [Revenda e marketing](use-cases.md#retail-and-marketing), [Jogos](use-cases.md#gaming) e [aplicações móveis e Web](use-cases.md#web-and-mobile-applications).

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o Cosmos DB [distribuição global](distribute-data-globally.md) e [criação de partições](partitioning-overview.md) capacidades.

Introdução ao Azure Cosmos DB com um dos nossos manuais de introdução:

* [Introdução à API SQL do Azure Cosmos DB](create-sql-api-dotnet.md)
* [Introdução à API do Azure Cosmos DB para MongoDB](create-mongodb-nodejs.md)
* [Introdução à API Cassandra do Azure Cosmos DB](create-cassandra-dotnet.md)
* [Introdução à API do Gremlin do Azure Cosmos DB](create-graph-dotnet.md)
* [Introdução à API de Tabela do Azure Cosmos DB](create-table-dotnet.md)

> [!div class="nextstepaction"]
> [Experimentar o Azure Cosmos DB gratuitamente](https://azure.microsoft.com/try/cosmosdb/)
