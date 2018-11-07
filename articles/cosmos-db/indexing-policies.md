---
title: O Azure Cosmos DB, políticas de indexação | Documentos da Microsoft
description: Compreenda como a indexação funciona no Azure Cosmos DB. Saiba como configurar e alterar a política de indexação para indexação automática e melhor desempenho.
keywords: como a indexação funciona, automática a indexação, a indexação da base de dados
services: cosmos-db
author: rafats
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: rafats
ms.openlocfilehash: cd3b5f49788282b535f07c6f84bf7e4002132ab9
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51237592"
---
# <a name="how-does-azure-cosmos-db-index-data"></a>Como funciona o Azure Cosmos DB indexa dados?

Por predefinição, todos os dados do Azure Cosmos DB é indexada. Embora muitos clientes são satisfeitos em permitir que processam automaticamente todos os aspetos da indexação do Azure Cosmos DB, pode especificar um personalizado *política de indexação* para coleções durante a criação no Azure Cosmos DB. Políticas de indexação no Azure Cosmos DB são mais flexíveis e eficientes do que os índices secundários que estão disponíveis em outras plataformas de base de dados. No Azure Cosmos DB, pode criar e personalizar a forma do índice sem sacrificar a flexibilidade de esquema. 

Para saber como funciona a indexação no Azure Cosmos DB, é importante compreender que quando gere a política de indexação, pode fazer refinados compromissos entre a sobrecarga de armazenamento de índice, escrita e débito de consulta e consistência das consultas.  

O vídeo seguinte, Andrew Liu gerente de programa do DB Cosmos Azure demonstra as capacidades e como otimizar e configurar a política de indexação no seu contentor do Azure Cosmos DB a indexação automática do Azure Cosmos DB. 

>[!VIDEO https://www.youtube.com/embed/uFu2D-GscG0]

Neste artigo, vamos dar uma visualização detalhada do Azure Cosmos DB políticas, como pode personalizar a política de indexação e compensações associadas de indexação. 

Depois de ler este artigo, será capaz de responder às seguintes perguntas:

* Como posso substituir as propriedades para incluir ou excluir da indexação?
* Como posso configurar o índice para atualizações eventual?
* Como posso configurar a indexação para executar consultas de ORDER BY ou um intervalo?
* Como posso fazer alterações à política de indexação de uma coleção?
* Como se compara o armazenamento e desempenho de diferentes políticas de indexação?

## <a id="Indexing"></a> Indexação do cosmos DB

O objetivo de índices de base de dados é servir consultas em suas várias formas e formas com consumo de recursos mínimo (como CPU e de entrada/saída), ao mesmo tempo, bom débito e baixa latência. Muitas vezes, a escolha do índice direito para consultar uma base de dados exige muito planejamento e a experimentação. Essa abordagem coloca um desafio para esquema de bases de dados onde os dados não está em conformidade com um esquema restrito e se desenvolve rapidamente. 

Por conseguinte, quando projetamos o subsistema de indexação do Cosmos DB, definimos os seguintes objetivos:

* Indexar documentos sem a necessidade de esquema: O subsistema de indexação não necessita de quaisquer informações de esquema ou fazer qualquer suposição sobre o esquema dos documentos.  

* Suporte para consultas relacionais e hierárquicas avançadas e eficientes: O índice oferece suporte a linguagem de consulta do Cosmos DB com eficiência, incluindo suporte para as projeções relacionais e hierárquicas.  

* Suporte para consultas consistentes face de um volume constante de gravações: escrita elevada a cargas de trabalho de débito com consultas consistentes, o índice ser atualizado incrementalmente, eficiência e online diante de um volume constante de gravações. A atualização de índice consistente é crucial para servir consultas no nível de consistência em que o utilizador configurado o serviço de documentos.  

* Suporte para vários inquilinos: tendo em conta o modelo baseado em reserva para governação de recursos em inquilinos, atualizações de índice são executadas dentro do orçamento de recursos do sistema (CPU, memória e operações de entrada/saída por segundo) alocados por réplica.  

* Eficiência de armazenamento: para a relação custo-eficácia, a sobrecarga de armazenamento em disco do índice é vinculado e previsível. Isso é crucial porque a Cosmos DB permite que o desenvolvedor a fazer compensações com base no custo entre a sobrecarga de índice em relação ao desempenho da consulta.  

## Personalizar a política de indexação de uma coleção <a id="CustomizingIndexingPolicy"></a>  
Pode personalizar as compensações entre armazenamento, escrita e desempenho de consulta e consistência das consultas ao substituir a predefinição de política numa coleção do Azure Cosmos DB de indexação. Pode configurar os seguintes aspetos:

* **Incluir ou excluir documentos e caminhos de e para o índice**. Pode excluir ou incluir documentos específicos no índice ao inserir ou substituir os documentos na coleção. Também pode incluir ou excluir propriedades específicas de JSON, também denominadas *caminhos*, ser indexados em documentos que estão incluídos num índice. Os percursos de padrões de carateres universais.
* **Configurar vários tipos de índice**. Para cada caminho incluído, pode especificar o tipo de índice que requer que o caminho para uma coleção. Pode especificar o tipo de índice com base nos dados do caminho, a carga de trabalho de consulta esperado e numérica/cadeia de caracteres "precisão."
* **Configurar os modos de atualização de índice**. Azure Cosmos DB suporta três modos de indexação: consistente, lento e nenhum. Pode configurar os modos de indexação através da política de indexação numa coleção do Azure Cosmos DB. 

O seguinte trecho de código do Microsoft .NET mostra como definir uma política de indexação personalizada quando cria uma coleção. Neste exemplo, definimos a política com um índice do intervalo para cadeias de caracteres e números na precisão máxima. Pode utilizar esta política para executar consultas ORDER BY em cadeias de caracteres.

    DocumentCollection collection = new DocumentCollection { Id = "myCollection" };

    collection.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), collection);   


