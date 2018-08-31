---
title: O Azure Cosmos DB perguntas mais frequentes | Documentos da Microsoft
description: Obtenha respostas para perguntas mais frequentes sobre o Azure Cosmos DB, um serviço de base de dados com múltiplos modelos distribuída globalmente. Saiba mais sobre a capacidade, níveis de desempenho e dimensionamento.
keywords: Perguntas de base de dados, perguntas mais frequentes perguntas, documentdb, azure, do Microsoft azure
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: sngun
ms.openlocfilehash: a8d9704c48801b98800abb71769ba0954e727848
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190461"
---
# <a name="azure-cosmos-db-faq"></a>FAQ do Azure Cosmos DB
## <a name="azure-cosmos-db-fundamentals"></a>Noções básicas do Azure do Cosmos DB

### <a name="what-happened-to-the-documentdb-api"></a>O que aconteceu com a API do DocumentDB?

O Azure Cosmos DB DocumentDB API ou o SQL (DocumentDB) API é agora conhecido como API de SQL do Azure Cosmos DB. Não precisa alterar nada para continuar a executar as suas aplicações criadas com a API do DocumentDB. A funcionalidade permanece igual.

Se tiver uma conta de API do DocumentDB antes, agora tem uma conta da API de SQL, sem qualquer alteração à sua faturação. 

### <a name="what-happened-to-azure-documentdb-as-a-service"></a>O que aconteceu para o Azure DocumentDB como serviço?

O serviço DocumentDB do Azure faz agora parte do serviço do Azure Cosmos DB e se manifesta na forma da API de SQL. Os aplicativos criados com o DocumentDB do Azure serão executada sem alterações de API de SQL do Azure Cosmos DB. Além disso, o Azure Cosmos DB suporta a Graph API, o API de tabela, a API do MongoDB e o Cassandra API (pré-visualização).

### <a name="what-are-the-typical-use-cases-for-azure-cosmos-db"></a>Quais são os casos de utilização típica para o Azure Cosmos DB?
Azure Cosmos DB é uma boa opção para o novo web, móvel, jogos e aplicações de IoT onde o dimensionamento automático, desempenho previsível, rápida ordem dos tempos de resposta de milissegundos e a capacidade de consultar sobre dados sem esquema é importante. O Azure Cosmos DB presta-se para um desenvolvimento rápido e suportar a iteração contínua de modelos de dados de aplicação. As aplicações que gerem conteúdos gerados pelo utilizador e os dados são [casos de utilização comuns para o Azure Cosmos DB](use-cases.md). 

### <a name="how-does-azure-cosmos-db-offer-predictable-performance"></a>Como é que o Azure Cosmos DB oferece um desempenho previsível?
R [unidades de pedido](request-units.md) (RU) é a medida de débito no Azure Cosmos DB. Um débito de 1 RU corresponde ao débito do GET de um documento de 1 KB. Cada operação no Azure Cosmos DB, incluindo leituras, escritas, consultas SQL e execuções de procedimentos armazenados, têm um valor de RU determinístico com base no débito necessário para concluir a operação. Em vez de pensar no CPU, memória e e/s e como elas cada afetam o débito da sua aplicação, pode pensar em termos de uma única medida de RU.

