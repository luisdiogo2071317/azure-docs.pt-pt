---
title: Conceitos e modelo de recursos do Azure Cosmos DB | Documentos da Microsoft
description: Saiba mais sobre o modelo hierárquico do Azure Cosmos DB de bases de dados, coleções, função definida pelo utilizador (UDF), documentos, permissões para gerir recursos e muito mais.
keywords: Modelo hierárquico, cosmosdb, do azure, Microsoft azure
services: cosmos-db
author: rafats
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: rafats
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3ba02e7760d7400b5168a902415f16c4b276b3a7
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43287958"
---
# <a name="azure-cosmos-db-hierarchical-resource-model-and-core-concepts"></a>Conceitos-chave e modelo de recursos hierárquicos do Azure Cosmos DB

As entidades de base de dados que gerencia o Azure Cosmos DB são denominadas **recursos**. Cada recurso é identificado exclusivamente por uma URI lógica. Pode interagir com os recursos através do verbo HTTP padrão, os cabeçalhos de solicitação/resposta e códigos de estado. 

Este artigo responde às seguintes perguntas:

* O que é o modelo de recursos do Azure Cosmos DB?
* Quais são sistema definidos recursos em vez de recursos definidos pelo utilizador?
* Como atender a um recurso?
* Como trabalhar com coleções?
* Como posso trabalhar com procedimentos armazenados, acionadores e funções definidas pelo utilizador (UDFs)?

## <a name="hierarchical-resource-model"></a>Modelo hierárquico de recursos
Como o diagrama seguinte ilustra, o Azure Cosmos DB hierárquica **modelo de recursos** consiste em conjuntos de recursos numa conta de base de dados, cada endereçável por meio de um URI de lógico e estável. Um conjunto de recursos são referidas como um **feed** neste artigo. 

> [!NOTE]
> O Azure Cosmos DB oferece um protocolo TCP altamente eficiente, o que é também RESTful no seu modelo de comunicação, disponível através da [API cliente .NET de SQL](sql-api-sdk-dotnet.md).
> 
> 

![Modelo de recursos hierárquicos do Azure Cosmos DB][1]  
**Modelo hierárquico de recursos**   

Para começar a trabalhar com recursos, deve [criar uma conta de base de dados](create-sql-api-dotnet.md) com a sua subscrição do Azure. Uma conta de base de dados pode ser composta por um conjunto de **bases de dados**, cada um contendo vários **coleções**, cada do que por sua vez contêm **procedimentos armazenados, acionadores, UDFs, documentos e relacionados anexos**. Uma base de dados também tem associados **usuários**, cada um com um conjunto de **permissões** para aceder a coleções, procedimentos armazenados, acionadores, UDFs, documentos ou anexos. Enquanto as bases de dados, os utilizadores, permissões e as coleções são recursos definidos pelo sistema com esquemas bem conhecidos, documentos e anexos contêm conteúdos JSON arbitrários, definidas pelo utilizador.  