> [!NOTE]
> O esquema JSON para a versão da API de REST versão 2015-06-03 de política de indexação. Com essa versão, o esquema JSON para a política de indexação suporta índices de intervalo em relação a strings. SDK de .NET 1.2.0 e Java, Python e node. js SDKs 1.1.0 suportam o novo esquema de política. Versões anteriores do SDK utilizam a API de REST versão 2015-04-08. Eles oferecem suporte o esquema anterior para a política de indexação.
> 
> Por predefinição, do Azure Cosmos DB indexa todas as propriedades de cadeia de caracteres em documentos consistentemente com um índice de Hash. Indexação todas as propriedades numéricas em documentos de forma consistente com um índice do intervalo.  
> 
> 

### <a name="customize-the-indexing-policy-in-the-portal"></a>Personalizar a política de indexação no portal

Pode alterar a política de indexação de uma coleção no portal do Azure: 

1. No portal, aceda à sua conta do Azure Cosmos DB e, em seguida, selecione a coleção. 
2. No menu de navegação esquerdo, selecione **configurações**e, em seguida, selecione **política de indexação**. 
3. Sob **política de indexação**, altere a sua política de indexação e, em seguida, selecione **OK**. 

### Modos de indexação do banco de dados <a id="indexing-modes"></a>  
Azure Cosmos DB suporta três modos de indexação que pode configurar através da política de indexação numa coleção do Azure Cosmos DB: consistente, lento e nenhum.

**Consistente**: se a política de uma coleção Azure Cosmos DB é a consistência, as consultas numa coleção específica do Azure Cosmos DB, siga o mesmo nível de consistência especificados para o ponto-lê (sólido, prescrição vinculada, sessão ou eventual). O índice é atualizado de forma síncrona como parte da atualização de documentos (inserir, substituir, update e delete um documento numa coleção do Azure Cosmos DB).

Indexação consistente suporta consultas consistentes ao custo de uma possível redução no débito de escrita. Essa redução é uma função dos caminhos exclusivos que têm de ser indexados e o "nível de consistência". Indexação de modo consistente foi concebido para cargas de trabalho "escrever rapidamente, consulta imediatamente".

**Lento**: O índice é atualizado de forma assíncrona quando uma coleção do Azure Cosmos DB está inativo, ou seja, quando a capacidade de débito da coleção não é totalmente utilizada para servir pedidos de utilizador.  Tenha em atenção que poderá obter resultados inconsistentes porque os dados são ingeridos e indexados lentamente. Isso significa que as consultas de CONTAGEM ou resultados de consulta específica podem não ser consistente ou passível de repetição no devido tempo. 

O índice em geral, está no modo catch-up com dados ingeridos. Com Lazy indexação, o tempo para live (TTL) muda o resultado no índice que está a ser removido e recriado. Isso faz com que os resultados de consulta e CONTAGEM inconsistente durante um período de tempo. A maioria das contas do Azure Cosmos DB devem utilizar o modo de indexação consistente.

**Nenhum**: uma coleção que tenha um nenhum modo de índice não tem nenhum índice associado ao mesmo. Isto é normalmente utilizado se o Azure Cosmos DB é utilizado como um armazenamento de chave-valor e estão a ser acedidos apenas por sua propriedade de ID. 

> [!NOTE]
> Configurar a política de indexação com como None tem o efeito colateral de remover qualquer índice existente. Utilize esta opção se os padrões de acesso requerem apenas o ID ou ligação personalizada.
> 
> 

A tabela seguinte mostra a consistência para consultas com base no modo indexação (consistência e Lazy) configurada para a coleção e o nível de consistência especificado para o pedido de consulta. Isto aplica-se às consultas efetuadas utilizando qualquer interface: REST API, SDKs, ou a partir da procedimentos armazenados e acionadores. 

