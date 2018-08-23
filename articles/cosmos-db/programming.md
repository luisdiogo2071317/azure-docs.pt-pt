---
title: Programação de JavaScript do lado do servidor para o Azure Cosmos DB | Documentos da Microsoft
description: Saiba como utilizar o Azure Cosmos DB para escrever procedimentos armazenados, acionadores de base de dados e funções definidas pelo utilizador (UDFs) em JavaScript. Obtenha dicas de programação de banco de dados e muito mais.
keywords: Base de dados de disparadores, procedimento armazenado, procedimento armazenado, o programa de banco de dados, sproc, azure, do Microsoft azure
services: cosmos-db
author: aliuy
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: andrl
ms.openlocfilehash: 6296eb423f24762ed32a21ef40852dc1a9dd8f36
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/20/2018
ms.locfileid: "42057026"
---
# <a name="azure-cosmos-db-server-side-programming-stored-procedures-database-triggers-and-udfs"></a>Programação de lado do servidor do Azure Cosmos DB: procedimentos armazenados, acionadores de base de dados e UDFs

Saiba como a execução transacional, integrada à linguagem do Azure Cosmos DB do JavaScript permite aos desenvolvedores escreverem **procedimentos armazenados**, **acionadores**, e **funções definidas pelo utilizador (UDFs)**  nativamente num [ECMAScript 2015](http://www.ecma-international.org/ecma-262/6.0/) JavaScript. Integração de JavaScript permite-lhe escrever a lógica do programa que pode ser fornecida e executada diretamente dentro das partições de armazenamento de base de dados. 

Recomendamos que comece por ver o vídeo seguinte, onde eu sou Andrew Liu fornece uma introdução ao modelo de programação de banco de dados do lado do servidor do Azure Cosmos DB. 

> [!VIDEO https://www.youtube.com/embed/s0cXdHNlVI0]
>
> 

Em seguida, regresse a este artigo, onde poderá aprender as respostas às seguintes perguntas:  

* Como escrever um procedimento armazenado, um acionador ou uma UDF usando JavaScript?
* Como é que o Cosmos DB garante ACID?
* Como funcionam transações no Cosmos DB?
* O que são previamente aciona e pós-aciona e como escrever um?
* Como registar e executar um procedimento armazenado, um acionador ou uma UDF de forma RESTful através de HTTP?
* O que SDKs do Cosmos DB estão disponíveis para criar e executar, os procedimentos, acionadores e UDFs são armazenados?

## <a name="introduction-to-stored-procedure-and-udf-programming"></a>Introdução ao procedimento armazenado e da programação UDF
Essa abordagem de *"JavaScript como um dia modernos T-SQL"* isenta os desenvolvedores de aplicativo das complexidades de tipo de correspondência de sistema e tecnologias de mapeamento relacional de objetos. Ele também tem uma série de vantagens intrínsecas que pode ser utilizado para criar aplicações avançadas:  

* **Lógica de procedimento:** JavaScript como uma linguagem de programação de alto nível, fornece uma interface de avançadas e familiar para expressar a lógica de negócios. Pode executar sequências complexas de operações mais perto dos dados.
* **Transações atômicas:** garantias de Cosmos DB que operações executadas dentro de um procedimento armazenado único ou o acionador de base de dados são atómicas. Esta funcionalidade atômica permite que um aplicativo combinar operações relacionadas num único lote, de modo que todos eles com êxito ou nenhum deles seja bem-sucedida. 
* **Desempenho:** o fato de que o JSON é intrinsecamente mapeado para o sistema de tipo de linguagem do Javascript e também é a unidade básica de armazenamento no Cosmos DB permite várias otimizações, como lenta materialização de documentos JSON no pool de buffer e tornando-os disponíveis a pedido para o código em execução. Existem mais benefícios de desempenho associados com a lógica de negócio de envio para a base de dados:
  
  * Processamento em lote – os desenvolvedores podem agrupar operações como inserções e submetê-las em massa. A latência de tráfego de rede de custos e a sobrecarga de armazenamento para criar transações separadas diminui significativamente. 
  * Pré-compilação – o Cosmos DB precompiles procedimentos armazenados, acionadores e definido pelo utilizador funções (UDFs) para evitar o custo de compilação de JavaScript para cada invocação. A sobrecarga de criar o código de byte para a lógica do procedimento é amortizada para um valor mínimo.
  * Sequenciamento – necessidade de muitas operações um efeito colateral ("acionador de") que potencialmente envolve fazer uma ou muitas operações de armazenamento secundário. Além de atomicidade, este é um melhor desempenho quando movido para o servidor. 
* **Encapsulamento:** procedimentos armazenados podem ser utilizados para agrupar a lógica de negócio num único lugar, tem duas vantagens:
  * Ele adiciona uma camada de abstração sobre os dados não processados, o que permite que arquitetos de dados desenvolver seus aplicativos de forma independente a partir dos dados. Essa camada de abstração é vantajosa quando os dados são o esquema, devido às suposições frágil, que poderão ter de ser embutido no aplicativo se têm de lidar com os dados diretamente.  
  * Essa abstração permite que as empresas a manter os seus dados seguros ao facilitar o acesso a partir dos scripts.  

A criação e execução de acionadores de base de dados, procedimentos armazenados e operadores de consulta personalizada é suportada através da [portal do Azure](https://portal.azure.com), o [REST API](/rest/api/cosmos-db/), [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases), e [SDKs cliente](sql-api-sdk-dotnet.md) em muitas plataformas, incluindo .NET, node. js e JavaScript.

Este tutorial utiliza a [SDK de node. js com as perguntas e promessas](http://azure.github.io/azure-documentdb-node-q/) para ilustrar a sintaxe e o uso de procedimentos armazenados, acionadores e UDFs.   

## <a name="stored-procedures"></a>Procedimentos armazenados
### <a name="example-write-a-stored-procedure"></a>Exemplo: Escrever um procedimento armazenado
Vamos começar com um simple procedimento armazenado que retorna uma resposta de "Hello World".

```javascript
var helloWorldStoredProc = {
    id: "helloWorld",
    serverScript: function () {
        var context = getContext();
        var response = context.getResponse();

        response.setBody("Hello, World");
    }
}
```

Procedimentos armazenados estão registados por coleção e podem operar em todos os documentos e anexos presente nessa coleção. O fragmento seguinte mostra como registar o procedimento armazenado de helloWorld com uma coleção. 


```javascript
// register the stored procedure
var createdStoredProcedure;
client.createStoredProcedureAsync('dbs/testdb/colls/testColl', helloWorldStoredProc)
    .then(function (response) {
        createdStoredProcedure = response.resource;
        console.log("Successfully created stored procedure");
    }, function (error) {
        console.log("Error", error);
    });
```


Assim que o procedimento armazenado é registado, pode executá-lo na coleção e ler os resultados novamente no cliente. 

```javascript
// execute the stored procedure
client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/helloWorld')
    .then(function (response) {
        console.log(response.result); // "Hello, World"
    }, function (err) {
        console.log("Error", error);
    });
```

O objeto context fornece acesso a todas as operações que podem ser executadas no armazenamento do Cosmos DB, bem como acesso a objetos de solicitação e resposta. Neste caso, utilize o objeto de resposta para definir o corpo de resposta que foi enviado ao cliente. Para obter mais informações, consulte a [documentação do SDK do Azure Cosmos DB JavaScript servidor](https://azure.github.io/azure-cosmosdb-js-server/).  

Vamos expandir este exemplo e adicionar mais funcionalidades relacionadas com a base de dados para o procedimento armazenado. Procedimentos armazenados podem criar, atualizar, ler, consultar e eliminar documentos e anexos na coleção.    

### <a name="example-write-a-stored-procedure-to-create-a-document"></a>Exemplo: Escrever um procedimento armazenado para criar um documento
O fragmento seguinte mostra como usar o objeto de contexto para interagir com recursos do Cosmos DB.

```javascript
var createDocumentStoredProc = {
    id: "createMyDocument",
    serverScript: function createMyDocument(documentToCreate) {
        var context = getContext();
        var collection = context.getCollection();

        var accepted = collection.createDocument(collection.getSelfLink(),
              documentToCreate,
              function (err, documentCreated) {
                  if (err) throw new Error('Error' + err.message);
                  context.getResponse().setBody(documentCreated.id)
              });
        if (!accepted) return;
    }
}
```


Este procedimento armazenado aceita como entrada documentToCreate, o corpo de um documento a ser criada na coleção atual. Todas essas operações são assíncronas e dependem de retornos de chamada de função de JavaScript. A função de retorno de chamada tem dois parâmetros, uma para o objeto de erro, caso que a operação falhar e outro para o objeto criado. Dentro do retorno de chamada, os utilizadores podem lidar com a exceção ou gerar um erro. No caso de um retorno de chamada não for fornecido, e existe um erro, o tempo de execução do Azure Cosmos DB gera um erro.   

No exemplo acima, o retorno de chamada emite um erro se a operação falhou. Caso contrário, ele define o ID do documento criado como o corpo da resposta ao cliente. Eis como este procedimento armazenado é executado com parâmetros de entrada.

```javascript
// register the stored procedure
client.createStoredProcedureAsync('dbs/testdb/colls/testColl', createDocumentStoredProc)
    .then(function (response) {
        var createdStoredProcedure = response.resource;

        // run stored procedure to create a document
        var docToCreate = {
            id: "DocFromSproc",
            book: "The Hitchhiker’s Guide to the Galaxy",
            author: "Douglas Adams"
        };

        return client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/createMyDocument',
              docToCreate);
    }, function (error) {
        console.log("Error", error);
    })
.then(function (response) {
    console.log(response); // "DocFromSproc"
}, function (error) {
    console.log("Error", error);
});
```

Este procedimento armazenado pode ser modificado para usar uma matriz de órgãos de documento como entrada e criá-los na execução do procedimento armazenado mesmo em vez de vários pedidos para criar cada um deles individualmente. Este procedimento armazenado pode ser utilizado para implementar um importador em massa eficiente para o Cosmos DB (abordado posteriormente neste tutorial).   

O exemplo descrito demonstrei como usar procedimentos armazenados. Em seguida aprenderá sobre os acionadores e funções definidas pelo utilizador (UDFs) mais tarde no tutorial.

### <a name="known-issues"></a>Problemas conhecidos

Ao definir um procedimento armazenado com o portal do Azure, os parâmetros de entrada são sempre enviados como uma cadeia de caracteres para o procedimento armazenado. Mesmo se passar uma matriz de cadeias de caracteres como entrada, a matriz é convertida em cadeia de caracteres e enviada para o procedimento armazenado. Para resolver este problema, pode definir uma função dentro de seu procedimento armazenado para analisar a cadeia de caracteres como uma matriz. O código a seguir é um exemplo para analisar a cadeia de caracteres como uma matriz: 

```javascript
function sample(arr) {
    if (typeof arr === "string") arr = JSON.parse(arr);
    
    arr.forEach(function(a) {
        // do something here
        console.log(a);
    });
}
```

## <a name="database-program-transactions"></a>Transações de programa da base de dados
Transação numa base de dados típica pode ser definida como uma seqüência de operações executadas como uma unidade lógica única de trabalho. Cada transação fornece **garantias de ACID**. ACID é um acrônimo bem conhecido que representa quatro propriedades - atomicidade, consistência, isolamento e durabilidade.  

Resumidamente, atomicidade garante que todo o trabalho feito dentro de uma transação é tratado como uma única unidade em que qualquer um dos tudo é consolidada ou nenhum. Consistência certifica-se de que os dados estão sempre em bom estado interno em transações. Isolamento garante que nenhuma dois transação interfere uns aos outros – em geral, sistemas comerciais mais fornecem vários níveis de isolamento que podem ser utilizados com base nas necessidades da aplicação. Durabilidade garante que qualquer alteração que está empenhada na base de dados estará sempre presente.   

No Cosmos DB, o JavaScript está alojado no mesmo espaço de memória da base de dados. Por conseguinte, pedidos efetuados em procedimentos armazenados e acionadores executado no mesmo âmbito de uma sessão de banco de dados. Esta funcionalidade permite que o Cosmos DB garantir ACID para todas as operações que fazem parte de um único procedimento/acionador armazenado. Considere a seguinte definição de procedimento armazenado:

```javascript
// JavaScript source code
var exchangeItemsSproc = {
    id: "exchangeItems",
    serverScript: function (playerId1, playerId2) {
        var context = getContext();
        var collection = context.getCollection();
        var response = context.getResponse();

        var player1Document, player2Document;

        // query for players
        var filterQuery = 'SELECT * FROM Players p where p.id  = "' + playerId1 + '"';
        var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery, {},
            function (err, documents, responseOptions) {
                if (err) throw new Error("Error" + err.message);

                if (documents.length != 1) throw "Unable to find both names";
                player1Document = documents[0];

                var filterQuery2 = 'SELECT * FROM Players p where p.id = "' + playerId2 + '"';
                var accept2 = collection.queryDocuments(collection.getSelfLink(), filterQuery2, {},
                    function (err2, documents2, responseOptions2) {
                        if (err2) throw new Error("Error" + err2.message);
                        if (documents2.length != 1) throw "Unable to find both names";
                        player2Document = documents2[0];
                        swapItems(player1Document, player2Document);
                        return;
                    });
                if (!accept2) throw "Unable to read player details, abort ";
            });

        if (!accept) throw "Unable to read player details, abort ";

        // swap the two players’ items
        function swapItems(player1, player2) {
            var player1ItemSave = player1.item;
            player1.item = player2.item;
            player2.item = player1ItemSave;

            var accept = collection.replaceDocument(player1._self, player1,
                function (err, docReplaced) {
                    if (err) throw "Unable to update player 1, abort ";

                    var accept2 = collection.replaceDocument(player2._self, player2,
                        function (err2, docReplaced2) {
                            if (err) throw "Unable to update player 2, abort"
                        });

                    if (!accept2) throw "Unable to update player 2, abort";
                });

            if (!accept) throw "Unable to update player 1, abort";
        }
    }
}

// register the stored procedure in Node.js client
client.createStoredProcedureAsync(collection._self, exchangeItemsSproc)
    .then(function (response) {
        var createdStoredProcedure = response.resource;
    }
);
```

Este procedimento armazenado utiliza transações dentro de uma aplicação de jogos para itens de trade entre dois jogadores numa única operação. O procedimento armazenado tenta ler os dois documentos, cada um correspondendo aos IDs de player passado como um argumento. Se ambos os documentos de player forem encontrados, o procedimento armazenado atualiza os documentos trocando seus itens. Se for encontrado qualquer erro ao longo do processo, ele lançará uma exceção de JavaScript que implicitamente anulá-la.

Se a coleção o procedimento armazenado está registrada em relação a é uma coleção de partição única, em seguida, a transação tem um âmbito para todos os documentos na coleção. Se a coleção é particionada, procedimentos armazenados são executados no âmbito de transação de chave de partição única. Cada armazenado, a execução do procedimento, em seguida, tem de incluir um valor de chave de partição correspondentes ao âmbito de transação tem de ser executado. Para obter mais informações, consulte [particionamento do Azure Cosmos DB](partition-data.md).

### <a name="commit-and-rollback"></a>Confirmação e reversão
Transações estão profundamente e nativamente integradas no modelo de programação de JavaScript do Cosmos DB. Dentro de uma função de JavaScript, a todas as operações são automaticamente encapsuladas numa única transação. Se o JavaScript concluída sem qualquer exceção, as operações para a base de dados são confirmadas. Na verdade, as instruções de "BEGIN TRANSACTION" e "CONSOLIDAR a transação" nos bancos de dados relacionais são implícitas no Cosmos DB.  

Se houver qualquer exceção que é propagada de scripts, tempo de execução do JavaScript do Cosmos DB irá reverter a transação inteira. Conforme mostrado no exemplo anterior, lançando uma exceção é efetivamente equivalente a um "ROLLBACK TRANSACTION" no Cosmos DB.

### <a name="data-consistency"></a>Consistência dos dados
Procedimentos armazenados e acionadores são sempre executados na réplica primária do contentor do Azure Cosmos DB. Isto garante que as leituras de dentro de armazenadas consistência forte de oferta de procedimentos. Consultas com funções definidas pelo utilizador podem ser executadas em primário ou de qualquer réplica secundária, mas certifique-se de acordo com o nível de consistência pedido ao escolher a réplica adequada.

## <a name="bounded-execution"></a>Execução estagnação
Tem de concluir todas as operações de Cosmos DB no servidor especificado duração de tempo limite do pedido. Esta restrição aplica-se também a funções de JavaScript (procedimentos armazenados, acionadores e funções definidas pelo utilizador). Se uma operação não for concluída com limite de tempo, a transação é revertida. As funções JavaScript tem de concluir dentro do tempo limite ou implementar um modelo com base em continuação a execução de batch/retomar.  

Para simplificar o desenvolvimento de procedimentos armazenados e acionadores para lidar com limites de tempo, todas as funções no objeto de coleção (para criar, ler, substituir e eliminar de documentos e anexos) um booleano valor de retorno que representa se essa operação irá concluir. Se este valor for FALSO, é uma indicação de que o limite de tempo está prestes a expirar e que o procedimento tem de encapsular a execução.  Operações em fila antes da primeira operação de arquivo não aceite são garantidas para concluir o procedimento armazenado é concluído no tempo e não colocar em fila mais pedidos.  

As funções JavaScript também são limitadas no consumo de recursos. O cosmos DB reserva débito por coleção ou para um conjunto de contentores. Taxa de transferência é expresso em termos de uma unidade normalizada de CPU, memória e chamadas de unidades de pedido ou RUs de consumo de e/s. As funções JavaScript poderá potencialmente utilizar até um grande número de RUs dentro de pouco tempo e poderão obter a taxa de limitado se for atingido o limite da coleção. Os procedimentos armazenados com muitos recursos também podem ser colocados em quarentena para garantir a disponibilidade das operações de base de dados primitivos.  

### <a name="example-bulk-importing-data-into-a-database-program"></a>Exemplo: Em massa a importar dados para um programa de banco de dados
Segue-se um exemplo de um procedimento armazenado que é escrito para importação em massa documentos numa coleção. Observe como o procedimento armazenado processa a execução vinculada ao verificar o valor de booleano retornar o valor de createDocument e, em seguida, usa a contagem de documentos inserido em cada invocação do procedimento armazenado para controlar e retomar o progresso em lotes.

```javascript
function bulkImport(docs) {
    var collection = getContext().getCollection();
    var collectionLink = collection.getSelfLink();

    // The count of imported docs, also used as current doc index.
    var count = 0;

    // Validate input.
    if (!docs) throw new Error("The array is undefined or null.");

    var docsLength = docs.length;
    if (docsLength == 0) {
        getContext().getResponse().setBody(0);
    }

    // Call the create API to create a document.
    tryCreate(docs[count], callback);

    // Note that there are 2 exit conditions:
    // 1) The createDocument request was not accepted. 
    //    In this case the callback will not be called, we just call setBody and we are done.
    // 2) The callback was called docs.length times.
    //    In this case all documents were created and we don’t need to call tryCreate anymore. Just call setBody and we are done.
    function tryCreate(doc, callback) {
        var isAccepted = collection.createDocument(collectionLink, doc, callback);

        // If the request was accepted, callback will be called.
        // Otherwise report current count back to the client, 
        // which will call the script again with remaining set of docs.
        if (!isAccepted) getContext().getResponse().setBody(count);
    }

    // This is called when collection.createDocument is done in order to process the result.
    function callback(err, doc, options) {
        if (err) throw err;

        // One more document has been inserted, increment the count.
        count++;

        if (count >= docsLength) {
            // If we created all documents, we are done. Just set the response.
            getContext().getResponse().setBody(count);
        } else {
            // Create next document.
            tryCreate(docs[count], callback);
        }
    }
}
```

## <a id="trigger"></a> Acionadores de base de dados
### <a name="database-pre-triggers"></a>Pré-acionadores de base de dados
O cosmos DB fornece acionadores que são executados ou acionados por uma operação num documento. Por exemplo, pode especificar um pré-acionador de quando estiver a criar um documento – este pré-acionador será executado antes do documento é criado. O exemplo seguinte mostra como os pré-acionadores de podem ser usados para validar as propriedades de um documento que está a ser criada:

```javascript
var validateDocumentContentsTrigger = {
    id: "validateDocumentContents",
    serverScript: function validate() {
        var context = getContext();
        var request = context.getRequest();

        // document to be created in the current operation
        var documentToCreate = request.getBody();

        // validate properties
        if (!("timestamp" in documentToCreate)) {
            var ts = new Date();
            documentToCreate["my timestamp"] = ts.getTime();
        }

        // update the document that will be created
        request.setBody(documentToCreate);
    },
    triggerType: TriggerType.Pre,
    triggerOperation: TriggerOperation.Create
}
```

E o código de registo do lado do cliente do node. js correspondente para o acionador:

```javascript
// register pre-trigger
client.createTriggerAsync(collection.self, validateDocumentContentsTrigger)
    .then(function (response) {
        console.log("Created", response.resource);
        var docToCreate = {
            id: "DocWithTrigger",
            event: "Error",
            source: "Network outage"
        };

        // run trigger while creating above document 
        var options = { preTriggerInclude: "validateDocumentContents" };

        return client.createDocumentAsync(collection.self,
              docToCreate, options);
    }, function (error) {
        console.log("Error", error);
    })
.then(function (response) {
    console.log(response.resource); // document with timestamp property added
}, function (error) {
    console.log("Error", error);
});
```

Pré-acionadores não podem ter parâmetros de entrada. O objeto de pedido pode ser usado para manipular a mensagem de solicitação associada com a operação. Aqui, o pré-acionador de está a ser executado com a criação de um documento, e o corpo da mensagem de pedido contém o documento a ser criada no formato JSON.   

Quando os acionadores são registrados, os utilizadores podem especificar as operações que pode ser executado com. Este acionador foi criado com TriggerOperation.Create, o que significa que não é permitido com o acionador numa operação de substituição, conforme mostrado no código a seguir.

```javascript
var options = { preTriggerInclude: "validateDocumentContents" };

client.replaceDocumentAsync(docToReplace.self,
              newDocBody, options)
.then(function (response) {
    console.log(response.resource);
}, function (error) {
    console.log("Error", error);
});

// Fails, can’t use a create trigger in a replace operation
```
### <a name="database-post-triggers"></a>Pós-acionadores de base de dados
Pós-acionadores de, como o Pre-acionadores, estão associados uma operação num documento e não se parâmetros de entrada. São executados **depois de** a operação foi concluída e tem acesso para a mensagem de resposta que é enviado ao cliente.   

O exemplo seguinte mostra os pós-acionadores de em ação:
```javascript
var updateMetadataTrigger = {
    id: "updateMetadata",
    serverScript: function updateMetadata() {
        var context = getContext();
        var collection = context.getCollection();
        var response = context.getResponse();

        // document that was created
        var createdDocument = response.getBody();

        // query for metadata document
        var filterQuery = 'SELECT * FROM root r WHERE r.id = "_metadata"';
        var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery,
            updateMetadataCallback);
        if(!accept) throw "Unable to update metadata, abort";

        function updateMetadataCallback(err, documents, responseOptions) {
            if(err) throw new Error("Error" + err.message);
                     if(documents.length != 1) throw 'Unable to find metadata document';

                     var metadataDocument = documents[0];

                     // update metadata
                     metadataDocument.createdDocuments += 1;
                     metadataDocument.createdNames += " " + createdDocument.id;
                     var accept = collection.replaceDocument(metadataDocument._self,
                           metadataDocument, function(err, docReplaced) {
                                  if(err) throw "Unable to update metadata, abort";
                           });
                     if(!accept) throw "Unable to update metadata, abort";
                     return;                    
        }                                                                                            
    },
    triggerType: TriggerType.Post,
    triggerOperation: TriggerOperation.All
}

```
O acionador pode ser registrado, conforme mostrado no exemplo a seguir.
```javascript
// register post-trigger
client.createTriggerAsync('dbs/testdb/colls/testColl', updateMetadataTrigger)
    .then(function(createdTrigger) { 
        var docToCreate = { 
            name: "artist_profile_1023",
            artist: "The Band",
            albums: ["Hellujah", "Rotators", "Spinning Top"]
        };

        // run trigger while creating above document 
        var options = { postTriggerInclude: "updateMetadata" };

        return client.createDocumentAsync(collection.self,
              docToCreate, options);
    }, function(error) {
        console.log("Error" , error);
    })
.then(function(response) {
    console.log(response.resource); 
}, function(error) {
    console.log("Error" , error);
});
```

Este acionador consulta o documento de metadados e atualiza-o com detalhes sobre o documento recentemente criado.  

É algo que é importante ter em conta a **transacional** execução de acionadores no Cosmos DB. Este pós-acionador é executado como parte da mesma transação como a criação do documento original. Por conseguinte, se gerar uma exceção ao acionador de pós-(Digamos, se for não é possível atualizar o documento de metadados), a transação inteira falhará e ser revertida. Nenhum documento vai ser criado e uma exceção será retornada.  

## <a id="udf"></a>Funções definidas pelo utilizador
Funções definidas pelo utilizador (UDFs) são utilizadas para ampliar a gramática de linguagem de consulta do Azure Cosmos DB SQL e implementar a lógica de negócio personalizada. Eles só podem ser chamados de dentro de consultas. Eles não têm acesso ao objeto de contexto e se destinam a ser utilizado como o JavaScript só de computação. Por conseguinte, UDFs podem ser executadas em réplicas secundárias do serviço do Cosmos DB.  

O exemplo seguinte cria uma UDF para calcular o imposto de renda com base nas tarifas para vários colchetes de rendimento e, em seguida, utiliza-o dentro de uma consulta para encontrar todas as pessoas que pagas mais de US $20.000 em impostos.

```javascript
var taxUdf = {
    id: "tax",
    serverScript: function tax(income) {

        if(income == undefined) 
            throw 'no input';

        if (income < 1000) 
            return income * 0.1;
        else if (income < 10000) 
            return income * 0.2;
        else
            return income * 0.4;
    }
}
```

A UDF, em seguida, pode ser utilizada em consultas, como no exemplo a seguir:

```javascript
// register UDF
client.createUserDefinedFunctionAsync('dbs/testdb/colls/testColl', taxUdf)
    .then(function(response) { 
        console.log("Created", response.resource);

        var query = 'SELECT * FROM TaxPayers t WHERE udf.tax(t.income) > 20000'; 
        return client.queryDocuments('dbs/testdb/colls/testColl',
               query).toArrayAsync();
    }, function(error) {
        console.log("Error" , error);
    })
.then(function(response) {
    var documents = response.feed;
    console.log(response.resource); 
}, function(error) {
    console.log("Error" , error);
});
```

## <a name="javascript-language-integrated-query-api"></a>Consulta integrada à linguagem JavaScript API
Além de emitir consultas com a gramática SQL do Azure Cosmos DB, o SDK do lado do servidor permite-lhe executar consultas otimizadas através de uma interface Fluente do JavaScript sem qualquer conhecimento de SQL. A consulta de JavaScript que API permite-lhe criar programaticamente consultas ao transmitir as funções de predicado em função chainable chama, com uma sintaxe familiar do ECMAScript5 built-ins de matriz e bibliotecas JavaScript populares, como Lodash. Consultas são analisadas pelo tempo de execução JavaScript a ser executada com eficiência usando índices do Azure Cosmos DB.

> [!NOTE]
> `__` (valor de duplo sublinhado) é um alias para `getContext().getCollection()`.
> <br/>
> Em outras palavras, pode usar `__` ou `getContext().getCollection()` para acessar a API de consulta do JavaScript.
> 
> 

As funções suportadas incluem:

<ul>
<li>
<b>chain().... valor ([retorno de chamada] [, opções])</b>
<ul>
<li>
Inicia uma chamada em cadeia que tem de ser terminada com Value ().
</li>
</ul>
</li>
<li>
<b>filtro (predicateFunction [, opções] [, chamada de retorno])</b>
<ul>
<li>
Filtra a entrada usando uma função de predicado que retorna verdadeiro/falso para filtrar documentos entrados entrada/saída para o conjunto resultante. Esta função tem um comportamento semelhante a uma cláusula WHERE em SQL.
</li>
</ul>
</li>
<li>
<b>mapa (transformationFunction [, opções] [, chamada de retorno])</b>
<ul>
<li>
Aplica-se uma projeção dada uma função de transformação que mapeia cada item de entrada para um valor ou o objeto JavaScript. Esta função tem um comportamento semelhante a uma cláusula SELECT em SQL.
</li>
</ul>
</li>
<li>
<b>pluck ([propertyName] [, opções] [, chamada de retorno])</b>
<ul>
<li>
Esta função é um atalho para um mapa que extrai o valor de uma única propriedade de cada item de entrada.
</li>
</ul>
</li>
<li>
<b>Nivelamento ([isShallow] [, opções] [, chamada de retorno])</b>
<ul>
<li>
Combina e nivela as matrizes de cada item entrada para uma única matriz. Esta função tem um comportamento semelhante para SelectMany no LINQ.
</li>
</ul>
</li>
<li>
<b>sortBy ([predicado] [, opções] [, chamada de retorno])</b>
<ul>
<li>
Produza um novo conjunto de documentos classificando-os documentos no fluxo de documento de entrada em ordem com o predicado de determinado crescente. Esta função tem um comportamento semelhante a uma cláusula ORDER BY em SQL.
</li>
</ul>
</li>
<li>
<b>sortByDescending ([predicado] [, opções] [, chamada de retorno])</b>
<ul>
<li>
Produza um novo conjunto de documentos classificando-os documentos no fluxo de documento de entrada por ordem descendente utilizando o predicado especificado. Esta função tem um comportamento semelhante a uma cláusula ORDER BY x DESC em SQL.
</li>
</ul>
</li>
</ul>


Quando incluídos no interior do predicado de e/ou o Seletor de funções, as construções de JavaScript seguintes serão automaticamente otimizadas para ser executado diretamente no Azure Cosmos DB índices:

* Operadores simples: = + - * / % | ^ &amp; == != === !=== &lt; &gt; &lt;= &gt;= || &amp;&amp; &lt;&lt; &gt;&gt; &gt;&gt;&gt;! ~
* Literais, incluindo o literal de objeto: {}
* var, retorno

As seguintes construções de JavaScript não serão otimizadas para índices do Azure Cosmos DB:

* Controlar o fluxo (por exemplo, se, para, embora)
* Chamadas de função

Para obter mais informações, consulte a [JSDocs do lado do servidor](https://azure.github.io/azure-cosmosdb-js-server/).

### <a name="example-write-a-stored-procedure-using-the-javascript-query-api"></a>Exemplo: Escrever um procedimento armazenado com a API de consulta do JavaScript
O exemplo de código a seguir é um exemplo de como a API de consulta JavaScript pode ser utilizada no contexto de um procedimento armazenado. O procedimento armazenado insere um documento, dado por um parâmetro de entrada e uma metadados de atualizações de documentos, usando o `__.filter()` método, com minSize, maxSize e totalSize com base na propriedade de tamanho do documento de entrada.

```javascript
/**
 * Insert actual doc and update metadata doc: minSize, maxSize, totalSize based on doc.size.
 */
function insertDocumentAndUpdateMetadata(doc) {
  // HTTP error codes sent to our callback funciton by DocDB server.
  var ErrorCode = {
    RETRY_WITH: 449,
  }

  var isAccepted = __.createDocument(__.getSelfLink(), doc, {}, function(err, doc, options) {
    if (err) throw err;

    // Check the doc (ignore docs with invalid/zero size and metaDoc itself) and call updateMetadata.
    if (!doc.isMetadata && doc.size > 0) {
      // Get the meta document. We keep it in the same collection. it's the only doc that has .isMetadata = true.
      var result = __.filter(function(x) {
        return x.isMetadata === true
      }, function(err, feed, options) {
        if (err) throw err;

        // We assume that metadata doc was pre-created and must exist when this script is called.
        if (!feed || !feed.length) throw new Error("Failed to find the metadata document.");

        // The metadata document.
        var metaDoc = feed[0];

        // Update metaDoc.minSize:
        // for 1st document use doc.Size, for all the rest see if it's less than last min.
        if (metaDoc.minSize == 0) metaDoc.minSize = doc.size;
        else metaDoc.minSize = Math.min(metaDoc.minSize, doc.size);

        // Update metaDoc.maxSize.
        metaDoc.maxSize = Math.max(metaDoc.maxSize, doc.size);

        // Update metaDoc.totalSize.
        metaDoc.totalSize += doc.size;

        // Update/replace the metadata document in the store.
        var isAccepted = __.replaceDocument(metaDoc._self, metaDoc, function(err) {
          if (err) throw err;
          // Note: in case concurrent updates causes conflict with ErrorCode.RETRY_WITH, we can't read the meta again 
          //       and update again because due to Snapshot isolation we will read same exact version (we are in same transaction).
          //       We have to take care of that on the client side.
        });
        if (!isAccepted) throw new Error("replaceDocument(metaDoc) returned false.");
      });
      if (!result.isAccepted) throw new Error("filter for metaDoc returned false.");
    }
  });
  if (!isAccepted) throw new Error("createDocument(actual doc) returned false.");
}
```

## <a name="sql-to-javascript-cheat-sheet"></a>SQL para Javascript e dicas
A tabela seguinte apresenta várias consultas SQL e as consultas de JavaScript correspondentes.

Como com consultas SQL, chaves de propriedade de documento (por exemplo, `doc.id`) diferenciam maiúsculas de minúsculas.

|SQL| Consulta de JavaScript API|Descrição abaixo|
|---|---|---|
|SELECIONE *<br>ATRAVÉS da documentação| __.map(function(doc) { <br>&nbsp;&nbsp;&nbsp;&nbsp;devolver o documento;<br>});|1|
|SELECIONE docs.id, docs.message como msg, docs.actions <br>ATRAVÉS da documentação|__.map(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;devolver {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ID: doc.id,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;msg: doc.message,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;actions:doc.actions<br>&nbsp;&nbsp;&nbsp;&nbsp;};<br>});|2|
|SELECIONE *<br>ATRAVÉS da documentação<br>WHERE docs.id="X998_Y998"|__.filter(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;return doc.id ==="X998_Y998";<br>});|3|
|SELECIONE *<br>ATRAVÉS da documentação<br>WHERE ARRAY_CONTAINS(docs.Tags, 123)|__.filter(function(x) {<br>&nbsp;&nbsp;&nbsp;&nbsp;devolver x.Tags & & x.Tags.indexOf(123) > -1;<br>});|4|
|SELECIONE docs.id, docs.message como msg<br>ATRAVÉS da documentação<br>WHERE docs.id="X998_Y998"|__.chain()<br>&nbsp;&nbsp;&nbsp;&nbsp;.filter(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return doc.id ==="X998_Y998";<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.map(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;devolver {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ID: doc.id,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;msg: doc.message<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>.value();|5|
|Etiqueta de SELECT VALUE<br>ATRAVÉS da documentação<br>Junte-se docs IN de marca. Etiquetas<br>ORDER BY docs._ts|__.chain()<br>&nbsp;&nbsp;&nbsp;&nbsp;.filter(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;devolva o documento. As etiquetas & & Array.isArray (documento. Etiquetas);<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.sortBy(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;devolver doc._ts;<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.pluck("Tags")<br>&nbsp;&nbsp;&nbsp;&nbsp;.flatten()<br>&nbsp;&nbsp;&nbsp;&nbsp;.value()|6|

As seguintes descrições explicam cada consulta na tabela acima.
1. Resultados em todos os documentos (paginados com o token de continuação), como é.
2. O id, a mensagem (um alias para msg) e a ação a partir de todos os documentos de projetos.
3. Consultas para documentos com o predicado: id = "X998_Y998".
4. Consultas para documentos que têm uma propriedade de etiquetas e marcas é uma matriz que contém o valor ' 123.
5. Consultas para documentos com um predicado, id = "X998_Y998" e, em seguida, projeta o id e a mensagem (um alias para msg).
6. Filtros para documentos que tem uma propriedade de matriz, etiquetas e ordena os documentos resultantes pela propriedade de sistema TS timestamp e, em seguida, projetos + nivela a matriz de etiquetas.


## <a name="runtime-support"></a>Suporte de tempo de execução
O Azure Cosmos DB [API JavaScript do lado do servidor](https://azure.github.io/azure-cosmosdb-js-server/) fornece suporte para a maioria dos recursos de linguagem JavaScript importantes como padronizado pelo [ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm).

### <a name="security"></a>Segurança
JavaScript de procedimentos armazenados e acionadores ficam em área restrita, para que os efeitos de um script não escapem para o outro sem passar pelo isolamento de transação de instantâneo ao nível da base de dados. Os ambientes de tempo de execução são agrupados, mas limpos do contexto após cada execução. Por conseguinte, é garantido que estar seguro de quaisquer efeitos colaterais indesejados uns dos outros.

### <a name="pre-compilation"></a>Pré-compilação
Procedimentos armazenados, acionadores e UDFs são implicitamente pré-compilados para o formato de código de byte para evitar o custo de compilação no momento da cada invocação do script. Pré-compilação garante a invocação de procedimentos armazenados é rápida e ter um volume de memória baixa.

## <a name="client-sdk-support"></a>Suporte do SDK de cliente
Para além do Azure Cosmos DB [node. js](sql-api-sdk-node.md) API, Azure Cosmos DB tem [.NET](sql-api-sdk-dotnet.md), [.NET Core](sql-api-sdk-dotnet-core.md), [Java](sql-api-sdk-java.md), [JavaScript ](sql-api-sdk-node.md), e [Python SDKs](sql-api-sdk-python.md) para a API de SQL também. Procedimentos armazenados, acionadores e UDFs podem ser criados e executados usando qualquer um destes SDKs também. O exemplo seguinte mostra como criar e executar um procedimento armazenado com o cliente do .NET. Observe como os tipos .NET são passados para o procedimento armazenado como JSON e ler.

```javascript
var markAntiquesSproc = new StoredProcedure
{
    Id = "ValidateDocumentAge",
    Body = @"
            function(docToCreate, antiqueYear) {
                var collection = getContext().getCollection();    
                var response = getContext().getResponse();    

                if(docToCreate.Year != undefined && docToCreate.Year < antiqueYear){
                    docToCreate.antique = true;
                }

                collection.createDocument(collection.getSelfLink(), docToCreate, {}, 
                    function(err, docCreated, options) { 
                        if(err) throw new Error('Error while creating document: ' + err.message);                              
                        if(options.maxCollectionSizeInMb == 0) throw 'max collection size not found'; 
                        response.setBody(docCreated);
                });
         }"
};

// register stored procedure
StoredProcedure createdStoredProcedure = await client.CreateStoredProcedureAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), markAntiquesSproc);
dynamic document = new Document() { Id = "Borges_112" };
document.Title = "Aleph";
document.Year = 1949;

// execute stored procedure
Document createdDocument = await client.ExecuteStoredProcedureAsync<Document>(UriFactory.CreateStoredProcedureUri("db", "coll", "ValidateDocumentAge"), document, 1920);
```

Este exemplo mostra como utilizar o [SQL .NET API](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) para criar um pré- acionador de e criar um documento com o acionador ativado. 

```javascript
Trigger preTrigger = new Trigger()
{
    Id = "CapitalizeName",
    Body = @"function() {
        var item = getContext().getRequest().getBody();
        item.id = item.id.toUpperCase();
        getContext().getRequest().setBody(item);
    }",
    TriggerOperation = TriggerOperation.Create,
    TriggerType = TriggerType.Pre
};

Document createdItem = await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), new Document { Id = "documentdb" },
    new RequestOptions
    {
        PreTriggerInclude = new List<string> { "CapitalizeName" },
    });
```

E o exemplo seguinte mostra como criar uma função definida pelo utilizador (UDF) e utilizá-lo num [consulta SQL](sql-api-sql-query.md).

```javascript
UserDefinedFunction function = new UserDefinedFunction()
{
    Id = "LOWER",
    Body = @"function(input) 
    {
        return input.toLowerCase();
    }"
};

foreach (Book book in client.CreateDocumentQuery(UriFactory.CreateDocumentCollectionUri("db", "coll"),
    "SELECT * FROM Books b WHERE udf.LOWER(b.Title) = 'war and peace'"))
{
    Console.WriteLine("Read {0} from query", book);
}
```

## <a name="rest-api"></a>API REST
Todas as operações do Azure Cosmos DB podem ser executadas de forma RESTful. Procedimentos armazenados, acionadores e funções definidas pelo utilizador poderá registar-se numa coleção, utilizando o HTTP POST. O exemplo seguinte mostra como registar um procedimento armazenado:

    POST https://<url>/sprocs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT


    var x = {
      "name": "createAndAddProperty",
      "body": function (docToCreate, addedPropertyName, addedPropertyValue) {
                var collectionManager = getContext().getCollection();
                collectionManager.createDocument(
                    collectionManager.getSelfLink(),
                    docToCreate,
                    function(err, docCreated) {
                      if(err) throw new Error('Error:  ' + err.message);
                      docCreated[addedPropertyName] = addedPropertyValue;
                      getContext().getResponse().setBody(docCreated);
                    });
            }
    }


O procedimento armazenado for registado por um pedido POST contra o URI bds/testdb/colls/testColl/sprocs em execução com o corpo que contém o procedimento armazenado a criar. Acionadores e UDFs podem ser registrados da mesma forma ao emitir uma POSTAGEM contra /triggers e /udfs, respetivamente.
Isso armazenados procedimento pode, em seguida, ser executado através da emissão de um pedido POST em relação a respetiva ligação de recurso:

    POST https://<url>/sprocs/<sproc> HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:20 GMT


    [ { "name": "TestDocument", "book": "Autumn of the Patriarch"}, "Price", 200 ]


Aqui, a entrada para o procedimento armazenado é transmitida no corpo do pedido. A entrada é passada como uma matriz JSON de parâmetros de entrada. O procedimento armazenado aceita a entrada primeiro como um documento que é um corpo de resposta. Segue-se a resposta que receber:

    HTTP/1.1 200 OK

    { 
      name: 'TestDocument',
      book: 'Autumn of the Patriarch',
      id: 'V7tQANV3rAkDAAAAAAAAAA==',
      ts: 1407830727,
      self: 'dbs/V7tQAA==/colls/V7tQANV3rAk=/docs/V7tQANV3rAkDAAAAAAAAAA==/',
      etag: '6c006596-0000-0000-0000-53e9cac70000',
      attachments: 'attachments/',
      Price: 200
    }


Acionadores, ao contrário dos procedimentos armazenados, não não possível executar diretamente. Em vez disso, eles são executados como parte de uma operação num documento. Pode especificar os disparadores sejam executados com um pedido de uso de cabeçalhos HTTP. O código seguinte mostra o pedido para criar um documento.

    POST https://<url>/docs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    x-ms-documentdb-pre-trigger-include: validateDocumentContents 
    x-ms-documentdb-post-trigger-include: bookCreationPostTrigger

    {
       "name": "newDocument",
       "title": "The Wizard of Oz",
       "author": "Frank Baum",
       "pages": 92
    }


Aqui, o Pre-acionador para ser executado com o pedido é especificado no cabeçalho x-ms-documentdb-pre-trigger-include. Do mesmo modo, qualquer pós-gatilhos de são fornecidos no cabeçalho x-ms-documentdb-post-trigger-include. Pré e pós-acionadores podem ser especificados para uma determinada solicitação.

## <a name="sample-code"></a>Código de exemplo
Pode encontrar mais exemplos de código do lado do servidor (incluindo [eliminação em massa](https://github.com/Azure/azure-cosmosdb-js-server/blob/master/samples/stored-procedures/bulkDelete.js), e [atualizar](https://github.com/Azure/azure-cosmosdb-js-server/blob/master/samples/stored-procedures/update.js)) no [repositório do GitHub](https://github.com/Azure/azure-cosmosdb-js-server/tree/master/samples).

Quer partilhar o seu procedimento armazenado impressionante? contribuir para o repositório e crie um pedido pull! 

## <a name="next-steps"></a>Passos Seguintes
Depois de ter um ou mais procedimentos armazenados, acionadores e funções definidas pelo utilizador criadas, pode carregá-los e visualizá-los no portal do Azure com o Data Explorer.

Podem também ser as seguintes referências e recursos úteis no seu caminho para saber mais sobre a programação de lado do servidor do Azure Cosmos dB:

* [SDKs do Azure Cosmos DB](sql-api-sdk-dotnet.md)
* [Studio do DocumentDB](https://github.com/mingaliu/DocumentDBStudio/releases)
* [JSON](http://www.json.org/) 
* [JavaScript ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
* [Extensibilidade de base de dados seguras e portáteis](http://dl.acm.org/citation.cfm?id=276339) 
* [Arquitetura de base de dados orientada a serviços](http://dl.acm.org/citation.cfm?id=1066267&coll=Portal&dl=GUIDE) 
* [Hospedar o Runtime do .NET no Microsoft SQL server](http://dl.acm.org/citation.cfm?id=1007669)