| Recurso | Descrição |
| --- | --- |
| Conta de base de dados |Uma conta de base de dados está associada um conjunto de bases de dados e uma quantidade fixa de armazenamento de BLOBs para anexos. Pode criar uma ou mais contas de base de dados com a sua subscrição do Azure. Para obter mais informações, visite o [página de preços](https://azure.microsoft.com/pricing/details/cosmos-db/). |
| Base de Dados |Uma base de dados é um contentor lógico do armazenamento de documentos particionado em coleções. Também é um contentor de utilizadores. |
| Utilizador |O espaço de nomes lógico para definir o âmbito das permissões. |
| Permissão |Um token de autorização associado a um utilizador para o acesso a um recurso específico. |
| Coleção |Uma coleção é um contentor de documentos JSON e a lógica da aplicação associada JavaScript. As coleções podem abranger uma ou mais partições/servidores e podem dimensionar para processar volumes praticamente ilimitados de armazenamento ou débito. |
| Procedimento Armazenado |Lógica da aplicação escrita em JavaScript, que é registrado com uma coleção e executado ao nível das transações dentro do motor de base de dados. |
| Acionador |Lógica da aplicação escrita em JavaScript executado antes ou depois de qualquer uma inserção, substituir ou eliminar a operação. |
| UDF |Lógica de aplicação escrita em JavaScript. UDFs permitem-lhe modelar um operador de consulta personalizada e, deste modo, expandir a principal linguagem de consulta da API de SQL. |
| Documento |Conteúdos JSON (arbitrários) definidos pelo utilizador. Por predefinição, nenhum esquema tem de ser definida nem fazerem índices secundários precisam ser fornecidos para todos os documentos adicionados a uma coleção. |
| Anexo |Um anexo é um documento especial que contém referências e metadados associados para blob/suporte de dados externo. O desenvolvedor pode optar por ter o blob gerido pelo Cosmos DB ou armazená-lo com um provedor de serviços de blob externo, como o OneDrive, Dropbox, etc. |

## <a name="system-vs-user-defined-resources"></a>Sistema versus recursos definidos pelo utilizador
Recursos, tais como contas de base de dados, bases de dados, coleções, os utilizadores, permissões, procedimentos armazenados, acionadores e UDFs - têm um esquema fixo e são chamados de recursos do sistema. Por outro lado, os recursos, como documentos e anexos têm sem restrições no esquema e são exemplos de recursos definidos pelo utilizador. No Cosmos DB, o sistema e os recursos definidos pelo usuário são representados e geridos como compatíveis com o padrão JSON. Todos os recursos, sistema ou utilizador definido, tem as seguintes propriedades comuns:

> [!NOTE]
> Todas as propriedades de gerada pelo sistema num recurso são prefixadas com um caráter de sublinhado (_) em sua representação do JSON.
> 
> 

<table>
    <tbody>
        <tr>
            <td valign="top"><p><strong>Propriedade</strong></p></td>
            <td valign="top"><p><strong>Utilizador definível ou gerada pelo sistema?</strong></p></td>
            <td valign="top"><p><strong>Objetivo</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>_rid</p></td>
            <td valign="top"><p>Gerada pelo sistema</p></td>
            <td valign="top"><p>Sistema gerado, o identificador exclusivo e hierárquico do recurso</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_etag</p></td>
            <td valign="top"><p>Gerada pelo sistema</p></td>
            <td valign="top"><p>ETag do recurso necessário para o controlo de simultaneidade otimista</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_ts</p></td>
            <td valign="top"><p>Gerada pelo sistema</p></td>
            <td valign="top"><p>Última timestamp atualizado do recurso</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_self</p></td>
            <td valign="top"><p>Gerada pelo sistema</p></td>
            <td valign="top"><p>URI endereçável exclusivo do recurso</p></td>
        </tr>
        <tr>
            <td valign="top"><p>ID</p></td>
            <td valign="top"><p>Qualquer um dos</p></td>
            <td valign="top"><p>Definido pelo utilizador nome exclusivo do recurso (com o valor de chave de partição mesmo). Se o usuário não especificar um id, um id é gerada pelo sistema</p></td>
        </tr>
    </tbody>
</table>

### <a name="wire-representation-of-resources"></a>Durante a transmissão de representação de recursos
O cosmos DB não impõe quaisquer extensões de proprietários para as codificações de padrão ou especiais de JSON; funciona com o padrão de documentos JSON em conformidade.  

### <a name="addressing-a-resource"></a>Um recurso de endereçamento
Todos os recursos são URI endereçável. O valor do **Self** propriedade de um recurso representa o URI relativo do recurso. O formato do URI é composta pelo /\<feed\>/ {_rid} segmentos de caminho:  

| Valor da Self | Descrição |
| --- | --- |
| /dbs |Feed de bases de dados sob uma conta de base de dados |
| /dbs/{dbName} |Base de dados com um id de correspondência do valor {dbName} |
| /dbs/{dbName}/colls/ |Feed de coleções num banco de dados |
| /dbs/{dbName}/colls/{collName} |Coleção com um id de correspondência do valor {collName} |
| /dbs/{dbName}/colls/{collName}/docs |Feed de documentos numa coleção |
| /dbs/{dbName}/colls/{collName}/docs/{docId} |Documentos com um id de correspondência do valor {doc} |
| /dbs/{dbName}/users/ |Feed de usuários num banco de dados |
| /dbs/{dbName}/users/{userId} |Utilizador com um id de correspondência do valor {user} |
| /dbs/{dbName}/users/{userId}/permissions |Feed de permissões sob um utilizador |
| /dbs/{dbName}/users/{userId}/permissions/{permissionId} |Permissão com o id de correspondência do valor {permissão} |

Cada recurso tem um nome exclusivo definido pelo utilizador exposto por meio da propriedade de id. Nota: para documentos, se o usuário não especificar um id, os SDKs geram automaticamente um id exclusivo para o documento. O id é uma cadeia de caracteres de definidas pelo utilizador, até 256 carateres que seja exclusivo dentro do contexto de um recurso de pai específico. 

Cada recurso também tem um identificador de recursos hierárquicos do gerada pelo sistema (também referido como um RID), que está disponível por meio da propriedade _rid. O RID codifica toda a hierarquia de um determinado recurso e é uma representação interna conveniente utilizada para impor a integridade referencial de uma maneira distribuída. O RID é exclusivo dentro de uma conta de base de dados e é utilizado internamente pelo Cosmos DB para encaminhamento eficiente, sem a necessidade de pesquisas de partição cruzada. Os valores das propriedades _rid e o Self são representações alternativas e canônicas de um recurso. 

As APIs REST do suporte de recursos de endereçamento e encaminhamento de pedidos, o id e as propriedades de _rid.

## <a name="database-accounts"></a>Contas de base de dados
Pode aprovisionar uma ou mais contas do Cosmos DB da base de dados com a sua subscrição do Azure.

Pode criar e gerir contas de base de dados do Cosmos DB através do portal do Azure em [ http://portal.azure.com/ ](https://portal.azure.com/). Criar e gerir uma conta de base de dados requer acesso administrativo e apenas podem ser efetuada na sua subscrição do Azure. 

### <a name="database-account-properties"></a>Propriedades da conta de base de dados
Como parte do aprovisionamento e gestão de uma conta de base de dados pode configurar e leia as seguintes propriedades:  

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Nome da propriedade</strong></p></td>
            <td valign="top"><p><strong>Descrição</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Política de consistência</p></td>
            <td valign="top"><p>Defina esta propriedade para configurar o nível predefinido de consistência para todas as coleções na sua conta de base de dados. É possível substituir o nível de consistência numa base por solicitação usando o cabeçalho do pedido de [x-ms--um nível de consistência]. <p><p>Esta propriedade aplica-se apenas para o <i>recursos definidos pelo utilizador</i>. Todos os recursos definidos estão configurados para suportar leituras/consultas de sistema com consistência forte.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Chaves de autorização</p></td>
            <td valign="top"><p>As chaves primária e secundárias mestre e só de leitura que fornecem acesso administrativo para todos os recursos sob a conta de base de dados.</p></td>
        </tr>
    </tbody>
</table>

Além de aprovisionar, configurar e gerir a sua conta de base de dados do portal do Azure, pode também programaticamente criar e gerir contas de base de dados do Cosmos DB com o [APIs de REST do Azure Cosmos DB](/rest/api/cosmos-db/) , bem como o [SDKs cliente](sql-api-sdk-dotnet.md).  

## <a name="databases"></a>Bases de Dados
Uma base de dados do Cosmos DB é um contentor lógico de um ou mais coleções e os utilizadores, conforme mostrado no diagrama seguinte. Pode criar qualquer número de bases de dados sob uma conta de base de dados do Cosmos DB sujeitas a limites de oferta.  

![Modelo de hierárquico conta e coleções da base de dados][2]  
**Uma base de dados é um contentor lógico de utilizadores e de coleções**

Uma base de dados pode conter um armazenamento de documentos ilimitado particionado em coleções.

### <a name="elastic-scale-of-an-azure-cosmos-db-database"></a>Dimensionamento elástico de uma base de dados do Azure Cosmos DB
Uma base de dados do Cosmos DB é elástico por padrão – desde a alguns GB até petabytes de armazenamento de documentos SSD e débito aprovisionado. 

Ao contrário de uma base de dados num RDBMS tradicional, uma base de dados do Cosmos DB não tem um âmbito para uma única máquina. Com o Cosmos DB, como o dimensionamento de seu aplicativo precisa crescer, é possível criar mais coleções, bases de dados ou ambos. Na verdade, vários aplicativos de terceiros primeiro dentro da Microsoft têm usado do Azure Cosmos DB numa escala de consumidor através da criação de cada que contêm milhares de coleções de grandes bases de dados do Azure Cosmos DB com terabytes de armazenamento de documentos. Pode aumentar ou diminuir uma base de dados ao adicionar ou remover coleções para atender aos requisitos de escala do seu aplicativo. 

Pode criar qualquer número de coleções dentro de uma base de dados sujeitos a oferta. Cada coleção ou um conjunto de coleções (dentro de uma base de dados), tem SSD conta com armazenamento e débito aprovisionado para si, consoante a oferta selecionada.

Uma base de dados do Azure Cosmos DB também é um contentor de utilizadores. Um utilizador, em vez, é um espaço de nomes lógico para um conjunto de permissões que fornece autorização refinada e acesso a coleções, documentos e anexos.  

Como com outros recursos no modelo de recursos do Azure Cosmos DB, bases de dados podem ser criadas, substituído, eliminar, ler ou enumerado facilmente através de um a [REST APIs](/rest/api/cosmos-db/) ou qualquer um da [SDKs cliente](sql-api-sdk-dotnet.md). O Azure Cosmos DB garante a consistência forte para leitura ou consultar os metadados de um recurso de base de dados. Eliminar uma base de dados automaticamente assegura que não é possível aceder a qualquer um dos coleções ou os utilizadores contidos nela.   

## <a name="collections"></a>Coleções
Uma coleção do Cosmos DB é um contentor para os seus documentos JSON. 

### <a name="elastic-ssd-backed-document-storage"></a>Armazenamento de documentos SSD elástico
Uma coleção é intrinsecamente elástica - automaticamente aumenta e diminui à medida que adiciona ou remover documentos. Coleções são recursos lógicos e podem abranger uma ou mais partições físicas ou servidores. O número de partições atribuídas a uma coleção é determinado pelo Cosmos DB, com base no tamanho de armazenamento e o débito aprovisionado para a coleção ou um conjunto de coleções. Cada partição no Cosmos DB tem uma quantidade fixa de armazenamento SSD com cópia de segurança associado a ele e é replicada para elevada disponibilidade. Gestão de partição totalmente gerido pelo Azure Cosmos DB e não tem de escrever código complexo ou gerenciar suas partições. São coleções de cosmos DB **ilimitado** em termos de armazenamento e débito. 

### <a name="automatic-indexing-of-collections"></a>Indexação automática de coleções
O Azure Cosmos DB é um sistema de verdadeiro banco de dados sem esquema. Não assume nem requer nenhum esquema para os documentos JSON. À medida que adiciona documentos numa coleção, Azure Cosmos DB indexa automaticamente-los e estão disponíveis para consulta. Indexação automática de documentos sem a necessidade de esquema ou índices secundários, é uma capacidade essencial do Azure Cosmos DB e está ativada por técnicas de manutenção do índice otimizado para escrita, sem bloqueio e estruturada em registos. O Azure Cosmos DB oferece suporte a volume constante de gravações extremamente rápidas enquanto processa ainda consultas consistentes. Armazenamento de documentos e índice são utilizados para calcular o armazenamento consumido por cada coleção. Pode controlar os armazenamento e desempenho compensações associadas ao configurar a política de indexação para uma coleção de indexação. 

### <a name="configuring-the-indexing-policy-of-a-collection"></a>Configurar a política de indexação de uma coleção
A política de indexação de cada coleção permite-lhe efetuar o desempenho e armazenamento compromissos associados a indexação. As seguintes opções estão disponíveis para, como parte da configuração de indexação:  

* Escolha se a coleção indexa automaticamente todos os documentos ou não. Por predefinição, todos os documentos são automaticamente indexados. Pode optar por desativar a indexação automática e adicionar seletivamente apenas os documentos específicos para o índice. Por outro lado, pode seletivamente optar por excluir apenas documentos específicos. Pode conseguir isto ao definir a propriedade automática para ser true ou false no indexingPolicy de uma coleção e utilizar o cabeçalho do pedido de [x-ms-indexingdirective] ao inserir, substituir ou eliminar um documento.  
* Escolha se pretende incluir ou excluir caminhos específicos ou padrões em seus documentos do índice. Pode conseguir isto ao includedPaths de definição e excludedPaths no indexingPolicy de uma coleção, respetivamente. Também pode configurar os armazenamento e desempenho vantagens e desvantagens para consultas de intervalo e hash para padrões de caminho específico. 
* Escolha entre síncrona (em conformidade) e atualizações de índices (lento) assíncrona. Por predefinição, o índice é atualizado forma síncrona em cada inserir, substituir ou eliminar um documento à coleção. Isto permite que as consultas que respeite o mesmo nível de consistência que as leituras de documento. Enquanto o Azure Cosmos DB está escrita otimizada e oferece suporte a volumes constante de escritas de documento, juntamente com a manutenção do índice síncrona e que serve consultas consistentes, pode configurar determinadas coleções para atualizar o respetivo índice lentamente. A indexação lento aumenta ainda mais o desempenho de escrita e é ideal para cenários de ingestão em massa para principalmente as coleções de leitura intensiva.

A política de indexação pode ser alterada ao executar um PUT na coleção. Isso pode ser obtida através do [SDK de cliente](sql-api-sdk-dotnet.md), o [portal do Azure](https://portal.azure.com), ou o [REST APIs](/rest/api/cosmos-db/).

### <a name="querying-a-collection"></a>Consultar uma coleção
Os documentos dentro de uma coleção podem ter esquemas arbitrárias e pode consultar documentos dentro de uma coleção sem fornecer nenhum esquema ou índices secundários de antemão. Pode consultar a coleção com o [referência de sintaxe SQL do Azure Cosmos DB](https://msdn.microsoft.com/library/azure/dn782250.aspx), que fornece operadores avançados de hierárquicos, relacionais e espaciais e extensibilidade através de UDFs com base em JavaScript. A gramática JSON permite-lhe modelar documentos JSON como árvores com etiquetas como nós de árvores. Isso é explorado por técnicas de indexação automática da API de SQL, bem como o dialeto SQL do Azure Cosmos DB. A linguagem de consulta SQL consiste em três aspectos principais:   

1. Um pequeno conjunto de operações de consulta que mapeiam naturalmente na estrutura de árvore, incluindo consultas hierárquicas e projeções. 
2. Um subconjunto de operações relacionais, incluindo composição, filtro, projeções, agregados e associações personalizadas. 
3. Com base em JavaScript puro UDFs que funcionam com (1) e (2).  

O modelo de consulta do Azure Cosmos DB tenta encontrar um equilíbrio entre a funcionalidade, eficiência e simplicidade. O motor de base de dados do Azure Cosmos DB nativamente compila e executa as instruções de consulta SQL. Pode consultar uma coleção com o [REST APIs](/rest/api/cosmos-db/) ou qualquer um da [SDKs de cliente](sql-api-sdk-dotnet.md). O SDK de .NET vem com um provedor LINQ.

> [!TIP]
> Pode experimentar a API de SQL e executar consultas SQL no nosso conjunto de dados do [recreio de consultas](https://www.documentdb.com/sql/demo).
> 
> 

## <a name="multi-document-transactions"></a>Transações de vários documentos
Transações da base de dados fornecem um modelo de programação seguro e previsível para lidar com alterações simultâneas nos dados. RDBMS, a maneira tradicional de escrever a lógica de negócios é escrever **procedimentos armazenados** e/ou **acionadores** e enviá-lo para o servidor de base de dados para a execução transacional. No RDBMS, o Programador de aplicativos é necessária para lidar com duas linguagens de programação diferentes: 

* O idioma (por exemplo, JavaScript, Python, c#, Java, etc.) de programação de aplicações (não transacional)
* T-SQL, a linguagem de programação transacional que é executada nativamente pela base de dados

Por seu compromisso sério para JavaScript e JSON diretamente no motor de base de dados, o Azure Cosmos DB oferece um modelo de programação intuitivo para execução do JavaScript com base lógica do aplicativo diretamente nas coleções em termos de procedimentos armazenados e acionadores. Este procedimento permite que os seguintes elementos:

* Controlar a implementação eficiente de simultaneidade, recuperação, a indexação dos gráficos de objeto JSON diretamente no mecanismo de banco de dados automática
* Naturalmente expressar o fluxo de controle, variáveis de âmbito, atribuição e integração de primitivos com transações de base de dados diretamente em termos de linguagem de programação JavaScript de manipulação de exceção

A lógica de JavaScript registrada num nível de coleção, em seguida, pode emitir operações de banco de dados em documentos de determinada coleção. O Azure Cosmos DB implicitamente encapsula o JavaScript com base em procedimentos armazenados e acionadores dentro de uma transação do ambiente ACID com isolamento de instantâneo em documentos dentro de uma coleção. Durante a sua execução, se o JavaScript emitir uma exceção, toda a transação será abortada. O modelo de programação resultante é simples mas poderosa. Os desenvolvedores de JavaScript obtém um modelo de programação "durável" ao mesmo tempo ainda com construções de linguagem familiares e primitivos de biblioteca.   

A capacidade de execução do JavaScript diretamente dentro do motor de base de dados no mesmo espaço de endereço que o conjunto de memória intermédia permite com bom desempenho e a execução transacional de operações de base de dados nos documentos de uma coleção. Além disso, o motor de base de dados do Cosmos DB faz um compromisso sério para o JSON e JavaScript elimina qualquer incompatibilidade de impedância entre os sistemas de tipo de aplicativo e a base de dados.   

Depois de criar uma coleção, pode registrar procedimentos armazenados, acionadores e UDFs com uma coleção com o [REST APIs](/rest/api/cosmos-db/) ou qualquer um da [SDKs cliente](sql-api-sdk-dotnet.md). Após o registo, pode referenciar e executá-los. Considere o seguinte procedimento armazenado escrito inteiramente no JavaScript, o código a seguir utiliza dois argumentos (nome do livro e o nome de autor) e cria um novo documento, consulta um documento e, em seguida, atualiza-– tudo dentro de uma transação ACID implícita. Em qualquer momento durante a execução, se for lançada uma exceção de JavaScript, seja anulada toda a transação.

    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();        
        var collectionLink = collectionManager.getSelfLink()

        // create a new document.
        collectionManager.createDocument(collectionLink,
            {id: name, author: author},
            function(err, documentCreated) {
                if(err) throw new Error(err.message);

                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function(err, matchingDocuments) {
                        if(err) throw new Error(err.message);

                        context.getResponse().setBody(matchingDocuments.length);

                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don’t need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);   
                        }
                    })
            })
    };

O cliente pode "Enviar" a lógica de JavaScript acima para a base de dados para a execução transacional através de HTTP POST. Para obter mais informações sobre como utilizar os métodos HTTP, consulte [RESTful interações com os recursos do Azure Cosmos DB](https://msdn.microsoft.com/library/azure/mt622086.aspx). 

    client.createStoredProcedureAsync(collection._self, {id: "CRUDProc", body: businessLogic})
       .then(function(createdStoredProcedure) {
            return client.executeStoredProcedureAsync(createdStoredProcedure.resource._self,
                "NoSQL Distilled",
                "Martin Fowler");
        })
        .then(function(result) {
            console.log(result);
        },
        function(error) {
            console.log(error);
        });


Tenha em atenção que uma vez que a base de dados compreende de forma nativa JSON e JavaScript, não existe nenhum erro de correspondência do sistema de tipo, "Mapeamento OR" ou mágica de geração de código necessários.   

Procedimentos armazenados e acionadores interagirem com uma coleção e os documentos numa coleção através de um modelo de objeto bem definidos, que expõe o contexto de coleção atual.  

Podem ser criadas coleções na API de SQL, eliminado, leitura ou enumerados facilmente através de um a [REST APIs](/rest/api/cosmos-db/) ou qualquer um da [SDKs cliente](sql-api-sdk-dotnet.md). A API do SQL sempre fornece consistência forte para leitura ou consultar os metadados de uma coleção. A eliminar uma coleção automaticamente garante que não pode aceder a qualquer um dos documentos, anexos, procedimentos armazenados, acionadores e UDFs contém nela.   

## <a name="stored-procedures-triggers-and-user-defined-functions-udf"></a>Procedimentos armazenados, acionadores e funções definidas pelo utilizador (UDF)
Conforme descrito na secção anterior, pode escrever a lógica do aplicativo para executar diretamente dentro de uma transação dentro do motor de base de dados. A lógica do aplicativo pode ser escrita inteiramente no JavaScript e pode ser modelada como um procedimento armazenado, acionador ou uma UDF. O código de JavaScript dentro de um procedimento armazenado ou um acionador pode inserir, substituir, eliminar, ler ou consultar documentos dentro de uma coleção. Por outro lado, o JavaScript dentro de uma UDF não é possível inserir, substituir ou eliminar documentos. UDFs enumerar os documentos do conjunto de resultados de uma consulta e produzem a outro conjunto de resultados. Para vários inquilinos, o Azure Cosmos DB impõe uma governação de recursos estrita baseadas na reserva. Cada armazenados procedimento, acionador ou uma UDF obtém um quantum fixo de recursos do sistema operacional para realizar seu trabalho. Além disso, procedimentos armazenados, acionadores e UDFs não podem ligar-se se bibliotecas externas do JavaScript e são bloqueados se estas excedem os orçamentos de recursos alocados aos mesmos. Pode registar, anular o registo de procedimentos armazenados, acionadores e UDFs com uma coleção com as APIs REST.  Após o registo de um procedimento armazenado, acionador ou uma UDF é pré-compilados e armazenada como código de byte, que é executado mais tarde. A seção a seguir ilustra como pode usar o JavaScript SDK do Azure Cosmos DB para registar, executar e anular o registo de um procedimento armazenado, o acionador e uma UDF. O JavaScript SDK é um wrapper simples através da [REST APIs](/rest/api/cosmos-db/). 

### <a name="registering-a-stored-procedure"></a>Registar um procedimento armazenado
Registo de um procedimento armazenado cria um novo recurso de procedimento armazenado numa coleção através de HTTP POST.  

    var storedProc = {
        id: "validateAndCreate",
        body: function (documentToCreate) {
            documentToCreate.id = documentToCreate.id.toUpperCase();

            var collectionManager = getContext().getCollection();
            collectionManager.createDocument(collectionManager.getSelfLink(),
                documentToCreate,
                function(err, documentCreated) {
                    if(err) throw new Error('Error while creating document: ' + err.message;
                    getContext().getResponse().setBody('success - created ' + 
                            documentCreated.name);
                });
        }
    };

    client.createStoredProcedureAsync(collection._self, storedProc)
        .then(function (createdStoredProcedure) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-stored-procedure"></a>Executar um procedimento armazenado
Execução de um procedimento armazenado é feita ao emitir um HTTP POST em relação a um recurso de procedimento armazenado existente passando parâmetros para o procedimento no corpo do pedido.

    var inputDocument = {id : "document1", author: "G. G. Marquez"};
    client.executeStoredProcedureAsync(createdStoredProcedure.resource._self, inputDocument)
        .then(function(executionResult) {
            assert.equal(executionResult, "success - created DOCUMENT1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-stored-procedure"></a>Anular o registo de um procedimento armazenado
Anular o registo de um procedimento armazenado é feito através da emissão de um HTTP DELETE em relação a um recurso de procedimento armazenado existente.   

    client.deleteStoredProcedureAsync(createdStoredProcedure.resource._self)
        .then(function (response) {
            return;
        }, function(error) {
            console.log("Error");
        });


### <a name="registering-a-pre-trigger"></a>Registar um pré-acionador de
Registo de um acionador é feito através da criação de um novo recurso de Acionador numa coleção através de HTTP POST. Pode especificar se o acionador é pré ou um acionador de post e o tipo de operação pode ser associado com (por exemplo, criar, substituir, eliminar ou todos).   

    var preTrigger = {
        id: "upperCaseId",
        body: function() {
                var item = getContext().getRequest().getBody();
                item.id = item.id.toUpperCase();
                getContext().getRequest().setBody(item);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.All
    }

    client.createTriggerAsync(collection._self, preTrigger)
        .then(function (createdPreTrigger) {
            console.log("Successfully created trigger");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-pre-trigger"></a>Executar um pré-acionador de
Execução de um acionador é feita, especificando o nome de um acionador existente no momento da emissão do pedido de POST/PUT/eliminação de um recurso de documento com o cabeçalho do pedido.  

    client.createDocumentAsync(collection._self, { id: "doc1", key: "Love in the Time of Cholera" }, { preTriggerInclude: "upperCaseId" })
        .then(function(createdDocument) {
            assert.equal(createdDocument.resource.id, "DOC1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-pre-trigger"></a>Anular o registo de um pré-acionador de
Anular o registo de um acionador é feito por meio de emitir um HTTP DELETE em relação a um recurso de Acionador existente.  

    client.deleteTriggerAsync(createdPreTrigger._self);
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

### <a name="registering-a-udf"></a>Registar uma UDF
Registo de uma UDF é feito através da criação de um novo recurso UDF numa coleção através de HTTP POST.  

    var udf = { 
        id: "mathSqrt",
        body: function(number) {
                return Math.sqrt(number);
        },
    };
    client.createUserDefinedFunctionAsync(collection._self, udf)
        .then(function (createdUdf) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-udf-as-part-of-the-query"></a>Executar uma UDF como parte da consulta
Uma UDF pode ser especificada como parte da consulta SQL e é utilizada como uma forma de estender o núcleo [linguagem de consulta SQL](sql-api-sql-query-reference.md).

    var filterQuery = "SELECT udf.mathSqrt(r.Age) AS sqrtAge FROM root r WHERE r.FirstName='John'";
    client.queryDocuments(collection._self, filterQuery).toArrayAsync();
        .then(function(queryResponse) {
            var queryResponseDocuments = queryResponse.feed;
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-udf"></a>Anular o registo de uma UDF
Anular o registo de uma UDF simplesmente é feito através da emissão de um HTTP DELETE em relação a um recurso UDF existente.  

    client.deleteUserDefinedFunctionAsync(createdUdf._self)
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

Embora os trechos acima de mostrar o registo (POST), a anulação do registo (PUT), a leitura/lista (GET) e a execução (POST) através do [JavaScript SDK](https://github.com/Azure/azure-documentdb-js), também pode utilizar o [REST APIs](/rest/api/cosmos-db/) ou outros [SDKs do cliente](sql-api-sdk-dotnet.md). 

## <a name="documents"></a>Documentos
Pode inserir, substituir, eliminar, ler, enumerar e consultar documentos JSON arbitrários numa coleção. O Azure Cosmos DB não impõe qualquer esquema e não necessita de índices secundários para oferecer suporte à consulta através de documentos numa coleção. O tamanho máximo para um documento é 2 MB.   

Sendo um serviço de base de dados verdadeiramente aberto, Azure Cosmos DB não inventar quaisquer tipos de dados especializados (por exemplo, hora de data) ou codificações específicas para documentos JSON. O Azure Cosmos DB não requer qualquer especiais convenções JSON para codificar os relacionamentos entre vários documentos; a sintaxe SQL do Azure Cosmos DB fornece consultas relacionais e hierárquicas poderosa propriedades distinguem-se aos documentos de consulta e o projeto sem qualquer anotações especiais nem a necessidade de codificar relações entre documentos usando operadores.  

Como com todos os outros recursos, documentos podem ser criados, substituído, eliminar, ler, enumerados e consultados facilmente usando as APIs REST ou qualquer um da [SDKs cliente](sql-api-sdk-dotnet.md). A eliminar um documento instantaneamente liberta a quota correspondente a todos os anexos aninhados. O nível de consistência de leitura de documentos segue a política de consistência na conta de base de dados. Esta política pode ser substituída numa base por solicitação dependendo dos requisitos de consistência de dados da sua aplicação. Quando a consulta de documentos, a consistência de leitura segue o modo de indexação definido na coleção. Para "consistente,", isto segue a política de consistência da conta. 

## <a name="attachments-and-media"></a>Anexos e suporte de dados
O Azure Cosmos DB permite-lhe armazenar blobs binários/suporte de dados com o Azure Cosmos DB (máximo de 2 GB por conta) ou para o seu próprio arquivo de multimédia remoto. Também permite que representam os metadados de um suporte de dados em termos de documento especial denominado anexo. Um anexo no Azure Cosmos DB é um documento (JSON) especial que referencia o suporte de dados/blob armazenado noutro local. Um anexo é simplesmente um documento especial que captura metadados (por exemplo, localização, autor, etc.) de um suporte de dados armazenado no armazenamento de um suporte de dados remota. 

Considere um aplicativo de leitura de redes sociais que utiliza o Azure Cosmos DB para armazenar as anotações de tinta e metadados, incluindo comentários, destaca, marcadores, classificações, gostos/dislikes etc. associados para um livro eletrônico de um determinado usuário.   

* O conteúdo do livro em si é armazenado no armazenamento do suporte de dados ou disponíveis como parte da conta de base de dados do Azure Cosmos DB ou um arquivo de multimédia remoto. 
* Uma aplicação pode armazenar metadados de cada utilizador como um documento distinto – por exemplo, os metadados do João para book1 é armazenado num documento referenciado pelo /colls/joe/docs/book1. 
* Anexos a apontar para as páginas de conteúdo de um determinado livro de um usuário são armazenados em documento correspondente, por exemplo, /colls/joe/docs/book1/chapter1, /colls/joe/docs/book1/chapter2 etc. 

Os exemplos listados acima utilizam ids amigáveis para transmitir a hierarquia de recursos. Recursos são acessados por meio das APIs REST através de ids de recurso exclusiva. 

O suporte de dados que é gerido pelo Azure Cosmos DB, a propriedade de s_uporte de dados do anexo referencia o suporte de dados pelo respetivo URI. Garante que o Azure Cosmos DB para o lixo recolher o suporte de dados quando todas as referências pendentes são ignoradas. O Azure Cosmos DB automaticamente gera o anexo ao carregar o novo suporte de dados e preenche o s_uporte de dados para apontar para o suporte de dados adicionados recentemente. Se optar por armazenar o suporte de dados num armazenamento de BLOBs remoto gerenciado por si (por exemplo, OneDrive, armazenamento do Azure, DropBox, etc.), pode continuar a utilizar anexos para o suporte de dados de referência. Neste caso, irá criar o anexo por conta própria e preencher sua propriedade de s_uporte de dados.   

Como com todos os outros recursos, os anexos podem ser criados, substituído, eliminado, ler ou enumerado facilmente usando as APIs REST ou qualquer um dos SDKs do cliente. Tal como acontece com documentos, o nível de consistência de leitura de anexos segue a política de consistência na conta de base de dados. Esta política pode ser substituída numa base por solicitação dependendo dos requisitos de consistência de dados da sua aplicação. Ao consultar os anexos, a consistência de leitura segue o modo de indexação definido na coleção. Para "consistente,", isto segue a política de consistência da conta. 
 

## <a name="users"></a>Utilizadores
Um utilizador do Azure Cosmos DB representa um espaço de nomes de lógico para o agrupamento de permissões. Um utilizador do Azure Cosmos DB pode corresponder a um usuário num sistema de gestão de identidade ou uma função de aplicação predefinidas. Para o Azure Cosmos DB, um utilizador representa simplesmente uma abstração para agrupar um conjunto de permissões num banco de dados.   

Para a implementação de vários inquilinos na sua aplicação, pode criar utilizadores no Azure Cosmos DB, que corresponde a seus usuários reais ou os inquilinos do seu aplicativo. Em seguida, pode criar permissões para um determinado usuário que correspondem para o controlo de acesso através de várias coleções, documentos, anexos, etc.   

Como seus aplicativos precisam de dimensionamento com o crescimento dos seus utilizadores, pode adotar várias formas de dividir os dados. Pode modelar cada um dos utilizadores da seguinte forma:   

* Cada utilizador mapeia para uma base de dados.
* Cada utilizador mapeia para uma coleção. 
* Documentos correspondente para vários utilizadores Ir para uma coleção dedicada. 
* Documentos correspondente para vários utilizadores Ir para um conjunto de coleções.   

Independentemente da estratégia de fragmentação específico escolher, pode modelar seus usuários reais como utilizador na base de dados do Azure Cosmos DB e associar permissões detalhadas de cada utilizador.  

![Coleções de utilizadores][3]  
**Estratégias de fragmentação e os utilizadores de modelagem**

Como todos os outros recursos, os utilizadores no Azure Cosmos DB podem ser criados, substituído, eliminado, ler ou enumerado facilmente usando as APIs REST ou qualquer um dos SDKs do cliente. O Azure Cosmos DB fornece sempre consistência forte para leitura ou consultar os metadados de um recurso de utilizador. Vale a pena apontar que eliminar um utilizador automaticamente garante que não é possível aceder a nenhuma das permissões que ele contém. Mesmo que o Azure Cosmos DB reclama a quota das permissões como parte do utilizador eliminado em segundo plano, as permissões eliminadas está disponível instantaneamente novamente para que possa utilizar.  

## <a name="permissions"></a>Permissões
Da perspectiva de controlo de acesso, recursos, tais como contas de base de dados, bases de dados, os utilizadores e permissão são considerados *administrativas* recursos, uma vez que estes requerem permissões administrativas. Por outro lado, recursos, incluindo as coleções, documentos, anexos, procedimentos armazenados, acionadores, e os UDFs são no âmbito num determinado banco de dados e considerados *recursos de aplicativos*. Correspondente para os dois tipos de recursos e as funções que acessá-los (ou seja, o administrador e usuário), o modelo de autorização define dois tipos de *chaves de acesso*: *chave mestra* e  *chave de recurso*. A chave mestra é uma parte da conta de base de dados e é fornecida para o desenvolvedor (ou o administrador) que está a ser aprovisionada a conta de base de dados. Esta chave mestra tem semântica de administrador, que pode ser utilizado para autorizar o acesso aos recursos administrativos e de aplicações. Por outro lado, uma chave de recurso é uma chave de acesso granular que permite o acesso a uma *específico* recurso de aplicação. Portanto, ele captura a relação entre o utilizador de uma base de dados e as permissões que o utilizador tem sobre um recurso específico (por exemplo, coleção, documentos, anexo, procedimento armazenado, acionador ou UDF).   

A única forma de obter uma chave de recurso é através da criação de um recurso de permissão num determinado utilizador. Para criar ou obter uma permissão, uma chave mestra tem de ser apresentada no cabeçalho de autorização. Um recurso de permissão vincula o recurso, o acesso e o utilizador. Depois de criar um recurso de permissão, o usuário precisará apenas apresentar a chave do recurso associado para obter acesso ao recurso relevante. Por conseguinte, uma chave de recurso pode ser visualizada como uma representação lógica e compact do recurso de permissão.  

Como com todos os outros recursos, podem ser criadas permissões no Azure Cosmos DB, substituído, eliminado, ler ou enumerado facilmente usando as APIs REST ou qualquer um dos SDKs do cliente. O Azure Cosmos DB fornece sempre consistência forte para leitura ou consultar os metadados de uma permissão. 

## <a name="next-steps"></a>Passos Seguintes
Saiba mais sobre como trabalhar com os recursos com comandos HTTP na [RESTful interações com os recursos do Azure Cosmos DB](https://msdn.microsoft.com/library/azure/mt622086.aspx).

[1]: media/sql-api-resources/resources1.png
[2]: media/sql-api-resources/resources2.png
[3]: media/sql-api-resources/resources3.png