|Consistência|Modo de indexação: consistente|Modo de indexação: lenta|
|---|---|---|
|Forte|Forte|Eventual|
|Estagnação limitada|Estagnação limitada|Eventual|
|Sessão|Sessão|Eventual|
|Eventual|Eventual|Eventual|

O Azure Cosmos DB devolve um erro para consultas efetuadas em coleções que têm um nenhum modo de indexação. Consultas ainda podem ser executadas como análises via o explícita **x-ms-documentdb-enable-análise** cabeçalho na REST API ou o **EnableScanInQuery** pedido de opção com o SDK de .NET. Algumas funcionalidades de consulta, como ORDER BY não são suportadas como análises com **EnableScanInQuery**.

A tabela seguinte mostra a consistência para consultas com base no modo de indexação (consistência, Lazy e None) quando **EnableScanInQuery** está especificado.

|Consistência|Modo de indexação: consistente|Modo de indexação: lenta|Modo de indexação: nenhum|
|---|---|---|---|
|Forte|Forte|Eventual|Forte|
|Estagnação limitada|Estagnação limitada|Eventual|Estagnação limitada|
|Sessão|Sessão|Eventual|Sessão|
|Eventual|Eventual|Eventual|Eventual|

O seguir mostram de exemplo de código como criar uma coleção do Azure Cosmos DB com o SDK de .NET indexação consistente em todas as inserções de documentos.

     // Default collection creates a Hash index for all string fields and a Range index for all numeric    
     // fields. Hash indexes are compact and offer efficient performance for equality queries.

     var collection = new DocumentCollection { Id ="defaultCollection" };

     collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;

     collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("mydb"), collection);


### <a name="index-paths"></a>Caminhos de índice
O Azure Cosmos DB modela os documentos JSON e o índice como árvores. Pode otimizar a políticas para caminhos de dentro da árvore. Dentro de documentos, pode escolher os caminhos para incluir ou excluir da indexação. Isso pode oferecer desempenho aprimorado de escrita e de armazenamento de índice mais baixo para cenários em que os padrões de consulta são previamente conhecidos.

Caminhos de índice começar com a raiz (/) e, normalmente, terminar com o? operador de caráter universal. Isso indica que existem vários valores possíveis para o prefixo. Por exemplo, para servir de SELECT * FROM famílias F onde F.familyName = "Andrade", tem de incluir um caminho de índice para /familyName/? na política de índice da coleção.

Caminhos de índice também podem utilizar o \* operador de caráter universal para especificar o comportamento de caminhos recursivamente sob o prefixo. Por exemplo, / payload / * podem ser utilizadas para excluir tudo sob a propriedade de payload da indexação.

Aqui estão os padrões comuns para especificar caminhos de índice:

| Caminho                | Caso de utilização/descrição                                                                                                                                                                                                                                                                                         |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| /                   | Caminho predefinido da coleção. Recursiva e aplica-se a árvore do documento inteiro.                                                                                                                                                                                                                                   |
| / prop /?             | Caminho de índice necessário para servir consultas semelhante ao seguinte (com tipos de endereços ou intervalos de Hash, respectivamente):<br><br>SELECT FROM c coleção onde c.prop = "valor"<br><br>SELECT FROM c coleção onde c.prop > 5<br><br>SELECIONAR a partir de coleção c ORDER BY c.prop                                                                       |
| / prop / *             | Caminho de índice para todos os caminhos sob a etiqueta especificada. Funciona com as seguintes consultas<br><br>SELECT FROM c coleção onde c.prop = "valor"<br><br>SELECT FROM c coleção onde c.prop.subprop > 5<br><br>SELECT FROM c coleção onde c.prop.subprop.nextprop = "valor"<br><br>SELECIONAR a partir de coleção c ORDER BY c.prop         |
| [] / propriedades / /?         | Caminho de índice necessário para servir a iteração e Junte-se a consultas em matrizes de escalares, como ["a", "b", "c"]:<br><br>SELECIONAR etiqueta de etiqueta IN collection.props etiqueta de onde = "valor"<br><br>SELECIONAR etiqueta de coleção c associação marca IN c.props onde Etiquetar > 5                                                                         |
| /Props/ [] /subprop/? | Caminho de índice necessário para servir a iteração e consultas de JUNÇÃO em relação a matrizes de objetos, como [{subprop: "a"}, {subprop: "b"}]:<br><br>SELECIONAR etiqueta de etiqueta IN collection.props onde tag.subprop = "valor"<br><br>SELECIONAR etiqueta de coleção c associação marca IN c.props onde tag.subprop = "valor"                                  |
| / prop/subprop /?     | Caminho de índice necessário para servir consultas (com tipos de endereços ou intervalos de Hash, respectivamente):<br><br>SELECT FROM c coleção onde c.prop.subprop = "valor"<br><br>SELECT FROM c coleção onde c.prop.subprop > 5                                                                                                                    |