Pode reservar cada contentor do Azure Cosmos DB com um débito aprovisionado em termos de RUs de débito por segundo. Para aplicativos de qualquer escala, pode referenciar pedidos individuais para medir os respetivos valores de RU e aprovisionar um contentor para processar o total de unidades de pedido em todos os pedidos. Também pode aumentar verticalmente ou horizontalmente o débito do seu contentor conforme as necessidades da sua aplicação evoluem. Para obter mais informações sobre unidades de pedido e para ajudar a determinar o seu contentor necessita, veja [estimar as necessidades de débito](request-units.md#estimating-throughput-needs) e tente a [Calculadora de débito](https://www.documentdb.com/capacityplanner). O termo *contentor* aqui refere-se para se refere a uma coleção da API de SQL, de gráficos do Graph API, coleção da API do MongoDB e tabela de API de tabela. 

### <a name="how-does-azure-cosmos-db-support-various-data-models-such-as-keyvalue-columnar-document-and-graph"></a>Como é que o Azure Cosmos DB suporta vários modelos de dados, como a chave/valor, em colunas, documentos e gráficos?

Chaves/valores (tabela), em colunas, de documentos e dados de gráficos modelos são todos suportados nativamente devido a ARS (atoms, registos e sequências) de design que o Azure Cosmos DB baseia-se. Atoms, registos e sequências de podem ser facilmente mapeadas e projetadas para vários modelos de dados. As APIs para um subconjunto de modelos estão diretamente disponível agora (SQL, MongoDB, tabela e Graph APIs) e outras pessoas específicas para modelos de dados adicionais vão estar disponíveis no futuro.

O Azure Cosmos DB tem um esquema agnóstico indexação motor capaz de indexar automaticamente todos os dados que ingere sem necessidade de qualquer esquema ou índices secundários do desenvolvedor. O mecanismo conta com um conjunto de layouts de lógica de índice (invertida, em colunas, árvore) que desacoplar o esquema de armazenamento do índice e subsistemas de processamento de consultas. O cosmos DB também tem a capacidade de suporte a um conjunto de APIs e protocolos de transmissão de forma extensível e transformá-los com eficiência para o modelo de dados do core (1) e os layouts de lógica de índice (2) tornando exclusivamente capaz de suportar vários modelos de dados de forma nativa.

### <a name="is-azure-cosmos-db-hipaa-compliant"></a>HIPAA do Azure Cosmos DB está em conformidade?
Sim, o Azure Cosmos DB está em conformidade com HIPAA. A HIPAA estabelece requisitos para a utilização, divulgação e salvaguarda das informações médicas pessoalmente identificáveis. Para obter mais informações, consulte o [Centro de Fidedignidade da Microsoft](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA).

### <a name="what-are-the-storage-limits-of-azure-cosmos-db"></a>Quais são os limites de armazenamento do Azure Cosmos DB?
Não existe nenhum limite para a quantidade total de dados que um contentor pode armazenar no Azure Cosmos DB.

### <a name="what-are-the-throughput-limits-of-azure-cosmos-db"></a>Quais são os limites de débito do Azure Cosmos DB?
Não existe nenhum limite para a quantidade total de débito que pode oferecer suporte a um contentor no Azure Cosmos DB. A idéia-chave é distribuir a carga de trabalho aproximadamente uniformemente entre um número grande o suficiente de chaves de partição.

### <a name="are-direct-and-gateway-connectivity-modes-encrypted-"></a>Direct e o Gateway de modos de conectividade são encriptados? 
Sim ambos os modos são sempre totalmente encriptados. 

### <a name="how-much-does-azure-cosmos-db-cost"></a>Quanto custa o Azure Cosmos DB?
Para obter detalhes, consulte a [os detalhes dos preços do Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/) página. Os custos de utilização do Azure Cosmos DB são determinados pelo número de contentores aprovisionados, o número de horas, os contentores estiveram online e o débito aprovisionado para cada contentor. O termo *contentores* aqui refere-se para a coleção da API de SQL, de gráficos do Graph API, coleção da API do MongoDB e tabelas de API de tabela. 

### <a name="is-a-free-account-available"></a>É uma conta gratuita disponível?
Sim, pode inscrever-se para uma conta de tempo limitado sem custos, sem compromisso. Para se inscrever, visite [Experimente o Azure Cosmos DB gratuitamente](https://azure.microsoft.com/try/cosmosdb/) ou leia mais no [experimentar o Azure Cosmos DB FAQ](#try-cosmos-db).

Se estiver familiarizado com o Azure, pode inscrever-se para obter um [conta gratuita do Azure](https://azure.microsoft.com/free/), que dá-lhe 30 dias e um crédito para experimentar todos os serviços do Azure. Se tiver uma subscrição do Visual Studio, também é elegíveis para [gratuita créditos do Azure](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) para utilizar em qualquer serviço do Azure. 

Também pode utilizar o [emulador do Azure Cosmos DB](local-emulator.md) para desenvolver e testar seu aplicativo localmente para obter gratuitamente, sem criar uma subscrição do Azure. Quando estiver satisfeito com o funcionamento da sua aplicação no Emulador do Azure Cosmos DB, pode mudar e começar a utilizar uma conta do Azure Cosmos DB na cloud.

### <a name="how-can-i-get-additional-help-with-azure-cosmos-db"></a>Como posso obter ajuda adicional com o Azure Cosmos DB?

Para fazer uma pergunta técnica, pode publicar para um destes dois pergunta e responder a fóruns:
* [Fórum do MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurecosmosdb)
* [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-cosmosdb). Estouro de pilha é melhor para questões de programação. Certifique-se de que é a sua pergunta [no tópico](https://stackoverflow.com/help/on-topic) e [fornecer tantos detalhes quanto possível, fazer a pergunta clara e answerable](https://stackoverflow.com/help/how-to-ask). 

Para pedir novas funcionalidades, crie um novo pedido no [Uservoice](https://feedback.azure.com/forums/263030-azure-cosmos-db).

Para corrigir um problema na sua conta, crie um [pedido de suporte](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) no portal do Azure.

Outras perguntas que podem ser submetidas para a equipe da [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com); no entanto, isso não é um alias de suporte técnico. 


## <a id="try-cosmos-db"></a>Experimente as subscrições do Azure Cosmos DB

Agora pode Desfrute da experiência do Azure Cosmos DB por tempo limitado sem uma subscrição, sem encargos e compromissos. Para se inscrever para uma subscrição do Azure do Cosmos DB, aceda a [Experimente o Azure Cosmos DB gratuitamente](https://azure.microsoft.com/try/cosmosdb/). Esta subscrição está separada do [avaliação gratuita do Azure](https://azure.microsoft.com/free/)e pode ser utilizado para além de uma versão de avaliação gratuita do Azure ou um Azure subscrição paga. 

Try Azure Cosmos DB subscrições são apresentadas no portal do Azure a seguir outras subscrições associadas à sua ID de utilizador. 

As seguintes condições aplicam-se a subscrições do Azure do Cosmos DB:

* Um contêiner por subscrição para contas SQL, Gremlin (Graph API) e tabela.
* Até 3 coleções por subscrição para contas do MongoDB.
* 10 GB de capacidade de armazenamento.
* A replicação global está disponível na seguinte [regiões do Azure](https://azure.microsoft.com/regions/): E.U.A. Central, Europa do Norte e Sudeste asiático
* Débito máximo de 5 mil RU/s.
* Subscrições expirem após 24 horas e podem ser estendidas até um máximo de total de 48 horas.
* Pedidos de suporte do Azure não não possível criar contas de experimentar o Azure Cosmos DB No entanto, o suporte é fornecido para os subscritores com planos de suporte existentes. 

## <a name="set-up-azure-cosmos-db"></a>Configurar o Azure Cosmos DB
### <a name="how-do-i-sign-up-for-azure-cosmos-db"></a>Como me inscrevo para o Azure Cosmos DB?
O Azure Cosmos DB está disponível no portal do Azure. Em primeiro lugar, inscreva-se uma subscrição do Azure. Depois que tiver se inscrito, pode adicionar uma API de SQL, Graph API, API de tabela, API de MongoDB ou conta da API de Cassandra para a sua subscrição do Azure.

### <a name="what-is-a-master-key"></a>O que é uma chave mestra?
Uma chave mestra é um token de segurança para aceder a todos os recursos de uma conta. Indivíduos com a chave tem de leitura e escrita a todos os recursos na conta de base de dados. Tenha cuidado ao distribuir chaves mestras. A chave mestra principal e a chave mestra secundária estão disponíveis no **chaves** painel da [portal do Azure][azure-portal]. Para obter mais informações sobre chaves, consulte [ver, copiar e chaves de acesso de voltar a gerar](manage-account.md#keys).

### <a name="what-are-the-regions-that-preferredlocations-can-be-set-to"></a>Quais são as regiões que PreferredLocations pode ser definido como? 
O valor de PreferredLocations pode ser definido como qualquer uma das regiões do Azure em que o Cosmos DB está disponível. Para obter uma lista de regiões disponíveis, consulte [regiões do Azure](https://azure.microsoft.com/regions/).

### <a name="is-there-anything-i-should-be-aware-of-when-distributing-data-across-the-world-via-the-azure-datacenters"></a>Existe algo que deve ter conhecimento ao distribuir dados em todo o mundo por meio dos datacenters do Azure? 
O Azure Cosmos DB está presente em todas as regiões do Azure, conforme especificado no [regiões do Azure](https://azure.microsoft.com/regions/) página. Como é o serviço principal, todos os datacenters novos tem uma presença do Azure Cosmos DB. 

Quando define uma região, lembre-se de que o Azure Cosmos DB respeita clouds soberanas e Governo. Ou seja, se criar uma conta num [garante a soberania: região](https://azure.microsoft.com/global-infrastructure/), não é possível replicar tudo isso [região garante a soberania:](https://azure.microsoft.com/global-infrastructure/). Da mesma forma, não é possível ativar a replicação em outros locais soberanas de uma conta externa. 

### <a name="is-it-possible-to-switch-from-container-level-throughput-provisioning-to-database-level-throughput-provisioning-or-vice-versa"></a>É possível alternar de débito de nível de contêiner de aprovisionamento para o aprovisionamento de débito de nível de base de dados? Ou vice versa

Contentor e o aprovisionamento de débito de nível de base de dados são ofertas distintas e alterar entre qualquer um destes requerer a migrar dados de origem para destino. Que significa que precisa criar uma nova base de dados ou uma nova coleção e, em seguida, migrar dados, utilizando [biblioteca de executor em massa](bulk-executor-overview.md) ou [do Azure Data Factory](../data-factory/connector-azure-cosmos-db.md).

### <a name="how-do-i-create-fixed-collection-with-partition-key"></a>Como posso criar coleção fixa com chave de partição

Atualmente pode criar coleção com um débito de chave de partição utilizando a [CreatePartitionedCollection](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/CollectionManagement/Program.cs#L118) método do .net SDK ou utilizando o [CLI do Azure](https://docs.microsoft.com/cli/azure/cosmosdb/collection?view=azure-cli-latest#az-cosmosdb-collection-create). A criação de uma coleção fixa através do portal do Azure não está atualmente suportado.  

## <a name="sql-api"></a>SQL API

### <a name="how-do-i-start-developing-against-the-sql-api"></a>Como iniciar a programação com a API de SQL?
Primeiro, deve inscrever para uma subscrição do Azure. Depois de se inscrever para uma subscrição do Azure, pode adicionar um contentor de API de SQL para a sua subscrição do Azure. Para obter instruções sobre como adicionar uma conta do Azure Cosmos DB, veja [criar uma conta de base de dados do Azure Cosmos DB](create-sql-api-dotnet.md#create-account). 

[SDKs](sql-api-sdk-dotnet.md) estão disponíveis para .NET, Python, node. js, JavaScript e Java. Os programadores também podem utilizar o [RESTful HTTP APIs](/rest/api/cosmos-db/) para interagir com recursos do Azure Cosmos DB a partir de várias plataformas e linguagens.

### <a name="can-i-access-some-ready-made-samples-to-get-a-head-start"></a>Posso aceder a alguns exemplos prontas a utilizar para obter um ponto de partida?
Exemplos para a API de SQL [.NET](sql-api-dotnet-samples.md), [Java](https://github.com/Azure/azure-documentdb-java), [node. js](sql-api-nodejs-samples.md), e [Python](sql-api-python-samples.md) os SDKs estão disponíveis no GitHub.


### <a name="does-the-sql-api-database-support-schema-free-data"></a>A base de dados do SQL API suporta dados sem esquema?
Sim, a API de SQL permite que os aplicativos armazenem documentos JSON arbitrários sem definições de esquema ou sugestões. Os dados estão imediatamente disponíveis para consulta através da interface de consulta de SQL do Azure Cosmos DB.  

### <a name="does-the-sql-api-support-acid-transactions"></a>A API de SQL suporta transações ACID?
Sim, a API de SQL suporta transações entre documentos expressadas como procedimentos armazenados do JavaScript e acionadores. Transações estão confinadas a uma única partição dentro de cada contentor e executadas com semântica ACID como "tudo ou nada," isolado de outros pedidos de utilizador e de código em execução simultânea. Se forem geradas exceções através da execução do lado do servidor de código de aplicação do JavaScript, toda a transação é revertida. Para obter mais informações sobre transações, consulte [transações de programa de banco de dados](programming.md#database-program-transactions).

### <a name="what-is-a-container"></a>O que é um contentor?
Um contêiner é um grupo de documentos e a respetiva lógica da aplicação associada JavaScript. Um contentor é uma entidade faturável, onde o [custo](performance-levels.md) é determinada pela taxa de transferência e utilizado de armazenamento. Os contentores podem abranger uma ou mais partições ou servidores e podem ser dimensionado para processar volumes praticamente ilimitados de armazenamento ou débito. 

* Para contas SQL e a API do MongoDB, um contentor é mapeado para uma coleção. 
* Para contas de Cassandra e API de tabela, um contentor mapeia para uma tabela. 
* Para contas de API do Gremlin, um contentor é mapeado para um gráfico. 

Os contentores também são as entidades de faturação para o Azure Cosmos DB. Cada contentor é cobrado à hora, com base no débito aprovisionado e espaço de armazenamento utilizado. Para obter mais informações, consulte [preços do Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/). 

### <a name="how-do-i-create-a-database"></a>Como crio uma base de dados?
Pode criar bases de dados com o [portal do Azure](https://portal.azure.com), conforme descrito na [adicionar uma coleção](create-sql-api-dotnet.md#create-collection), um do [SDKs do Azure Cosmos DB](sql-api-sdk-dotnet.md), ou o [REST APIs](/rest/api/cosmos-db/). 

### <a name="how-do-i-set-up-users-and-permissions"></a>Como posso configurar utilizadores e permissões?
Pode criar utilizadores e permissões ao utilizar uma da [SDKs de API do Cosmos DB](sql-api-sdk-dotnet.md) ou o [REST APIs](/rest/api/cosmos-db/).  

### <a name="does-the-sql-api-support-sql"></a>A API de SQL suporta SQL?
A linguagem de consulta SQL suportada por contas da API de SQL é um subconjunto avançado da funcionalidade de consulta que é suportado pelo SQL Server. A linguagem de consulta de SQL do Azure Cosmos DB fornece operadores avançados de hierárquicos e relacionais e extensibilidade através de funções com base em JavaScript, definidas pelo utilizador (UDFs). A gramática JSON permite-lhe modelar documentos JSON como árvores connosco com nome, o que são utilizados pelas técnicas de indexação automática do Azure Cosmos DB e o dialeto de consulta SQL do Azure Cosmos DB. Para obter informações sobre como utilizar a gramática SQL, consulte a [consulta SQL] [ query] artigo.

### <a name="does-the-sql-api-support-sql-aggregation-functions"></a>A API de SQL suporta as funções de agregação de SQL?
A API do SQL oferece suporte a agregação de baixa latência em qualquer escala através de funções de agregação `COUNT`, `MIN`, `MAX`, `AVG`, e `SUM` via a gramática SQL. Para obter mais informações, consulte [funções de agregação](sql-api-sql-query.md#Aggregates).

### <a name="how-does-the-sql-api-provide-concurrency"></a>Como a API de SQL fornece simultaneidade?
A API de SQL suporta o controlo de simultaneidade otimista (OCC) através de etiquetas de entidade HTTP ou ETags. Cada recurso de API de SQL tem uma ETag e ETag está definido no servidor sempre que um documento é atualizado. O cabeçalho ETag e o valor atual estão incluídos em todas as mensagens de resposta. ETags pode ser utilizadas com o cabeçalho If-Match para permitir que o servidor decidir se um recurso deve ser atualizado. O valor If-Match é o valor de ETag para ser verificado com base. Se o valor de ETag coincidir com o valor de ETag de servidor, o recurso for atualizado. Se a ETag já não está disponível, o servidor rejeita a operação com um "HTTP 412 Falha de pré-condição" código de resposta. Em seguida, o cliente obtém novamente o recurso para adquirir o valor de ETag atual para o recurso. Além disso, as ETags pode ser utilizadas com o cabeçalho If-None-Match para determinar se um novamente obter de um recurso é necessário.

Para utilizar a simultaneidade otimista no .NET, utilize o [AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx) classe. Para obter um exemplo de .NET, veja [Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs) no exemplo DocumentManagement no GitHub.

### <a name="how-do-i-perform-transactions-in-the-sql-api"></a>Como posso efetuar transações na API de SQL?
A API do SQL oferece suporte a transações de linguagem integrada através de acionadores e procedimentos armazenados do JavaScript. Todas as operações de base de dados no interior de scripts são executadas em isolamento do instantâneo. Se é uma coleção de partição única, a execução tem como escopo o conjunto. Se a coleção é particionada, a execução é confinada a documentos com o mesmo valor de chave de partição dentro da coleção. Um instantâneo das versões do documento (ETags) é tirado no início da transação e consolidado apenas se o script for concluído com êxito. Se o JavaScript emitir um erro, a transação é revertida. Para obter mais informações, consulte [programação de JavaScript do lado do servidor para o Azure Cosmos DB](programming.md).

### <a name="how-can-i-bulk-insert-documents-into-cosmos-db"></a>Como pode eu inserção em massa documentos para o Cosmos DB?
Pode inserção em massa documentos para o Azure Cosmos DB em uma das seguintes formas:

* A ferramenta de executor em massa, conforme descrito em [biblioteca de .NET de executor do Using em massa](bulk-executor-dot-net.md) e [biblioteca de Java de executor do Using em massa](bulk-executor-java.md)
* A ferramenta de migração de dados, conforme descrito em [ferramenta de migração de bases de dados para o Azure Cosmos DB](import-data.md).
* Procedimentos armazenados, conforme descrito em [programação de JavaScript do lado do servidor para o Azure Cosmos DB](programming.md).

### <a name="i-have-setup-my-container-to-use-lazy-indexing-i-see-that-my-queries-do-not-return-expected-results"></a>Eu tenho a configuração do meu contêiner para usar indexação lento, vejo que as minhas consultas não devolver os resultados esperados. 
Conforme explicado na seção de indexação, a indexação lento pode resultar nesse comportamento. Deve sempre usar indexação consistente para todos os aplicativos. 


### <a name="does-the-sql-api-support-resource-link-caching"></a>É que o cache do link de recurso do suporte de API de SQL?
Sim, como o Azure Cosmos DB é um serviço RESTful, ligações de recursos são imutáveis e podem ser colocadas em cache. Os clientes de API de SQL podem especificar um cabeçalho de "If-None-Match" para leituras em relação a qualquer documento de tipo de recurso ou uma coleção e, em seguida, atualizar suas cópias locais depois da versão de servidor foi alterada.

### <a name="is-a-local-instance-of-sql-api-available"></a>Uma instância local da SQL API está disponível?
Sim. O [emulador do Azure Cosmos DB](local-emulator.md) fornece uma emulação de alta fidelidade do serviço do Cosmos DB. Ele oferece suporte a funcionalidade que é idêntica ao Azure Cosmos DB, incluindo suporte para criar e consultar documentos JSON, aprovisionamento e dimensionar coleções e a execução de procedimentos armazenados e acionadores. Pode desenvolver e testar aplicações com o emulador do Azure Cosmos DB e implementá-las para o Azure à escala global, fazendo uma única alteração de configuração para o ponto final de ligação para o Azure Cosmos DB.

### <a name="why-are-long-floating-point-values-in-a-document-rounded-when-viewed-from-data-explorer-in-the-portal"></a>Por que são valores longa vírgula flutuante num documento arredondado quando visualizado a partir do Explorador de dados no portal. 
Esta é a limitação do JavaScript. JavaScript usa números de formato de vírgula flutuante de dupla precisão, conforme especificado na IEEE 754 e com segurança, ela pode representar números entre-(253 - 1) e 253-1 (ou seja, 9007199254740991) apenas.

### <a name="where-are-permissions-allowed-in-the-object-hierarchy"></a>Em que as permissões são permitidas na hierarquia do objeto?

Criação de permissões com ResourceTokens é permitido no nível do contentor e respetivos descendentes (como documentos, anexos). Isso implica que tentar criar uma permissão no banco de dados ou o nível de uma conta não é atualmente permitido.


## <a name="mongodb-api"></a>API do MongoDB
### <a name="what-is-the-azure-cosmos-db-api-for-mongodb"></a>O que é a API do Azure Cosmos DB para o MongoDB?
A API do Azure Cosmos DB para o MongoDB é uma camada de compatibilidade que permite que aplicativos facilmente e transparente comunicar com o motor de base de dados do Azure Cosmos DB nativo, utilizando existente, com suporte da Comunidade APIs do MongoDB Apache e drivers. Os programadores podem agora utilizar cadeias de ferramentas do MongoDB existentes e as habilidades necessários para criar aplicações que tiram partido do Azure Cosmos DB. Os desenvolvedores se beneficiam com as capacidades exclusivas do Azure Cosmos DB, que incluem a manutenção de cópia de segurança, indexa automaticamente, contratos de nível de serviço com suporte financeiro (SLAs) e assim por diante.

### <a name="how-do-i-connect-to-my-api-for-mongodb-database"></a>Como posso ligar a minha API da base de dados do MongoDB?
A forma mais rápida para ligar à API do Azure Cosmos DB do MongoDB é head ao longo para o [portal do Azure](https://portal.azure.com). Aceda à sua conta e, em seguida, no menu de navegação esquerdo, clique em **início rápido**. Início rápido é a melhor forma de obter trechos de código para ligar à base de dados. 

O Azure Cosmos DB impõe requisitos de segurança rigorosas e padrões. As contas do Azure Cosmos DB requerem autenticação e comunicação segura através de SSL, por isso, certifique-se de que utilize TLSv1.2.

Para obter mais informações, consulte [ligar à sua API para a base de dados MongoDB](connect-mongodb-account.md).

### <a name="are-there-additional-error-codes-for-an-api-for-mongodb-database"></a>Existem códigos de erro adicionais para uma API para a base de dados MongoDB?
Além de códigos de erro comuns do MongoDB, a API do MongoDB tem seus próprios códigos de erro específicas:


| Erro               | Código  | Descrição  | Solução  |
|---------------------|-------|--------------|-----------|
| TooManyRequests     | 16500 | O número total de unidades de pedido consumidas excedeu a taxa de unidade de pedido de aprovisionamento para a coleção e otimizado. | Considere aumentar o débito atribuído a um contentor ou um conjunto de contentores do Azure portal ou a repetir novamente. |
| ExceededMemoryLimit | 16501 | Como um serviço de multi-inquilino, a operação excedeu a alocação de memória do cliente. | Reduza o âmbito da operação por meio de critérios de consulta mais restritivas ou contacte o suporte do [portal do Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). <br><br>Exemplo:  *&nbsp; &nbsp; &nbsp; &nbsp;db.getCollection('users').aggregate ([<br> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;{$match: {nome: "Andy"}}, <br> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;{$sort: {idade: -1} }<br>&nbsp;&nbsp;&nbsp;&nbsp;])*) |

## <a id="table"></a>API de tabela

### <a name="how-can-i-use-the-table-api-offering"></a>Como posso utilizar a oferta de API de tabela? 
A API de tabela do Azure Cosmos DB está disponível na [portal do Azure][azure-portal]. Primeiro, deve inscrever para uma subscrição do Azure. Depois que tiver se inscrito, pode adicionar uma conta de API de tabela do Azure Cosmos DB para a sua subscrição do Azure e, em seguida, adicionar tabelas à sua conta. 

Pode encontrar os idiomas suportados e associados inícios Rápidos no [introdução à API de tabela do Azure Cosmos DB](table-introduction.md).

### <a name="do-i-need-a-new-sdk-to-use-the-table-api"></a>É necessário um novo SDK para utilizar a API de tabela? 
Não, existentes armazenamento que SDKs deverá continuar a funcionar. Contudo, recomenda-se que sempre obtém os SDKs mais recentes para o melhor suporte e em muitos casos desempenho superior. Ver a lista de idiomas de disponíveis no [introdução à API de tabela do Azure Cosmos DB](table-introduction.md).

### <a name="where-is-table-api-not-identical-with-azure-table-storage-behavior"></a>Em que a API de tabela não é idêntico com o comportamento de armazenamento de tabelas do Azure?
Existem algumas diferenças de comportamento que os utilizadores provenientes do armazenamento de tabelas do Azure que pretendem criar tabelas com a API de tabela do Azure Cosmos DB devem ter conhecimento:

* API de tabela do Azure Cosmos DB utiliza um modelo de capacidade de reserva para garantir o desempenho garantido, mas isso significa que paga a capacidade de assim que a tabela é criada, mesmo que a capacidade não está a ser utilizada. Com o armazenamento de tabelas do Azure apenas paga por capacidade, na verdade, é utilizada. Esta ajuda a explicar por que o API de tabela pode oferecer a que leitura dos 10 ms e a 15 ms escrevem SLA no percentil 99, enquanto o armazenamento de tabelas do Azure oferece um SLA de segundo 10. Mas, como conseqüência, com tabelas de API de tabela, até mesmo esvaziar tabelas sem todos os pedidos, custo dinheiro para garantir que a capacidade está disponível para processar pedidos aos mesmos no SLA oferecidos pelo Azure Cosmos DB.
* Resultados devolvidos pela API de tabela não são ordenados por ordem de chave de linha/chave de partição como estão no armazenamento de tabelas do Azure.
* Chaves de linha só é possível até 255 bytes
* Lotes só podem conter até 2 MB
* Não é atualmente suportada CORS
* Os nomes das tabelas no armazenamento de tabelas do Azure não diferenciam maiúsculas de minúsculas, mas estão na API de tabela do Azure Cosmos DB
* Alguns dos formatos internos do Azure Cosmos DB para informações de codificação, como campos binários, atualmente não são tão eficientes como um poderia desejar. Portanto, isso pode causar limitações inesperadas no tamanho dos dados. Por exemplo, atualmente é não foi possível usar a completa 1 MB de uma entidade de tabela para armazenar dados binários, uma vez que a codificação aumenta o tamanho dos dados.
* Nome de propriedade da entidade "Id" não suportado atualmente
* TableQuery TakeCount não se limita a 1000

Em termos da API REST, existem várias opções de consulta/pontos finais que não são suportados pela API de tabela do Azure Cosmos DB:
| Método (s) de REST | Opção de consulta/ponto final de REST | URLs de documento | Explicação |
| ------------| ------------- | ---------- | ----------- |
| OBTER, COLOCAR | /?restype=service@comp=properties| [Definir as propriedades do serviço de tabela](https://docs.microsoft.com/rest/api/storageservices/set-table-service-properties) e [obter as propriedades do serviço de tabela](https://docs.microsoft.com/rest/api/storageservices/get-table-service-properties) | Este ponto final é utilizado para definir regras CORS, configuração de análise de armazenamento e definições de registo. CORS não é atualmente suportado e o registo e análise são processadas forma diferente no Azure Cosmos DB que as tabelas de armazenamento do Azure |
| OPÇÕES | / < nome de tabela-recursos > | [Pedido de tabela CORS de simulação](https://docs.microsoft.com/rest/api/storageservices/preflight-table-request) | Esta é a parte do CORS que o Azure Cosmos DB não suporta atualmente. |
| GET | /?restype=service@comp=stats | [Obter estatísticas do serviço tabela](https://docs.microsoft.com/rest/api/storageservices/get-table-service-stats) | Fornece informações como rapidamente dados está a replicar entre primárias e secundárias. Isso não é necessário no Cosmos DB como a replicação é a parte das escritas. |
| OBTER, COLOCAR | /MyTable? comp = acl | [Obter a tabela de ACL](https://docs.microsoft.com/rest/api/storageservices/get-table-acl) e [Definir tabela de ACL](https://docs.microsoft.com/rest/api/storageservices/set-table-acl) | Isso obtém e define as políticas de acesso armazenadas utilizadas para gerir assinaturas de acesso partilhado (SAS). Embora seja suportada SAS, eles são estabelecidos e gerenciados de forma diferente. |

Além disso API de tabela do Azure Cosmos DB suporta apenas o formato JSON, não ATOM.

Enquanto o Azure Cosmos DB suporta partilhado assinaturas de acesso (SAS) existem determinadas políticas, que ele não dá suporte, especificamente aqueles relacionados com operações de gestão, como o direito de criar novas tabelas.

Para o SDK de .NET em particular, existem algumas classes e métodos que o Azure Cosmos DB não suporta atualmente.

| Classe | Método não suportado |
|-------|-------- |
| CloudTableClient | \*ServiceProperties * |
|                  | \*ServiceStats * |
| CloudTable | SetPermissions * |
|            | GetPermissions * |
| TableServiceContext | * (essa classe, na verdade, foi preterida) |
| TableServiceEntity | " " |
| TableServiceExtensions | " " |
| TableServiceQuery | " " |

Se qualquer uma dessas diferenças são um problema para o seu projeto, contacte [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com) e informe-nos.

### <a name="how-do-i-provide-feedback-about-the-sdk-or-bugs"></a>Como posso enviar comentários sobre o SDK ou bugs?
Pode partilhar os seus comentários em qualquer uma das seguintes formas:

* [Uservoice](https://feedback.azure.com/forums/599062-azure-cosmos-db-table-api)
* [Fórum do MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurecosmosdb)
* [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-cosmosdb). Estouro de pilha é melhor para questões de programação. Certifique-se de que é a sua pergunta [no tópico](https://stackoverflow.com/help/on-topic) e [fornecer tantos detalhes quanto possível, fazer a pergunta clara e answerable](https://stackoverflow.com/help/how-to-ask).

### <a name="what-is-the-connection-string-that-i-need-to-use-to-connect-to-the-table-api"></a>O que é a cadeia de ligação que preciso para utilizar para ligar para a API de tabela?
A cadeia de ligação é:
```
DefaultEndpointsProtocol=https;AccountName=<AccountNamefromCosmos DB;AccountKey=<FromKeysPaneofCosmosDB>;TableEndpoint=https://<AccountName>.table.cosmosdb.azure.com
```
Pode obter a cadeia de ligação a partir da página de cadeia de ligação no portal do Azure. 

### <a name="how-do-i-override-the-config-settings-for-the-request-options-in-the-net-sdk-for-the-table-api"></a>Como posso substituir as definições de configuração para as opções de pedido no SDK do .NET para a API de tabela?
Para obter informações sobre definições de configuração, consulte [capacidades do Azure Cosmos DB](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities). Algumas definições são processadas no método CreateCloudTableClient e outros via o App. config na seção appSettings no aplicativo cliente.

### <a name="are-there-any-changes-for-customers-who-are-using-the-existing-azure-table-storage-sdks"></a>Existem alterações para os clientes que estão a utilizar o armazenamento de tabelas do Azure SDKs existente?
Nenhum. Não há nenhuma alteração para clientes de novas ou existentes que estão a utilizar o armazenamento de tabelas do Azure existente SDKs. 

### <a name="how-do-i-view-table-data-that-is-stored-in-azure-cosmos-db-for-use-with-the-table-api"></a>Como faço para visualizar dados de tabela que são armazenados no Azure Cosmos DB para utilização com a API de tabela? 
Pode utilizar o portal do Azure para navegar pelos dados. Também pode utilizar o código de API de tabela ou as ferramentas que mencionei na resposta seguinte. 

### <a name="which-tools-work-with-the-table-api"></a>Quais são as ferramentas funcionam com a API de tabela? 
Pode utilizar o [Explorador de armazenamento do Azure](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer).

Ferramentas com a flexibilidade de tirar uma cadeia de ligação no formato especificado anteriormente podem suportar a nova API de tabela. É fornecida uma lista de ferramentas de tabela no [ferramentas de cliente de armazenamento do Azure](../storage/common/storage-explorers.md) página. 

### <a name="is-the-concurrency-on-operations-controlled"></a>É a simultaneidade em operações controladas?
Sim, a simultaneidade otimista é fornecida através da utilização do mecanismo ETag. 

### <a name="is-the-odata-query-model-supported-for-entities"></a>O modelo de consulta de OData é suportado para entidades? 
Sim, a API de tabela oferece suporte a consulta de OData e consulta LINQ. 

### <a name="can-i-connect-to-azure-table-storage-and-azure-cosmos-db-table-api-side-by-side-in-the-same-application"></a>Posso ligar ao armazenamento de tabelas do Azure e a API de tabela do Azure Cosmos DB lado a lado no mesmo aplicativo? 
Sim, pode ligar através da criação de duas instâncias separadas da CloudTableClient, cada um apontando para o seu próprio URI através da cadeia de ligação.

### <a name="how-do-i-migrate-an-existing-azure-table-storage-application-to-this-offering"></a>Como posso migrar um aplicativo de armazenamento de tabelas do Azure existente a esta oferta?
[AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy) e o [ferramenta de migração de dados do Azure Cosmos DB](import-data.md) são ambas suportada.

### <a name="how-is-expansion-of-the-storage-size-done-for-this-service-if-for-example-i-start-with-n-gb-of-data-and-my-data-will-grow-to-1-tb-over-time"></a>Como é a expansão do tamanho do armazenamento feito por este serviço se, por exemplo, começar com *n* GB de dados e meus dados, irá aumentar a 1 TB ao longo do tempo? 
O Azure Cosmos DB foi concebido para fornecer armazenamento ilimitado através da utilização de dimensionamento horizontal. O serviço pode monitorizar e aumentar o seu armazenamento com eficiência. 

### <a name="how-do-i-monitor-the-table-api-offering"></a>Como posso monitorizar a oferta de API de tabela?
Pode usar a API de tabela **métricas** painel para monitorizar pedidos e a utilização de armazenamento. 

### <a name="how-do-i-calculate-the-throughput-i-require"></a>Como posso calcular o débito de que preciso?
Pode usar o avaliador de capacidade para calcular o TableThroughput necessários para as operações. Para obter mais informações, consulte [unidades de pedido de estimativa e o armazenamento de dados](https://www.documentdb.com/capacityplanner). Em geral, pode representar a entidade como JSON e fornecer os números das suas operações. 

### <a name="can-i-use-the-table-api-sdk-locally-with-the-emulator"></a>Posso utilizar o SDK de API de tabela localmente com o emulador?
Neste momento, não.

### <a name="can-my-existing-application-work-with-the-table-api"></a>Meu aplicativo existente pode trabalhar com a API de tabela? 
Sim, é suportada a mesma API.

### <a name="do-i-need-to-migrate-my-existing-azure-table-storage-applications-to-the-sdk-if-i-do-not-want-to-use-the-table-api-features"></a>É necessário migrar as minhas aplicações de armazenamento de tabelas do Azure existentes para o SDK se não pretendo utilizar as funcionalidades de API de tabela?
Não, pode criar e utilizar recursos de armazenamento de tabelas do Azure existentes sem interrupção de qualquer tipo. No entanto, se não utilizar a API de tabela, não pode se beneficiar de índices automática, a opção de consistência adicionais ou distribuição global. 

### <a name="how-do-i-add-replication-of-the-data-in-the-table-api-across-multiple-regions-of-azure"></a>Como adicionar replicação dos dados na API de tabela entre várias regiões do Azure?
Pode utilizar o portal Azure Cosmos DB [as definições de replicação global](tutorial-global-distribution-sql-api.md#portal) adicionar regiões que são adequadas para a sua aplicação. Para desenvolver uma aplicação distribuída globalmente, deverá ainda adicionar o seu aplicativo com as informações de PreferredLocation definidas para a região local para o fornecimento de baixa latência de leitura. 

### <a name="how-do-i-change-the-primary-write-region-for-the-account-in-the-table-api"></a>Como posso alterar a região de escrita principal para a conta na API de tabela?
Pode utilizar o painel do portal do Azure Cosmos DB replicação global para adicionar uma região e, em seguida, efetuar a ativação pós-falha para a região necessária. Para obter instruções, consulte [desenvolver com contas do Azure Cosmos DB de várias regiões](regional-failover.md). 

### <a name="how-do-i-configure-my-preferred-read-regions-for-low-latency-when-i-distribute-my-data"></a>Como posso configurar meu regiões de leitura preferenciais de baixa latência quando posso distribuir meus dados? 
Para ajudar a ler a partir da localização local, utilize a chave de PreferredLocation no ficheiro App. config. Para aplicativos existentes, a API de tabela emite um erro se LocationMode está definido. Remova esse código, uma vez que a API de tabela são recolhidos por esta informação do ficheiro App. config. Para obter mais informações, consulte [capacidades do Azure Cosmos DB](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities).

### <a name="how-should-i-think-about-consistency-levels-in-the-table-api"></a>Como deve pensar sobre os níveis de consistência na API de tabela? 
O Azure Cosmos DB fornece bem manter compromissos entre consistência, disponibilidade e latência. O Azure Cosmos DB oferece cinco níveis de consistência aos desenvolvedores de API de tabela, para que possa escolher o modelo de consistência direita no nível da tabela e fazer pedidos individuais ao consultar os dados. Quando se liga um cliente, pode especificar um nível de consistência. Pode alterar o nível via o argumento de consistencyLevel de CreateCloudTableClient. 

A API de tabela fornece leituras de latência baixa com "ler suas próprias escritas" com consistência de limitado vinculada como predefinição. Para obter mais informações, consulte [níveis de consistência](consistency-levels.md). 

Por predefinição, o armazenamento de tabelas do Azure fornece consistência forte dentro de uma região e consistência Eventual nas localizações secundárias. 

### <a name="does-azure-cosmos-db-table-api-offer-more-consistency-levels-than-azure-table-storage"></a>API de tabela do Azure Cosmos DB oferece mais níveis de consistência que o armazenamento de tabelas do Azure?
Sim, para obter informações sobre como se beneficiar da natureza distribuída do Azure Cosmos DB, consulte [níveis de consistência](consistency-levels.md). Porque garantias são fornecidas para os níveis de consistência, pode usá-los com confiança. Para obter mais informações, consulte [capacidades do Azure Cosmos DB](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities).

### <a name="when-global-distribution-is-enabled-how-long-does-it-take-to-replicate-the-data"></a>Quando a distribuição global está ativada, o tempo que demora a replicar os dados?
O Azure Cosmos DB consolida os dados de maneira duradoura na região local e envia os dados para outras regiões imediatamente em questão de milissegundos. Esta replicação é depende apenas o tempo de ida e volta (RTT) do Data Center. Para saber mais sobre a capacidade de distribuição global do Azure Cosmos DB, veja [do Azure Cosmos DB: um serviço de base de dados globalmente distribuída no Azure](distribute-data-globally.md).

### <a name="can-the-read-request-consistency-level-be-changed"></a>O nível de consistência de solicitação de leitura pode ser alterado?
Com o Azure Cosmos DB, pode definir o nível de consistência ao nível do contentor (na tabela). Com o SDK de .NET, pode alterar o nível ao fornecer o valor de chave de TableConsistencyLevel no ficheiro App. config. Os valores possíveis são: forte, estagnação limitada, sessão, prefixo consistente e Eventual. Para obter mais informações, consulte [níveis de consistência Sincronizáveis dados no Azure Cosmos DB](consistency-levels.md). A idéia-chave é que não é possível definir a consistência de pedido nível em mais do que a definição da tabela. Por exemplo, não é possível definir o nível de consistência para a tabela na Eventual e o nível de consistência pedido em forte. 

### <a name="how-does-the-table-api-handle-failover-if-a-region-goes-down"></a>Como a API de tabela com ativação pós-falha se uma região ficar inativo? 
A API de tabela tira partido da plataforma de distribuição global do Azure Cosmos DB. Para garantir que seu aplicativo pode tolerar o período de indisponibilidade do Centro de dados, ative, pelo menos, uma região de mais para a conta no portal do Azure Cosmos DB [desenvolver com contas do Azure Cosmos DB de várias regiões](regional-failover.md). Pode definir a prioridade da região com o portal [desenvolver com contas do Azure Cosmos DB de várias regiões](regional-failover.md). 

Pode adicionar tantas regiões à medida que pretende para a conta e controla onde ele pode efetuar a ativação pós-falha, fornecendo uma prioridade de ativação pós-falha. É claro, para utilizar a base de dados, terá de fornecer um aplicativo aqui também. Ao fazê-lo, os seus clientes não ocorrerá um período de indisponibilidade. O [SDK mais recente do cliente .NET](table-sdk-dotnet.md) é automática homing mas outros SDKs não são. Ou seja, pode detetar a região que está inativo e automaticamente a ativação pós-falha para a nova região.

### <a name="is-the-table-api-enabled-for-backups"></a>A API de tabela está ativada para cópias de segurança?
Sim, a API de tabela tira partido da plataforma do Azure Cosmos DB para cópias de segurança. As cópias de segurança são efetuadas automaticamente. Para obter mais informações, consulte [Online backup e restauração com o Azure Cosmos DB](online-backup-and-restore.md).

 
### <a name="does-the-table-api-index-all-attributes-of-an-entity-by-default"></a>A API de tabela de índice todos os atributos de uma entidade por predefinição?
Sim, todos os atributos de uma entidade são indexados por predefinição. Para obter mais informações, consulte [do Azure Cosmos DB: políticas de indexação](indexing-policies.md). 

### <a name="does-this-mean-i-do-not-have-to-create-multiple-indexes-to-satisfy-the-queries"></a>Isso significa que não é necessário criar vários índices para satisfazer as consultas? 
Sim, a API de tabela do Azure Cosmos DB fornece uma indexação automática de todos os atributos sem qualquer definição de esquema. Esta automatização isenta os desenvolvedores concentrar-se a aplicação em vez de criação de índices e gestão. Para obter mais informações, consulte [do Azure Cosmos DB: políticas de indexação](indexing-policies.md).

### <a name="can-i-change-the-indexing-policy"></a>Pode alterar a política de indexação?
Sim, pode alterar a política de indexação ao fornecer a definição do índice. Para obter mais informações, consulte [capacidades do Azure Cosmos DB](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities). Terá de codificar e as definições de escape adequadamente. 

para os SDKs que não da .NET a política de indexação só pode ser definida no portal em **Data Explorer**, navegue para a tabela específica que pretende alterar e, em seguida, vá para o **dimensionamento e definições**-> política de indexação, Faça a alteração pretendida e, em seguida **guardar**.

Do SDK do .NET que pode ser submetido no ficheiro App. config:
```
{
  "indexingMode": "consistent",
  "automatic": true,
  "includedPaths": [
    {
      "path": "/somepath",
      "indexes": [
        {
          "kind": "Range",
          "dataType": "Number",
          "precision": -1
        },
        {
          "kind": "Range",
          "dataType": "String",
          "precision": -1
        } 
      ]
    }
  ],
  "excludedPaths": 
[
 {
      "path": "/anotherpath"
 }
]
}
```

### <a name="azure-cosmos-db-as-a-platform-seems-to-have-lot-of-capabilities-such-as-sorting-aggregates-hierarchy-and-other-functionality-will-you-be-adding-these-capabilities-to-the-table-api"></a>O Azure Cosmos DB como uma plataforma parece ter vários recursos, tais como a ordenação, agregações, hierarquia e outras funcionalidades. Adicionará estas capacidades para a API de tabela? 
A API de tabela fornece a mesma funcionalidade de consulta como armazenamento de tabelas do Azure. Também suporta ordenação, agregações, consultas geoespaciais, hierarquias e um vasto conjunto de funções incorporadas. Forneceremos uma funcionalidade adicional na API de tabela numa atualização futura do serviço. Para obter mais informações, consulte [consultas SQL](sql-api-sql-query.md).
 
### <a name="when-should-i-change-tablethroughput-for-the-table-api"></a>Quando posso alterar TableThroughput para a API de tabela?
Deve alterar TableThroughput quando qualquer uma das seguintes condições se aplicar:
* Estiver a efetuar uma extração, transformação e carregamento (ETL) de dados ou, se pretender carregar uma grande quantidade de dados num curto período de tempo. 
* Precisa de mais débito do contentor ou a partir de um conjunto de contentores em back-end. Por exemplo, verá que a taxa de transferência utilizada é mais do que o débito aprovisionado e obter limitados. Para obter mais informações, consulte [definir débito de contentores do Azure Cosmos DB](set-throughput.md).

### <a name="can-i-scale-up-or-scale-down-the-throughput-of-my-table-api-table"></a>Pode aumentar ou reduzir verticalmente o débito da minha tabela de API de tabela? 
Sim, pode utilizar o painel de escala do portal do Azure Cosmos DB para dimensionar o débito. Para obter mais informações, consulte [definir débito](set-throughput.md).

### <a name="is-a-default-tablethroughput-set-for-newly-provisioned-tables"></a>É um padrão que tablethroughput definida para tabelas recentemente aprovisionadas?
Sim, se não se substituam TableThroughput via App. config e não utilizar um contentor previamente criado no Azure Cosmos DB, o serviço cria uma tabela com um débito de 400.
 
### <a name="is-there-any-change-of-pricing-for-existing-customers-of-the-azure-table-storage-service"></a>Existe qualquer alteração de preços para os clientes existentes do serviço de armazenamento de tabelas do Azure?
Nenhum. Não há nenhuma alteração no preço para os clientes existentes do armazenamento de tabelas do Azure. 

### <a name="how-is-the-price-calculated-for-the-table-api"></a>Como é calculado o preço para a API de tabela? 
O preço depende do TableThroughput alocado. 

### <a name="how-do-i-handle-any-rate-limiting-on-the-tables-in-table-api-offering"></a>Como com qualquer limitação de velocidade em tabelas na oferta de API de tabela? 
Se a taxa de pedido exceder a capacidade do débito aprovisionado para o contentor subjacente ou um conjunto de contentores, obterá um erro e o SDK repetirá a chamada ao aplicar a política de repetição.

### <a name="why-do-i-need-to-choose-a-throughput-apart-from-partitionkey-and-rowkey-to-take-advantage-of-the-table-api-offering-of-azure-cosmos-db"></a>Por que motivo é necessário escolher uma taxa de transferência para além dos PartitionKey e RowKey para tirar partido da oferta de API de tabela do Azure Cosmos DB?
O Azure Cosmos DB define uma taxa de transferência predefinido para o seu contentor, se não fornecer um no ficheiro App. config ou através do portal. 

O Azure Cosmos DB fornece garantias de desempenho e latência, com limites operação. Essa garantia é possível quando o mecanismo pode impor a governação nas operações do inquilino. Definição TableThroughput garante que obtenha o débito garantido e a latência, uma vez que a plataforma esta capacidade de reserva-se e garantias de êxito operacional. 

Ao utilizar a especificação de débito, pode de forma elástica alterá-lo para se beneficiar de sazonalidade do seu aplicativo, satisfazer as necessidades de débito e reduzir os custos.

### <a name="azure-table-storage-has-been-very-inexpensive-for-me-because-i-pay-only-to-store-the-data-and-i-rarely-query-the-azure-cosmos-db-table-api-offering-seems-to-be-charging-me-even-though-i-have-not-performed-a-single-transaction-or-stored-anything-can-you-please-explain"></a>Armazenamento de tabelas do Azure tem sido muito baixo custo para mim, porque eu presto apenas armazenar os dados e eu raramente consulta. A oferta de API de tabela do Azure Cosmos DB parece ser cobrar-me, apesar de não realizada uma única transação ou armazenados nada. Pode me explicar?

O Azure Cosmos DB foi concebido para ser um sistema distribuído globalmente com base no SLA com garantias de disponibilidade, latência e débito. Quando a reservar débito no Azure Cosmos DB, é garantido, ao contrário da taxa de transferência de outros sistemas. O Azure Cosmos DB fornece capacidades adicionais que os clientes solicitaram, como índices secundários e distribuição global.  

### <a name="i-never-get-a-quota-full-notification-indicating-that-a-partition-is-full-when-i-ingest-data-into-azure-table-storage-with-the-table-api-i-do-get-this-message-is-this-offering-limiting-me-and-forcing-me-to-change-my-existing-application"></a>Notificação de nunca entendi, uma quota total"(que indica que uma partição está cheio) quando eu ingerir dados no armazenamento de tabelas do Azure. Com a API de tabela, eu receber esta mensagem. Isso oferece limitar-me e me forçar a alterar a minha aplicação existente?

O Azure Cosmos DB é um sistema com base no SLA que oferece um dimensionamento ilimitado, sem garantias de latência, débito, disponibilidade e consistência. Para garantir um desempenho premium garantida, certifique-se de que o tamanho dos dados e índice são gerenciável e dimensionável. O limite de 10 GB no número de entidades ou itens por chave de partição é garantir que podemos fornecer um excelente desempenho de pesquisa e consulta. Para garantir que seu aplicativo se dimensione bem, mesmo para o armazenamento do Azure, recomendamos que *não* criar uma partição de acesso frequente ao armazenar todas as informações de uma partição e consultá-lo. 

### <a name="so-partitionkey-and-rowkey-are-still-required-with-the-table-api"></a>Então, PartitionKey e RowKey ainda são necessárias com a API de tabela? 
Sim. Como a área de superfície da API de tabela é semelhante à que o SDK de armazenamento de tabelas do Azure, a chave de partição fornece uma maneira eficiente para distribuir os dados. A chave de linha é única dentro dessa partição. A chave de linha tem de estar presente e não pode ser nula como no SDK do padrão. O comprimento do RowKey é 255 bytes e o comprimento de PartitionKey é de 1 KB. 

### <a name="what-are-the-error-messages-for-the-table-api"></a>Quais são as mensagens de erro para a API de tabela?
API de tabela do Azure Cosmos DB e armazenamento de tabelas do Azure utilizam os SDKs do mesmo, por isso, a maioria dos erros será o mesmo.

### <a name="why-do-i-get-throttled-when-i-try-to-create-lot-of-tables-one-after-another-in-the-table-api"></a>Por que, é limitado ao tentar criar um após o outro muitas tabelas na API de tabela?
O Azure Cosmos DB é um sistema com base no SLA que fornece a latência, débito, disponibilidade e garantias de consistência. Como é um sistema aprovisionado, se a reserva recursos para garantir a esses requisitos. A taxa de rápida da criação de tabelas é detectada e limitada. Recomendamos que examinar a taxa de criação de tabelas e reduzi-lo para menos de 5 por minuto. Lembre-se de que a API de tabela é um sistema aprovisionado. No momento em que aprovisioná-lo, começará a pagá-la. 

## <a name="graph-api"></a>Graph API
### <a name="how-can-i-apply-the-functionality-of-graph-api-to-azure-cosmos-db"></a>Como posso aplicar a funcionalidade do Graph API para o Azure Cosmos DB?
Pode utilizar uma biblioteca de extensão para aplicar a funcionalidade do Graph API. Esta biblioteca é chamada de gráficos do Microsoft Azure e está disponível no [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Graphs). 

### <a name="it-looks-like-you-support-the-gremlin-graph-traversal-language-do-you-plan-to-add-more-forms-of-query"></a>Parece suporta o idioma de passagem de gráfico do Gremlin. Planeja adicionar mais formas de consulta?
Sim, planeamos adicionar outros mecanismos para consulta no futuro. 

### <a name="how-can-i-use-the-new-graph-api-offering"></a>Como posso utilizar a nova oferta de Graph API? 
Para começar a utilizar, execute o [Graph API](../cosmos-db/create-graph-dotnet.md) de início rápido.


## <a id="cassandra"></a> API para Cassandra

### <a name="what-is-the-protocol-version-supported-in-the-private-preview-is-there-a-plan-to-support-other-protocols"></a>O que é a versão do protocolo suportada na pré-visualização privada? Existe um plano para suportar outros protocolos?
API Apache Cassandra para o Azure Cosmos DB suporta atualmente a versão CQL 4. Se tiver comentários sobre o suporte de outros protocolos, fale conosco [comentários do uservoice](https://feedback.azure.com/forums/263030-azure-cosmos-db) ou envie um e-mail para [ askcosmosdbcassandra@microsoft.com ](mailto:askcosmosdbcassandra@microsoft.com). 

### <a name="why-is-choosing-a-throughput-for-a-table-a-requirement"></a>Por isso que escolhe um débito de uma tabela de um requisito?
O Azure Cosmos DB define o débito predefinido para o contentor com base em onde criar a tabela de - portal ou CQL. O Azure Cosmos DB fornece garantias de desempenho e latência, com limites operação. Essa garantia é possível quando o mecanismo pode impor a governação nas operações do inquilino. Débito de definição garante que obtenha o débito garantido e a latência, uma vez que a plataforma esta capacidade de reserva-se e garantias de êxito da operação. Elástica pode alterar o débito para se beneficiar de sazonalidade do seu aplicativo e reduzir os custos.

O conceito de débito é explicado no [unidades de pedido no Azure Cosmos DB](request-units.md) artigo. O débito de uma tabela é distribuído entre as partições físicas subjacentes igualmente.  

### <a name="what-is-the-default-rus-of-table-when-created-through-cql-what-if-i-need-to-change-it"></a>O que é a predefinição de RU/s da tabela quando criado por meio de CQL? E se eu precisar de alterá-la?
O Azure Cosmos DB utiliza unidades de pedido por segundo (RU/s) como uma moeda para fornecer o débito. As tabelas criadas por meio de CQL ter 400 RU. Pode alterar o RU do portal. 

CQL
```
CREATE TABLE keyspaceName.tablename (user_id int PRIMARY KEY, lastname text) WITH cosmosdb_provisioned_throughput=1200
```

.NET
```csharp
int provisionedThroughput = 400;
var simpleStatement = new SimpleStatement($"CREATE TABLE {keyspaceName}.{tableName} (user_id int PRIMARY KEY, lastname text)");
var outgoingPayload = new Dictionary<string, byte[]>();
outgoingPayload["cosmosdb_provisioned_throughput"] = Encoding.UTF8.GetBytes(provisionedThroughput.ToString());
simpleStatement.SetOutgoingPayload(outgoingPayload); 
``` 

### <a name="what-happens-when-throughput-is-exceeded"></a>O que acontece quando o débito é excedido? 
O Azure Cosmos DB fornece garantias de desempenho e latência, com limites operação. Essa garantia é possível quando o mecanismo pode impor a governação nas operações do inquilino. Isso é possível com base na definição a taxa de transferência, o que garante que obtenha o débito garantido e a latência, uma vez que esta capacidade de reserva-se de plataforma e garantias de êxito da operação. Quando o excedem esta capacidade receberá a mensagem de erro sobrecarregado indicando que sua capacidade foi excedida. 0x1001 sobrecarregado: não é possível processar o pedido porque "Taxa de pedidos é grande". Neste juncture é essencial para saber quais operações e volume faz com que este problema. Pode ter uma idéia sobre a capacidade consumida exceder a capacidade aprovisionada com métricas no portal. Em seguida, certifique-se de capacidade é quase consumida igualmente todas as partições subjacente. Se vir a maior parte da taxa de transferência é consumido por partição, tem distorção da carga de trabalho. 

As métricas estão disponíveis que mostram como débito é utilizado durante horas, dias e por sete dias, entre partições ou em forma agregada. Para obter mais informações, consulte [monitorização e depuração com métricas na Azure Cosmos DB](use-metrics.md).

Os registos de diagnóstico são explicados no [registo de diagnósticos do Azure Cosmos DB](logging.md) artigo.

### <a name="does-the-primary-key-map-to-the-partition-key-concept-of-azure-cosmos-db"></a>É que o mapa de chave primário para o conceito de chave de partição do Azure Cosmos DB?
Sim, a chave de partição é utilizada para colocar a entidade na localização correta. No Azure Cosmos DB é utilizado para localizar a partição lógica direito que está armazenada numa partição física. O conceito de criação de partições é explicado bem a [particionar e dimensionar no Azure Cosmos DB](partition-data.md) artigo. O essencial take distância aqui é que uma partição lógica não deve exceder o limite de 10 GB hoje mesmo. 

### <a name="what-happens-when-i-get-a-quota-full-notification-indicating-that-a-partition-is-full"></a>O que acontece quando recebo uma quota total"notificação indicando que uma partição é completa?
O Azure Cosmos DB é um sistema com base no SLA que oferece um dimensionamento ilimitado, sem garantias de latência, débito, disponibilidade e consistência. API do Cassandra também permite que o armazenamento ilimitada de dados. Este armazenamento ilimitado baseia-se no dimensionamento horizontal de dados com a criação de partições como o conceito-chave. O conceito de criação de partições é explicado bem a [particionar e dimensionar no Azure Cosmos DB](partition-data.md) artigo.

O limite de 10 GB no número de entidades ou itens por partição lógica, que deve seguir. Para garantir que seu aplicativo se dimensione bem, recomendamos que *não* criar uma partição de acesso frequente ao armazenar todas as informações de uma partição e consultá-lo. Este erro pode apenas vêm se dados é desviada, que é ter muitos dados para a chave de partição - ou seja, mais de 10 GB. Pode encontrar a distribuição de dados com o portal de armazenamento. Forma para corrigir este erro é recrete a tabela e escolha um principal granular (chave de partição), que permite uma melhor distribuição dos dados.

### <a name="is-it-possible-to-use-cassandra-api-as-key-value-store-with-millions-or-billions-of-individual-partition-keys"></a>É possível utilizar a Cassandra API como arquivo de chave-valor com milhões ou bilhões de chaves de partição individual?
O Azure Cosmos DB pode armazenar dados ilimitados ao aumentar horizontalmente o armazenamento. Este facto é independente da taxa de transferência. Sim sempre apenas pode utilizar a API de Cassandra para armazenar e obter chaves/valores ao especificar a chave primária/partição correta. Essas chaves individuais obtém sua própria partição lógica e permanecem acima da partição física sem problemas. 

### <a name="is-it-possible-to-create-multiple-tables-with-apache-cassandra-api-of-azure-cosmos-db"></a>É possível criar várias tabelas com o Apache Cassandra API do Azure Cosmos DB?
Sim, é possível crete várias tabelas com a Apache Cassandra API. Cada uma dessas tabelas é tratada como a unidade de débito e armazenamento. 

### <a name="is-it-possible-to-create-multiple-tables-in-succession"></a>É possível criar várias tabelas numa sucessão?
O Azure Cosmos DB é regido por recursos de sistema para atividades de plano de dados e controle. Contentores, como coleções, as tabelas são entidades de tempo de execução que são aprovisionadas para devido a capacidade de débito. A criação destes contentores numa sucessão rápida não é esperada de atividade e limitada. Se tiver de testes, que lista/criar tabelas imediatamente - volte a tentar espaçá-los.

### <a name="what-is-maximum-number-of-tables-which-can-be-created"></a>O que é o número máximo de tabelas que podem ser criados?
Não tem qualquer limite físico no número de tabelas, envie um e-mail para [ askcosmosdbcassandra@microsoft.com ](mailto:askcosmosdbcassandra@microsoft.com) se tiver um número muito grande de tabelas (em que o tamanho de constante total exceder 10 TB de dados) que têm de ser criado a partir habitual 10s ou 100s. 

### <a name="what-is-the-maximum--of-keyspace-which-we-can-create"></a>O que é o n. º máximo de keyspace que podemos criar? 
Não há físico limite no número de keyspaces, pois são contentores de metadados, envie um e-mail para [ askcosmosdbcassandra@microsoft.com ](mailto:askcosmosdbcassandra@microsoft.com) se tiver um número muito grande de keyspaces por algum motivo. 

### <a name="is-it-possible-to-bring-in-lot-of-data-after-starting-from-normal-table"></a>É possível colocar em grande quantidade de dados após o início da tabela normal? 
A capacidade de armazenamento é gerida automaticamente e aumenta à medida que emitir no mais dados. Portanto, é possível importar com confiança quantidade de dados que necessita sem gerenciamento e provisionamento de nós, etc. 

### <a name="is-it-possible-to-supply-yaml-file-settings-to-configure-apache-casssandra-api-of-azure-cosmos-db-behavior"></a>É possível fornecer definições de ficheiro yaml para configurar o comportamento de Apache Casssandra API do Azure Cosmos DB?
O Apache Cassandra API do Azure Cosmos DB é um serviço de plataforma. Ele fornece compatibilidade de nível de protocolo para a execução de operações. Distância oculta a complexidade de gestão, monitorização e configuração. Como um desenvolvedor/utilizador não é necessário se preocupar sobre a disponibilidade, marcas para exclusão, cache de chave, cache de linha, filtro de bloom e inúmeras outras definições. Focos de Apache Cassandra API do Azure Cosmos DB no fornecimento de ler e gravar o desempenho de que necessita sem a sobrecarga de configuração e gestão.

### <a name="will-apache-cassandra-api-for-azure-cosmos-db-support-node-additioncluster-statusnode-status-commands"></a>API Apache Cassandra para o Azure Cosmos DB irá suportar a comandos de estado do nó adição/cluster/nó Estado?
Apache Cassandra API é um serviço de plataforma que facilita o planeamento de capacidade, a responder as demandas de elasticidade de débito e armazenamento muito simples. Com o Azure Cosmos DB, aprovisionar o débito de que precisa. Em seguida, pode dimensioná-lo e diminuir a qualquer número de vezes por dia sem se preocupar em Adicionar/eliminar nós ou gerenciá-los. Isso implica que não é necessário utilizar o nó, a ferramenta de gerenciamento de cluster também. 

### <a name="what-happens-with-respect-to-various-config-settings-for-keyspace-creation-like-simplenetwork"></a>O que acontece em relação a várias definições de configuração para criação de keyspace como simples/rede?
O Azure Cosmos DB oferece distribuição global prontos a utilizar por motivos de baixa latência e de disponibilidade. Não é necessário para réplicas de configuração etc. Todas as escritas são sempre maneira duradoura quórum confirmada em qualquer região em que escreve e fornece garantias de desempenho.  

### <a name="what-happens-with-respect-to-various-settings-for-table-metadata-like-bloom-filter-caching-read-repair-change-gcgrace-compression-memtableflushperiod-etc"></a>O que acontece em relação a várias definições para os metadados de tabela, como o filtro de bloom, colocação em cache, leia a alteração de reparação, gc_grace, compactação memtable_flush_period etc?
O Azure Cosmos DB fornece desempenho para leituras e gravações e débito sem necessidade de alterar qualquer das definições de configuração e manipular acidentalmente.  

### <a name="is-time-to-live-ttl-supported-for-cassandra-tables"></a>Time-to-live (TTL) é suportada para tabelas de Cassandra? 
Sim, o TTL é suportada. 

### <a name="is-it-possible-to-monitor-node-status-replica-status-gc-and-os-parameters-earlier-with-various-tools-what-needs-to-be-monitored-now"></a>É possível monitorizar o estado do nó, o estado da réplica, o gc e parâmetros de sistema operacional mais cedo com várias ferramentas? O que precisa ser monitorado agora?
O Azure Cosmos DB é um serviço de plataforma que o ajuda a aumentar a produtividade e não se preocupar sobre como gerir e monitorizar a infraestrutura. Terá de tomar conta de débito que está disponível em métricas de portais para encontrar se obter limitados e aumentar ou diminuir esse débito. Monitor [SLAs](monitor-accounts.md).
Uso [métricas](use-metrics.md) utilização [registos de diagnóstico](logging.md).

### <a name="which-client-sdks-can-work-with-apache-cassandra-api-of-azure-cosmos-db"></a>Os SDKs de cliente podem trabalhar com o Apache Cassandra API do Azure Cosmos DB?
No cliente do SDK de pré-visualização privada, Apache Cassandra drivers que utilizam CQLv3 foram utilizados para programas de cliente. Se tiver outros controladores que pode utilizar a ou se está a enfrentar problemas, envie um e-mail para [ askcosmosdbcassandra@microsoft.com ](mailto:askcosmosdbcassandra@microsoft.com). 

### <a name="is-composite-partition-key-supported"></a>Chave de partição composta é suportada?
Sim, pode utilizar a sintaxe normal para criar a chave de partição compostos. 

### <a name="can-i-use-sstable-loader-for-data-loading"></a>Posso utilizar o carregador de sstable para carregamento de dados?
Não, durante a pré-visualização sstable carregador não é suportada. 

### <a name="can-an-on-premises-cassandra-cluster-be-paired-with-azure-cosmos-dbs-apache-cassandra-api"></a>Um cluster do cassandra no local pode ser emparelhado com o Azure Cosmos DB Apache Cassandra API?
No presente para o Azure Cosmos DB tem uma experiência otimizada para o ambiente de cloud sem sobrecarga de operações. Se necessitar de emparelhamento, envie um e-mail para [ askcosmosdbcassandra@microsoft.com ](mailto:askcosmosdbcassandra@microsoft.com) com uma descrição do seu cenário.

### <a name="does-cassandra-api-provide-full-backups"></a>O Cassandra API fornece cópias de segurança completas? 
O Azure Cosmos DB fornece dois gratuitas completas cópias de segurança criadas em intervalos de quatro horas hoje em todas as APIs. Isto garante que não é necessário configurar um agendamento de cópia de segurança etc. Se pretender modificar a retenção e a frequência, envie um e-mail para [ askcosmosdbcassandra@microsoft.com ](mailto:askcosmosdbcassandra@microsoft.com) ou emitir um pedido de suporte. São fornecidas informações sobre a capacidade de cópia de segurança no [cópia de segurança online automática e restauro com o Azure Cosmos DB](online-backup-and-restore.md) artigo. 

### <a name="how-does-the-cassandra-api-account-handle-failover-if-a-region-goes-down"></a>Como a conta do Cassandra API com ativação pós-falha se uma região ficar inativo? 
A API de Cassandra do Azure Cosmos DB empresta a partir da plataforma de distribuição global do Azure Cosmos DB. Para garantir que seu aplicativo pode tolerar o período de indisponibilidade do Centro de dados, ative, pelo menos, uma região de mais para a conta no portal do Azure Cosmos DB [desenvolver com contas do Azure Cosmos DB de várias regiões](regional-failover.md). Pode definir a prioridade da região com o portal [desenvolver com contas do Azure Cosmos DB de várias regiões](regional-failover.md). 

Pode adicionar tantas regiões à medida que pretende para a conta e controla onde ele pode efetuar a ativação pós-falha, fornecendo uma prioridade de ativação pós-falha. Para utilizar a base de dados, terá de fornecer um aplicativo aqui também. Ao fazê-lo, os seus clientes não ocorrerá um período de indisponibilidade. 

### <a name="does-the-apache-cassandra-api-index-all-attributes-of-an-entity-by-default"></a>Apache Cassandra API indexar todos os atributos de uma entidade por predefinição?
Sim, todos os atributos de uma entidade são indexados por predefinição pelo Azure Cosmos DB. Para obter mais informações, consulte [do Azure Cosmos DB: políticas de indexação](indexing-policies.md). Obtém os benefícios de desempenho garantido indexação consistente e durável quórum consolidada escreve sempre. 

### <a name="does-this-mean-i-do-not-have-to-create-multiple-indexes-to-satisfy-the-queries"></a>Isso significa que não é necessário criar vários índices para satisfazer as consultas? 
Sim, o Azure Cosmos DB fornece uma indexação automática de todos os atributos sem qualquer definição de esquema. Esta automatização isenta os desenvolvedores concentrar-se a aplicação em vez de criação de índices e gestão. Para obter mais informações, consulte [do Azure Cosmos DB: políticas de indexação](indexing-policies.md).

### <a name="can-i-use-the-new-cassandra-api-sdk-locally-with-the-emulator"></a>Posso utilizar o novo SDK de API de Cassandra localmente com o emulador?
Planeamos suportar esta capacidade no futuro. 

### <a name="azure-cosmos-db-as-a-platform-seems-to-have-lot-of-capabilities-such-as-changefeed-and-other-functionality-will-these-capabilities-be-added-to-the-cassandra-api"></a>O Azure Cosmos DB como uma plataforma parece ter vários recursos, tais como changefeed e outras funcionalidades. Serão adicionados esses recursos para a API de Cassandra? 
Apache Cassandra API fornece a mesma funcionalidade CQL Apache Cassandra. Planeamos examinar a viabilidade de oferecer suporte a vários recursos no futuro.

### <a name="feature-x-of-regular-cassandra-api-is-not-working-as-today-where-can-the-feedback-be-provided"></a>O recurso x da API de Cassandra regular não está a funcionar como hoje em dia, onde podem os comentários ser fornecidos?
Fornecer feedback através de [comentários do uservoice](https://feedback.azure.com/forums/263030-azure-cosmos-db).

[azure-portal]: https://portal.azure.com
[query]: sql-api-sql-query.md