> [!NOTE]
> Quando definir os caminhos de índice personalizado, é necessário especificar a regra de indexação predefinida para a árvore de todo o documento, o que está em falta pelo caminho especial "/ *". 
> 
> 

O exemplo seguinte configura um caminho específico com o índice do intervalo e um valor de precisão personalizado de 20 bytes:

```
    var collection = new DocumentCollection { Id = "rangeSinglePathCollection" };    

    collection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/Title/?", 
            Indexes = new Collection<Index> { 
                new RangeIndex(DataType.String) { Precision = 20 } } 
            });

    // Default for everything else
    collection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/*" ,
            Indexes = new Collection<Index> {
                new HashIndex(DataType.String) { Precision = 3 }, 
                new RangeIndex(DataType.Number) { Precision = -1 } 
            }
        });

    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), pathRange);
```

Quando um caminho é adicionado para indexação, números e cadeias de caracteres dentro desses caminhos são indexadas. Portanto, mesmo que define a indexação para apenas cadeias de caracteres, o Azure Cosmos DB adiciona definição padrão para números também. Em outras palavras, o Azure Cosmos DB tem a capacidade de exclusão de caminho de política de indexação, mas não escrever exclusão de um caminho específico. Segue-se um exemplo, tenha em atenção que apenas um índice é especificado para os dois caminhos (caminho = "/ *" e caminho = "/\"attr1\"/?"), mas o tipo de dados de número também é adicionado ao resultado.

```
var indices = new[]{
                new IncludedPath  {
                    Indexes = new Collection<Index>
                    {
                        new RangeIndex(DataType.String) { Precision = 3 }// <- note: only 1 index specified
                    },
                    Path =  "/*"
                },
                new IncludedPath  {
                    Indexes = new Collection<Index>
                    {
                        new RangeIndex(DataType.String) { Precision = 3 } // <- note: only 1 index specified
                    },
                    Path =  "/\"attr1\"/?"
                }
            };...

            foreach (var index in indices)
            {
                documentCollection.IndexingPolicy.IncludedPaths.Add(index);
            }
```

Resultado da criação de índices:

```json
{
    "indexingMode": "consistent",
    "automatic": true,
    "includedPaths": [
        {
            "path": "/*",
            "indexes": [
                {
                    "kind": "Range",
                    "dataType": "String",
                    "precision": 3
                },
                {
                    "kind": "Range",
                    "dataType": "Number",
                    "precision": -1
                }
            ]
        },
        {
            "path": "/\"attr\"/?",
            "indexes": [
                {
                    "kind": "Range",
                    "dataType": "String",
                    "precision": 3
                },
                {
                    "kind": "Range",
                    "dataType": "Number",
                    "precision": -1
                }
            ]
        }
    ],
}
```

### <a name="index-data-types-kinds-and-precisions"></a>Tipos de dados do índice, tipos e precisions
Tem várias opções quando configura a política de indexação para um caminho. Pode especificar uma ou mais definições de indexação para cada caminho:

* **Tipo de dados**: cadeia de caracteres, número, ponto, polígono ou LineString (pode conter apenas uma entrada por tipo de dados por caminho).
* **Tipo de índice**: Hash (consultas de igualdade), intervalo (igualdade, intervalo ou ORDER BY consultas) ou Geográ (consultas espaciais).
* **Precisão**: índice de Hash, isso varia de 1 a 8 para cadeias de caracteres e números. A predefinição é 3. Para um índice do intervalo, este valor pode ser -1 (precisão máxima). Ele pode variar from between 1 e 100 (precisão máxima) para a cadeia de caracteres ou valores numéricos.

#### <a name="index-kind"></a>Tipo de índice
O Azure Cosmos DB suporta o índice de Hash e tipos de índice do intervalo para cada caminho que pode ser configurado para tipos de dados de cadeia de caracteres ou um número, ou ambos.

* **Hash** oferece suporte a igualdade eficiente e consultas de JUNÇÃO. Para a maioria dos casos de utilização, os índices de Hash não tem uma precisão maior que o valor predefinido de 3 bytes. O tipo de dados pode ser a cadeia de caracteres ou número.
* **Intervalo** suporta consultas de igualdade eficiente, consultas de intervalo (usando >, <>, =, < =,! =) e consultas de ORDER BY. ORDER By consultas por predefinição também exigem a precisão de índice máximo (-1). O tipo de dados pode ser a cadeia de caracteres ou número.

O Azure Cosmos DB também suporta o tipo de índice espacial para cada caminho que pode ser especificado para os tipos de dados LineString, Polygon ou ponto. O valor no caminho especificado tem de ser um fragmento de GeoJSON válido, como `{"type": "Point", "coordinates": [0.0, 10.0]}`.

* **Espaciais** suporta eficiente espaciais (dentro e a distância) consultas. O tipo de dados pode ser LineString, Polygon ou ponto.

> [!NOTE]
> Azure Cosmos DB suporta a indexação automática de LineString, Polygon e ponto de tipos de dados.
> 
> 

Eis os tipos de índice suportados e exemplos de consultas que podem ser utilizadas para fornecer:

| Tipo de índice | Caso de utilização/descrição                                                                                                                                                                                                                                                                                                                                                                                                              |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Hash       | Hash sobre/prop /? (ou /) pode ser utilizado para servir as seguintes consultas de forma eficiente:<br><br>SELECT FROM c coleção onde c.prop = "valor"<br><br>Hash sobre/propriedades / [] /? (ou / ou/propriedades /) pode ser utilizado para servir as seguintes consultas de forma eficiente:<br><br>SELECIONAR etiqueta de coleção c associação marca IN c.props marca onde = 5                                                                                                                       |
| Intervalo      | Intervalo em/prop /? (ou /) pode ser utilizado para servir as seguintes consultas de forma eficiente:<br><br>SELECT FROM c coleção onde c.prop = "valor"<br><br>SELECT FROM c coleção onde c.prop > 5<br><br>SELECIONAR a partir de coleção c ORDER BY c.prop                                                                                                                                                                                                              |
| Espacial     | Intervalo em/prop /? (ou /) pode ser utilizado para servir as seguintes consultas de forma eficiente:<br><br>SELECT FROM c de coleção<br><br>WHERE ST_DISTANCE(c.prop, {"type": "Point", "coordinates": [0.0, 10.0]}) < 40<br><br>SELECIONE de coleção c onde ST_WITHIN(c.prop, {"type": "Polygon",...}) – indexação nos pontos ativados<br><br>SELECIONE de coleção c onde ST_WITHIN({"type": "Point",...}, c.prop) – com indexação de polígonos ativados              |

Por predefinição, é devolvido um erro para consultas com operadores de intervalo como > = se existe um índice de intervalo (de qualquer precisão) para sinalizar que uma análise poderá ser necessária para servir a consulta. Consultas de intervalo podem ser executadas sem um índice do intervalo, utilizando o **x-ms-documentdb-enable-análise** cabeçalho na REST API ou o **EnableScanInQuery** pedido de opção com o SDK de .NET. Se existirem quaisquer outros filtros na consulta que o Azure Cosmos DB pode usar o índice para filtrar contra, nenhum erro é retornado.

As mesmas regras aplicam-se para consultas espaciais. Por predefinição, é devolvido um erro para consultas espaciais se existe um índice geográfico, e existem não existem outros filtros que podem ser fornecidos de índice. Eles podem ser executados como uma análise utilizando **x-ms-documentdb-enable-análise** ou **EnableScanInQuery**.

#### <a name="index-precision"></a>Precisão de índice
Pode usar a precisão de índice para fazer compensações entre o armazenamento de índice sobrecarga e o desempenho da consulta. Para números, recomendamos que utilize a configuração de precisão de padrão de -1 (máximo). Como os números são 8 bytes em JSON, isto é equivalente a uma configuração de 8 bytes. Escolher um valor inferior para precisão, por exemplo, 1 a 7, significa que mapeia os valores dentro de alguns intervalos para o mesmo índice de entrada. Portanto, reduzir o espaço de armazenamento de índice, mas a execução da consulta poderá ter de processar mais documentos. Por conseqüência, ele consome mais débito em unidades de pedido.

A configuração de precisão do índice tem mais prático do aplicativo com intervalos de cadeia de caracteres. Como cadeias de caracteres podem ser qualquer comprimento arbitrário, a escolha de precisão o índice poderá afetar o desempenho das consultas de intervalo de cadeia de caracteres. Também poderá afetar a quantidade de espaço de armazenamento de índice que é necessário. Índices de intervalo de cadeia de caracteres podem ser configurados com 1 a 100 ou -1 (máximo). Se quiser executar consultas de ORDER BY em Propriedades de cadeia de caracteres, tem de especificar uma precisão de -1 para os caminhos correspondentes.

Os índices espaciais utilizam sempre a precisão de índice predefinido para todos os tipos (ponto, LineString e polígonos). A precisão de índice predefinido para os índices espaciais não pode ser substituída. 

O exemplo seguinte mostra como aumentar a precisão para índices de intervalo de uma coleção com o SDK de .NET. 

**Criar uma coleção com uma precisão de índice personalizado**

    var rangeDefault = new DocumentCollection { Id = "rangeCollection" };

    // Override the default policy for strings to Range indexing and "max" (-1) precision
    rangeDefault.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), rangeDefault);   


> [!NOTE]
> O Azure Cosmos DB devolve um erro quando uma consulta utiliza ORDER BY, mas não tem um índice do intervalo contra o caminho de consultados com a precisão máxima. 
> 
> 

Da mesma forma, pode excluir completamente caminhos de indexação. O exemplo seguinte mostra como excluir uma seção inteira de documentos (um *subárvore*) de indexação ao utilizar o \* operador de caráter universal.

    var excluded = new DocumentCollection { Id = "excludedPathCollection" };
    excluded.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
    excluded.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*" });

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), excluded);



## <a name="opt-in-and-opt-out-of-indexing"></a>Opção ativa de participação e a opção de desativar a indexação
Pode escolher se pretende que a coleção para indexar automaticamente todos os documentos. Por predefinição, todos os documentos são indexados automaticamente, mas pode desativar a indexação automática. Quando a indexação é desativada, documentos podem ser acedidos através de apenas seus auto-ligações ou por consultas utilizando o documento ID.

Com a indexação automática desativada, pode adicionar ainda seletivamente apenas os documentos específicos para o índice. Por outro lado, pode deixar a indexação automática e seletivamente optar por excluir documentos específicos. Indexação/desativar as configurações são úteis quando tem apenas um subconjunto de documentos que tem de ser consultado.

O exemplo a seguir mostra como incluir um documento explicitamente ao utilizar o [SDK de .NET API de SQL](https://docs.microsoft.com/azure/cosmos-db/sql-api-sdk-dotnet) e o [RequestOptions.IndexingDirective](https://msdn.microsoft.com/library/microsoft.azure.documents.client.requestoptions.indexingdirective.aspx) propriedade.

    // If you want to override the default collection behavior to either
    // exclude (or include) a document in indexing,
    // use the RequestOptions.IndexingDirective property.
    client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        new { id = "AndersenFamily", isRegistered = true },
        new RequestOptions { IndexingDirective = IndexingDirective.Include });

## <a name="modify-the-indexing-policy-of-a-collection"></a>Modificar a política de indexação de uma coleção
No Azure Cosmos DB, pode efetuar alterações à política de indexação de uma coleção em tempo real. Uma alteração na política numa coleção do Azure Cosmos DB de indexação pode levar a uma alteração na forma do índice. A alteração afetará os caminhos que podem ser indexados, sua precisão e o modelo de consistência do índice em si. Uma alteração na política de indexação com eficiência requer uma transformação do índice antigo num novo índice. 

**Transformações de índice online**

![Como funciona a indexação – transformações de índice online do Azure Cosmos DB](./media/indexing-policies/index-transformations.png)

Transformações de índice são feitas online. Isso significa que os documentos indexados pela política antiga com eficiência são transformados pela nova política *sem afetar a disponibilidade de escrita ou o débito aprovisionado* da coleção. A consistência de leitura e escrita operações feitas com o uso da API REST, SDKs, ou a partir da procedimentos armazenados e acionadores não é afetada durante a transformação de índice. 

Alterar a política de indexação é um processo assíncrono e o tempo para concluir a operação depende do número de documentos, de RUs aprovisionadas e de tamanho de documentos. Enquanto a reindexação está em curso, a sua consulta pode não devolver todos os resultados se a consulta utiliza o índice que está a ser modificado e consultas correspondente não retornará quaisquer erros/falhas. Enquanto a reindexação está em curso, as consultas são eventualmente consistentes, independentemente da configuração de modo indexação (consistência ou Lazy). Depois do índice de transformação estiver concluída, irá continuar a ver resultados consistentes. Também se aplica às consultas de todas as interfaces: REST API, SDKs e a partir da procedimentos armazenados e acionadores. Assim como com Lazy indexação, a transformação de índice é executada de forma assíncrona em segundo plano nas réplicas através dos recursos sobressalentes disponíveis para uma réplica específica. 

Transformações de índice também são feitas no local. O Azure Cosmos DB não mantém duas cópias do índice e troca horizontalmente o índice antigo pelo novo. Isso significa que não existe espaço em disco adicional é necessário ou consumido nos seus conjuntos, enquanto ocorrem de transformações de índice.

Quando alterar a política de indexação, as alterações são aplicadas para mover do índice antigo para o novo principalmente com base nas configurações de modo indexação. Indexação de modo de configurações desempenham um papel maior do que outros valores, como caminhos incluído/excluído, tipos de índice e precisions. 

Se seus antigas e novas políticas de ambos os usar indexação consistentes, o Azure Cosmos DB executa uma transformação de índice online. Não é possível aplicar a outra alteração na diretiva indexação que tenha o modo de indexação consistente enquanto a transformação está em curso. No entanto, pode avançar para Lazy ou nenhum modo de indexação a uma transformação está em curso: 

* Ao mover para Lazy, a alteração de política do índice tem efeita imediato. O Azure Cosmos DB começa a recriação do índice de uma forma assíncrona. 
* Quando move como None, o índice é removido imediatamente. Mover para None é útil quando deseja cancelar uma transformação em andamento e comece do zero com uma política de indexação diferente. 

O fragmento de código seguinte mostra como modificar política de indexação uma coleção do modo de indexação consistente para o modo de indexação lento. Se estiver a utilizar o SDK de .NET, pode disparar uma alteração de política de indexação pela utilização da nova **ReplaceDocumentCollectionAsync** método.

**Modificar a política de indexação de consistência para Lazy**

    // Switch to Lazy indexing mode.
    Console.WriteLine("Changing from Default to Lazy IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.Lazy;

    await client.ReplaceDocumentCollectionAsync(collection);

**Controlar o progresso da transformação de índice**

Pode controlar o progresso de porcentagem da transformação de índice para um índice consistente utilizando o **IndexTransformationProgress** propriedade de resposta de um **ReadDocumentCollectionAsync** chamar. Outros SDKs e da API REST, suportam a métodos e propriedades equivalentes para fazer alterações de política de indexação. Pode verificar o progresso de uma transformação de índice para um índice consistente ao chamar **ReadDocumentCollectionAsync**: 

    long smallWaitTimeMilliseconds = 1000;
    long progress = 0;

    while (progress < 100)
    {
        ResourceResponse<DocumentCollection> collectionReadResponse = await client.ReadDocumentCollectionAsync(
            UriFactory.CreateDocumentCollectionUri("db", "coll"));

        progress = collectionReadResponse.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromMilliseconds(smallWaitTimeMilliseconds));
    }

> [!NOTE]
> * O **IndexTransformationProgress** propriedade é aplicável apenas quando transformaram a um índice consistente. Utilize o **ResourceResponse.LazyIndexingProgress** propriedade para transformações de controlo para um índice lento.
> * O **IndexTransformationProgress** e o **LazyIndexingProgress** propriedades são preenchidas apenas para uma coleção não particionada, ou seja, uma coleção que for criada sem uma chave de partição.
>

Pode remover o índice de uma coleção, movendo para nenhum modo de indexação. Isso pode ser uma ferramenta operacional útil se pretender cancelar uma transformação em curso e, em seguida, começar de imediato uma nova.

**Remova o índice de uma coleção**

    // Switch to Lazy indexing mode.
    Console.WriteLine("Dropping index by changing to the None IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.None;

    await client.ReplaceDocumentCollectionAsync(collection);

Quando seria são efetuadas alterações de política de indexação a suas coleções do Azure Cosmos DB? Seguem-se os casos de utilização mais comuns:

* Servir resultados consistentes durante o funcionamento normal, mas reverta para o modo de indexação lento durante importações de dados em massa.
* Começar a utilizar os novos recursos de indexação no suas coleções do Azure Cosmos DB atuais. Por exemplo, pode utilizar consultas geoespaciais, que requer que o tipo de índice geográfico ou ORDER BY / consultas de intervalo, que requerem a cadeia de tipo de índice do intervalo de cadeias de caracteres.
* Selecione a mão as propriedades ser indexados e alterá-los ao longo do tempo.
* Otimize a precisão de indexação para melhorar o desempenho de consulta ou para reduzir o armazenamento consumido.

> [!NOTE]
> Para modificar a política de indexação usando **ReplaceDocumentCollectionAsync**, tem de utilizar a versão 1.3.0 ou uma versão posterior do SDK do .NET.
> 
> Para transformação de índice concluir com êxito, certifique-se de que existe espaço de armazenamento livre suficiente disponível na coleção. Se a coleção de atingir a quota de armazenamento, a transformação de índice é colocada em pausa. Transformação de índice retoma automaticamente quando o espaço de armazenamento está disponível, por exemplo, se eliminar alguns documentos.
> 
> 

## <a name="performance-tuning"></a>Otimização do desempenho
As APIs de SQL fornecem informações sobre as métricas de desempenho, como o armazenamento de índice utilizado e o custo de débito (unidades de pedido) para cada operação. Pode usar essas informações para comparar várias políticas de indexação e para o ajuste de desempenho.

Para verificar a quota de armazenamento e a utilização de uma coleção, execute uma **HEAD** ou **obter** pedido em relação ao recurso de coleção. Em seguida, Inspecione o **x-ms-pedido-quota** e o **x-ms-pedido-utilização** cabeçalhos. No SDK do .NET, o [DocumentSizeQuota](https://msdn.microsoft.com/library/dn850325.aspx) e [DocumentSizeUsage](https://msdn.microsoft.com/library/azure/dn850324.aspx) propriedades no [ResourceResponse < T\> ](https://msdn.microsoft.com/library/dn799209.aspx) conter estes valores correspondentes.

     // Measure the document size usage (which includes the index size) against   
     // different policies.
     ResourceResponse<DocumentCollection> collectionInfo = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));  
     Console.WriteLine("Document size quota: {0}, usage: {1}", collectionInfo.DocumentQuota, collectionInfo.DocumentUsage);


Para medir a sobrecarga de indexação de cada operação de escrita (criar, atualizar ou eliminar), Inspecione o **x-ms--de encargos de pedidos** cabeçalho (ou o equivalente [RequestCharge](https://msdn.microsoft.com/library/dn799099.aspx) propriedade no [ ResourceResponse < T\> ](https://msdn.microsoft.com/library/dn799209.aspx) no SDK do .NET) para medir o número de unidades de pedido são consumidas por essas operações.

     // Measure the performance (request units) of writes.     
     ResourceResponse<Document> response = await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), myDocument);              
     Console.WriteLine("Insert of document consumed {0} request units", response.RequestCharge);

     // Measure the performance (request units) of queries.    
     IDocumentQuery<dynamic> queryable =  client.CreateDocumentQuery(UriFactory.CreateDocumentCollectionUri("db", "coll"), queryString).AsDocumentQuery();

     double totalRequestCharge = 0;
     while (queryable.HasMoreResults)
     {
        FeedResponse<dynamic> queryResponse = await queryable.ExecuteNextAsync<dynamic>(); 
        Console.WriteLine("Query batch consumed {0} request units",queryResponse.RequestCharge);
        totalRequestCharge += queryResponse.RequestCharge;
     }

     Console.WriteLine("Query consumed {0} request units in total", totalRequestCharge);

## <a name="changes-to-the-indexing-policy-specification"></a>Alterações a especificação de política de indexação
Uma alteração no esquema para a política de indexação foi introduzida a 7 de Julho de 2015, com a API de REST versão 2015-06-03. As classes correspondentes nas versões do SDK tem novas implementações de acordo com o esquema. 

As seguintes alterações foram implementadas na especificação de JSON:

* Política de indexação suporta índices de intervalo para cadeias de caracteres.
* Cada caminho pode ter várias definições de índice. Ele pode ter um para cada tipo de dados.
* Precisão de indexação suporta 1 a 8 para números, 1 a 100 para cadeias de caracteres e -1 (precisão máxima).
* Segmentos de caminho não necessitam de uma aspas duplas para cada caminho de escape. Por exemplo, pode adicionar um caminho para   **/título /?** em vez de **/ "title" /?**.
* O caminho de raiz que representa a "todos os caminhos" pode ser representado como **/ \*** (além **/**).

Se tiver o código que coleções de provisões com uma política de indexação personalizada escrita com a versão 1.1.0 do SDK do .NET ou uma versão anterior, para mover para o SDK versão 1.2.0, tem de alterar o código da aplicação para processar estas alterações. Se não tiver o código que configura a política de indexação ou, se pretender continuar a utilizar uma versão anterior do SDK, não são necessárias alterações.

Para obter uma comparação prática, eis um exemplo de uma política de indexação personalizada escrito com a API de REST versão 2015-06-03, seguido da mesma política de indexação escrita com a API de REST versão 2015-04-08 anteriores.

**Atual JSON (REST API versão 2015-06-03) de política de indexação**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "includedPaths":[
          {
             "path":"/*",
             "indexes":[
                {
                   "kind":"Hash",
                   "dataType":"String",
                   "precision":3
                },
                {
                   "kind":"Hash",
                   "dataType":"Number",
                   "precision":7
                }
             ]
          }
       ],
       "ExcludedPaths":[
          {
             "path":"/nonIndexedContent/*"
          }
       ]
    }


**Política JSON (REST API versão 2015-04-08) de indexação anteriormente**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "IncludedPaths":[
          {
             "IndexType":"Hash",
             "Path":"/",
             "NumericPrecision":7,
             "StringPrecision":3
          }
       ],
       "ExcludedPaths":[
          "/\"nonIndexedContent\"/*"
       ]
    }


## <a name="next-steps"></a>Passos Seguintes
Para exemplos de gestão de política de índice e para saber mais sobre a linguagem de consulta do Azure Cosmos DB, veja as ligações seguintes:

* [Exemplos de código de gestão de índice de .NET API de SQL](https://github.com/Azure/azure-documentdb-net/blob/master/samples/code-samples/IndexManagement/Program.cs)
* [Operações de coleção de REST de API de SQL](https://msdn.microsoft.com/library/azure/dn782195.aspx)
* [Consulta com o SQL](sql-api-sql-query.md)

