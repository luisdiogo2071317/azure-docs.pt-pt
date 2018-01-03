---
title: "BD do Azure do Cosmos indexação políticas | Microsoft Docs"
description: "Compreenda como indexação funciona do BD Azure Cosmos. Saiba como configurar e alterar a política de indexação para indexação automática e melhor desempenho."
keywords: "como a indexação funciona, automática indexação, a indexação de base de dados"
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: d5e8f338-605d-4dff-8a61-7505d5fc46d7
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 08/17/2017
ms.author: arramac
ms.openlocfilehash: b09f5323f0378721412baade9be9926ebd0c171e
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/03/2018
---
# <a name="how-does-azure-cosmos-db-index-data"></a>Como funciona a dados do índice de BD do Cosmos do Azure?

Por predefinição, todos os dados de base de dados do Azure Cosmos está indexada. Embora muitos clientes estiverem satisfeitos permitir que a base de dados do Cosmos do Azure processam automaticamente todos os aspetos da indexação, pode especificar um personalizado *política de indexação* para coleções durante a criação do BD Azure Cosmos. As políticas de indexação do BD Azure Cosmos são mais flexível e de índices secundários que são disponibilizados em outras plataformas de base de dados. Na base de dados do Azure Cosmos, pode criar e personalizar a forma do índice sem sacrificar a flexibilidade de esquema. 

Para saber como indexação funciona do BD Azure Cosmos, é importante compreender que quando gere a política de indexação, pode efetuar detalhados compromissos entre tolerância de armazenamento de índice, escrita e débito de consulta e consistência de consulta.  

Neste artigo, vamos observe fechar Azure Cosmos DB indexação políticas, como personalizar a política de indexação e compromissos associados. 

Depois de ler este artigo, poderá responder às seguintes questões:

* Como pode a substituir as propriedades para incluir ou excluir da indexação?
* Como posso configurar o índice para atualizações de uma eventual?
* Como configurar a indexação para executar consultas de ORDER BY ou um intervalo?
* Como efetuar alterações à política de indexação de uma coleção?
* Como comparar armazenamento e o desempenho da indexação de políticas diferentes?

## Personalizar a política de indexação de uma coleção<a id="CustomizingIndexingPolicy"></a>  
Pode personalizar os compromissos entre o armazenamento, escrita e desempenho das consultas e consistência de consulta ao substituir a predefinição de indexação de política de uma coleção de BD do Cosmos do Azure. Pode configurar os seguintes aspetos:

* **Incluir ou excluir os documentos e caminhos de e para o índice**. Pode excluir ou incluir documentos específicos no índice ao inserir ou substituir os documentos na coleção. Também pode incluir ou excluir propriedades JSON específicas, também denominadas *caminhos*, ser indexados em documentos que estão incluídos num índice. Caminhos incluem padrões de carateres universais.
* **Configurar vários tipos de índice**. Para cada caminho incluído, pode especificar o tipo de índice que requer que o caminho para uma coleção. Pode especificar o tipo de índice com base em dados do caminho, a carga de trabalho consulta esperado e numérica/cadeia "precisão."
* **Configurar os modos de atualização do índice**. BD do Azure do Cosmos suporta três modos de indexação: consistente, lento e nenhum. Pode configurar os modos de indexação através da política de indexação de uma coleção de BD do Cosmos do Azure. 

O seguinte fragmento de código do Microsoft .NET mostra como definir uma política de indexação personalizada quando cria uma coleção. Neste exemplo, vamos definir a política com um índice de intervalos de cadeias e números de cada a precisão máxima. Pode utilizar esta política para executar consultas de ORDER BY em relação a cadeias.

    DocumentCollection collection = new DocumentCollection { Id = "myCollection" };

    collection.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), collection);   


> [!NOTE]
> O esquema JSON para a política de indexação alterado com a versão da API de REST versão 2015-06-03. Com essa versão, o esquema JSON para a política de indexação suporta índices de intervalo em relação a cadeias. .NET SDK 1.2.0 e Java, Python e Node.js SDKs 1.1.0 suportam o novo esquema de política. Versões anteriores do SDK utilizam a API de REST versão 2015-04-08. Suportam o esquema anterior para a política de indexação.
> 
> Por predefinição, base de dados do Azure Cosmos indexa todas as propriedades de cadeia dentro de documentos consistentemente com um índice de Hash. -Consistentemente indexa todas as propriedades de um valor numérico dentro de documentos com um índice de intervalo.  
> 
> 

### <a name="customize-the-indexing-policy-in-the-portal"></a>Personalizar a política de indexação no portal

Pode alterar a política de indexação de uma coleção no portal do Azure: 

1. No portal, aceda à sua conta de base de dados do Azure Cosmos e, em seguida, selecione a coleção. 
2. No menu de navegação esquerdo, selecione **definições**e, em seguida, selecione **política de indexação**. 
3. Em **política de indexação**, altere a sua política de indexação e, em seguida, selecione **OK**. 

### Modos de indexação de base de dados<a id="indexing-modes"></a>  
BD do Azure do Cosmos suporta três modos de indexação que pode ser configurada através da política de indexação de uma coleção de base de dados do Azure Cosmos: consistente, lento e nenhum.

**Consistente**: se a política de uma coleção de base de dados do Azure Cosmos é consistente, as consultas de uma coleção específica da base de dados do Azure Cosmos siga o mesmo nível de consistência especificados para as ponto de leituras (forte, consistência vinculada, sessão, ou eventual). O índice é atualizado de forma síncrona como parte da atualização de documento (inserir, substituir, atualização e eliminar um documento numa coleção de base de dados do Azure Cosmos).

A indexação consistente suporta consultas consistentes, mas utiliza uma redução possíveis débito de escrita. Este redução é uma função caminhos exclusivo que precisam de ser indexados e "nível de consistência." Modo de indexação consistente foi concebido para cargas de trabalho "escrever rapidamente, consulta imediatamente".

**Em diferido**: O índice é atualizado de forma assíncrona quando uma coleção de base de dados do Azure Cosmos quiescent, ou seja, quando a capacidade de débito da coleção não é totalmente utilizada para servir pedidos do utilizador. O modo de indexação lento poderá ser adequado para "de inserção agora, consulta mais tarde" as cargas de trabalho que necessitam de ingestão de documento. Tenha em atenção que poderá obter resultados inconsistentes porque os dados são ingeridos e indexados lentamente. Isto significa que as consultas de CONTAGEM ou consulta específicas resultados poderão não ser consistente ou repeatable em qualquer momento. 

O índice é, geralmente, no modo catch-up com dados de transmissões em. Com Lazy indexação, o tempo (TTL) em direto alterações resultado no índice que está a ser removido e recriado. Isto faz com que os resultados de consulta e CONTAGEM inconsistente durante um período de tempo. Por este motivo, a maioria das contas de base de dados do Azure Cosmos devem utilizar o modo de indexação consistente.

**Nenhum**: uma coleção que tenha um nenhum modo de índice não tem nenhum índice associado. Isto é normalmente utilizado se a BD do Cosmos do Azure é utilizado como um armazenamento de chave-valor e estão a ser acedidos apenas pela respetiva propriedade ID. 

> [!NOTE]
> Configurar a política de indexação com como None tem o efeito de remover um índice existente. Utilize esta opção se os seus padrões de acesso necessitam apenas de ID ou ligação personalizada.
> 
> 

A tabela seguinte mostra a consistência do consultas baseadas em modo indexação (Consistent e Lazy) configurada para a coleção e o nível de consistência especificado para o pedido de consulta. Isto aplica-se para consultas efetuadas utilizando qualquer interface: REST API, SDKs, ou a partir do e procedimentos armazenados acionadores. 

|Consistência|Modo de indexação: consistente|Modo de indexação: lento|
|---|---|---|
|Forte|Forte|Eventual|
|Obsoletismo|Obsoletismo|Eventual|
|Sessão|Sessão|Eventual|
|Eventual|Eventual|Eventual|

BD do Azure do Cosmos devolve um erro para consultas efetuadas em coleções que tenham um nenhum modo de indexação. Ainda pode executar consultas como análises através de explícita **x-ms-documentdb-enable-análise** cabeçalho na REST API ou o **EnableScanInQuery** pedir opção utilizando o SDK .NET. Algumas funcionalidades de consulta, como ordenar por, não são suportadas como análises com **EnableScanInQuery**.

A tabela seguinte mostra a consistência do consultas baseadas no modo de indexação (Consistent, Lazy e nenhum) quando **EnableScanInQuery** está especificado.

|Consistência|Modo de indexação: consistente|Modo de indexação: lento|Modo de indexação: nenhuma|
|---|---|---|---|
|Forte|Forte|Eventual|Forte|
|Obsoletismo|Obsoletismo|Eventual|Obsoletismo|
|Sessão|Sessão|Eventual|Sessão|
|Eventual|Eventual|Eventual|Eventual|

A mostrar de exemplo de código seguinte como criar uma coleção de BD do Cosmos Azure utilizando o SDK .NET com indexação consistente em todos os inserções de documento.

     // Default collection creates a Hash index for all string fields and a Range index for all numeric    
     // fields. Hash indexes are compact and offer efficient performance for equality queries.

     var collection = new DocumentCollection { Id ="defaultCollection" };

     collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;

     collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("mydb"), collection);


### <a name="index-paths"></a>Caminhos de índice
BD do Azure do Cosmos modelos documentos JSON e o índice como árvores. Pode otimizar políticas para caminhos de árvore. Dentro de documentos, pode escolher os caminhos para incluir ou excluir da indexação. Isto pode oferecer desempenho melhorado de escrita e de armazenamento de índice inferior para cenários em que os padrões de consulta são conhecidos previamente.

Caminhos de índice começar a utilizar a raiz (/) e, normalmente, terminar com o? operador de caráter universal. Isto indica que existem vários valores possíveis para o prefixo. Por exemplo, para servir SELECIONAR * de famílias F onde F.familyName = ou "seja", tem de incluir um caminho de índice para /familyName/? na política de índice da coleção.

Caminhos de índice também podem utilizar o \* operador de caráter universal para especificar o comportamento de caminhos recursivamente o prefixo. Por exemplo, payload / * pode ser utilizado para excluir tudo sob a propriedade de payload da indexação.

Seguem-se os padrões comuns para especificar os caminhos de índice:

| Caminho                | Caso de utilização/descrição                                                                                                                                                                                                                                                                                         |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| /                   | Caminho predefinido para a coleção. Recursiva e aplica-se para a árvore de documentos completo.                                                                                                                                                                                                                                   |
| prop /?             | Caminho de índice necessário para efetuar consultas semelhante ao seguinte (com Hash ou um intervalo de tipos, respetivamente):<br><br>SELECIONE da coleção c WHERE c.prop = "valor"<br><br>SELECIONE da coleção c WHERE c.prop > 5<br><br>SELECIONE a partir da coleção c ORDER BY c.prop                                                                       |
| / prop / *             | Caminho de índice para todos os caminhos sob a etiqueta especificada. Funciona com as seguintes consultas<br><br>SELECIONE da coleção c WHERE c.prop = "valor"<br><br>SELECIONE da coleção c WHERE c.prop.subprop > 5<br><br>SELECIONE da coleção c WHERE c.prop.subprop.nextprop = "valor"<br><br>SELECIONE a partir da coleção c ORDER BY c.prop         |
| [] propriedades / /?         | Caminho de índice necessários para servir iteração e associar as consultas em relação a matrizes de escalares comparáveis como ["a", "b", "c"]:<br><br>SELECIONE tag da tag IN collection.props tag onde = "valor"<br><br>SELECIONE tag da coleção c associação tag IN c.props onde tag > 5                                                                         |
| /Props/ [] /subprop/? | Caminho de índice necessários para efetuar iteração e consultas de associação em relação a matrizes de objetos como [{subprop: "a"}, {subprop: "b"}]:<br><br>SELECIONE tag da tag IN collection.props onde tag.subprop = "valor"<br><br>SELECIONE tag da coleção c associação tag IN c.props onde tag.subprop = "valor"                                  |
| / prop/subprop /?     | Caminho de índice necessário para efetuar consultas (com Hash ou um intervalo de tipos, respetivamente):<br><br>SELECIONE da coleção c WHERE c.prop.subprop = "valor"<br><br>SELECIONE da coleção c WHERE c.prop.subprop > 5                                                                                                                    |

> [!NOTE]
> Quando definir caminhos de índice personalizado, é necessário especificar a regra de indexação predefinida para a árvore de documentos completo, que está em falta que o caminho especiais "/ *". 
> 
> 

O exemplo a seguir configura um caminho específico com índice de intervalo e um valor de precisão personalizada de 20 bytes:

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


### <a name="index-data-types-kinds-and-precisions"></a>Tipos de dados do índice, tipos e precisions
Tem várias opções quando configura a política de indexação de um caminho. Pode especificar uma ou mais definições de indexação para cada caminho:

* **Tipo de dados**: cadeia, número, ponto, polígono ou LineString (pode conter apenas uma entrada por tipo de dados por caminho).
* **Tipo de índice**: Hash (consultas de igualdade), intervalo (igualdade, intervalo ou consultas de ORDER BY) ou Spatial (consultas geográficos).
* **Precisão**: o índice de Hash, isto varia entre 1 a 8 para cadeias e números. A predefinição é 3. Para um índice de intervalo, este valor pode ser -1 (precisão máxima). Pode variar a from between 1 e 100 (precisão máxima) para a cadeia ou valores numéricos.

#### <a name="index-kind"></a>Tipo de índice
BD do Azure do Cosmos suporta tipos de índice de intervalo e o índice de Hash para cada caminho que pode ser configurado para tipos de dados de cadeia ou um número, ou ambos.

* **Hash** suporta igualdade eficiente e consultas de associação. A maioria dos casos de utilização, índices de Hash não tem uma precisão maior que o valor predefinido de 3 bytes. O tipo de dados pode ser a cadeia ou um número.
* **Intervalo** suporta consultas de igualdade eficiente, consultas de intervalo (utilizando >, <>, =, < =,! =) e consultas de ORDER BY. Consultas de ORDER By por predefinição também necessitam de precisão de índice máximo (-1). O tipo de dados pode ser a cadeia ou um número.

BD do Azure do Cosmos também suporta o tipo de índice espacial para cada caminho que pode ser especificado para os tipos de dados ponto, polígono ou LineString. O valor no caminho especificado tem de ser um fragmento de GeoJSON válido como `{"type": "Point", "coordinates": [0.0, 10.0]}`.

* **Geográficos** suporta eficiente geográficos (dentro e distância) consultas. O tipo de dados pode ser ponto, Polygon ou LineString.

> [!NOTE]
> BD do Azure do Cosmos suporta indexação automática ponto, polígono e LineString com tipos de dados.
> 
> 

Eis os tipos de índice suportados e exemplos de consultas que podem ser utilizados para servir:

| Tipo de índice | Caso de utilização/descrição                                                                                                                                                                                                                                                                                                                                                                                                              |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Hash       | Hash através de/prop /? (ou /) pode ser utilizado para efetuar as seguintes consultas de forma eficiente:<br><br>SELECIONE da coleção c WHERE c.prop = "valor"<br><br>Hash através de propriedades / [] /? (ou / ou propriedades /) pode ser utilizado para efetuar as seguintes consultas de forma eficiente:<br><br>SELECIONE tag da coleção c associação tag IN c.props tag onde = 5                                                                                                                       |
| Intervalo      | Intervalo de ativação pós-falha/prop /? (ou /) pode ser utilizado para efetuar as seguintes consultas de forma eficiente:<br><br>SELECIONE da coleção c WHERE c.prop = "valor"<br><br>SELECIONE da coleção c WHERE c.prop > 5<br><br>SELECIONE a partir da coleção c ORDER BY c.prop                                                                                                                                                                                                              |
| Espacial     | Intervalo de ativação pós-falha/prop /? (ou /) pode ser utilizado para efetuar as seguintes consultas de forma eficiente:<br><br>SELECIONE da coleção c<br><br>ONDE ST_DISTANCE (c.prop, {"type": "Ponto", "coordenadas": [0.0, 10.0]}) < 40<br><br>SELECIONE de coleção c onde ST_WITHIN(c.prop, {"type": "Polygon",...}) – com a indexação pontos ativada<br><br>SELECIONE da coleção c onde ST_WITHIN({"type": "Point",...}, c.prop) – com a indexação polígonos ativados              |

Por predefinição, é devolvido um erro de consultas com operadores de intervalo como > = Se não houver nenhum índice de intervalo (de qualquer precisão) para assinalar que uma análise poderá ser necessária servir a consulta. Consultas de intervalo podem ser realizadas sem um índice de intervalo utilizando o **x-ms-documentdb-enable-análise** cabeçalho na REST API ou o **EnableScanInQuery** pedir opção utilizando o SDK .NET. Se existirem quaisquer outros filtros na consulta que BD do Cosmos Azure pode utilizar o índice para filtrar contra, não é devolvido nenhum erro.

As mesmas regras aplicam-se para consultas geográficos. Por predefinição, é devolvido um erro de consultas geográficos se existir nenhum índice geográfico, e existem não existem outros filtros que podem ser servidos do índice. Podem ser realizados como uma análise utilizando **x-ms-documentdb-enable-análise** ou **EnableScanInQuery**.

#### <a name="index-precision"></a>Índice de precisão
Pode utilizar a precisão do índice efetuar compromissos entre o armazenamento de índice gerais e de desempenho das consultas. Para números, recomendamos que utilize a configuração de precisão predefinida de -1 (máximo). Uma vez que os números são 8 bytes no JSON, isto é equivalente a uma configuração de 8 bytes. Escolher um valor inferior para precisão, tal como 1 e 7, significa que os valores dentro de alguns intervalos de mapeiam para o mesmo índice entrada. Por conseguinte, reduzir o espaço de armazenamento de índice, mas a execução da consulta poderá ter de processar mais documentos. Por conseguinte, o que consome mais débito em unidades de pedido.

Configuração de precisão de índice possui uma aplicação mais prática com intervalos de cadeia. Como as cadeias podem ser qualquer comprimento arbitrário, a escolha da precisão do índice poderá afetar o desempenho das consultas de intervalo de cadeia. Também poderá afetar a quantidade de espaço de armazenamento de índice que é necessário. Os índices de intervalo de cadeia podem ser configurados com 1 a 100 ou -1 (máximo). Se pretender efetuar consultas de ORDER BY em relação a propriedades de cadeia, tem de especificar uma precisão de -1 para os caminhos correspondentes.

Os índices espaciais utilizam sempre a precisão do índice predefinido para todos os tipos (ponto, LineString e polígono). A precisão do índice predefinido para os índices espaciais não pode ser substituída. 

O exemplo seguinte mostra como aumentar a precisão para índices de intervalo de uma coleção, utilizando o SDK .NET. 

**Criar uma coleção com uma precisão de índice personalizado**

    var rangeDefault = new DocumentCollection { Id = "rangeCollection" };

    // Override the default policy for strings to Range indexing and "max" (-1) precision
    rangeDefault.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), rangeDefault);   


> [!NOTE]
> BD do Azure do Cosmos devolve um erro quando uma consulta utiliza ORDER BY, mas não tem um índice de intervalo contra o caminho consultado com a precisão máxima. 
> 
> 

Da mesma forma, pode excluir completamente caminhos de indexação. O exemplo seguinte mostra como excluir uma secção completa dos documentos (um *subárvore*) de indexação utilizando o \* operador de caráter universal.

    var collection = new DocumentCollection { Id = "excludedPathCollection" };
    collection.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
    collection.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*" });

    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), excluded);



## <a name="opt-in-and-opt-out-of-indexing"></a>Optar ativamente por participar no e ativamente indexação
Pode escolher se pretende que a coleção para indexar automaticamente todos os documentos. Por predefinição, todos os documentos são automaticamente indexados, mas pode desativar a indexação automática. Quando a indexação está desativada, documentos podem ser acedidos através de apenas os respetivos auto-ligações ou por consultas utilizando o documento ID.

Com automática indexação desativada, pode adicionar ainda seletivamente apenas os documentos específicos para o índice. Por outro lado, pode deixar automática de indexação no e escolha seletivamente excluir a documentos específicos. A indexação/desative as configurações são úteis quando tem apenas um subconjunto de documentos que tem de ser consultado.

O exemplo seguinte mostra como incluir um documento explicitamente utilizando o [SQL API .NET SDK](https://docs.microsoft.com/azure/cosmos-db/sql-api-sdk-dotnet) e [RequestOptions.IndexingDirective](http://msdn.microsoft.com/library/microsoft.azure.documents.client.requestoptions.indexingdirective.aspx) propriedade.

    // If you want to override the default collection behavior to either
    // exclude (or include) a document in indexing,
    // use the RequestOptions.IndexingDirective property.
    client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        new { id = "AndersenFamily", isRegistered = true },
        new RequestOptions { IndexingDirective = IndexingDirective.Include });

## <a name="modify-the-indexing-policy-of-a-collection"></a>Modificar a política de indexação de uma coleção
Na base de dados do Azure Cosmos, pode efetuar alterações à política de indexação de uma coleção no momento. Uma alteração na política de uma coleção de base de dados do Azure Cosmos de indexação pode levar a uma alteração na forma do índice. A alteração afetará os caminhos que podem ser indexados, os respetivos precisão e o modelo de consistência do índice em si. Uma alteração na política de indexação eficazmente requer uma transformação do índice antigo para um novo índice. 

**Transformações de índice online**

![Como a indexação funciona – transformações de índice online de base de dados do Azure Cosmos](./media/indexing-policies/index-transformations.png)

Transformações de índice que são efetuadas online. Isto significa que os documentos indexados pela política antiga eficientemente são transformados pela nova política *sem afetar a disponibilidade de escrita ou o débito aprovisionado* da coleção. A consistência de leitura e escrita operações efetuadas utilizando a API REST, SDKs, ou a partir do acionadores e procedimentos armazenados não são afetados durante a transformação do índice. Não há nenhuma degradação do desempenho ou o período de indisponibilidade às suas aplicações quando efetuar uma política de indexação alterar.

No entanto, durante o tempo de transformação de índice progresso, as consultas são eventualmente consistentes, independentemente da configuração do modo de indexação (Consistent ou Lazy). Também se aplica a consultas de todas as interfaces: REST API, SDKs e a partir do e procedimentos armazenados acionadores. Tal como com Lazy indexação, a transformação do índice é executada no modo assíncrono em segundo plano nas réplicas com os recursos dos componentes de reserva que estão disponíveis para uma réplica específica. 

Transformações de índice também são efetuadas no local. BD do Azure do Cosmos não manter duas cópias do índice e troca terminar o índice antigo pelo novo. Isto significa que não existe espaço em disco adicional necessário ou consumido na suas coleções enquanto ocorrem de transformações de índice.

Quando alterar a política de indexação, as alterações são aplicadas para mover desde o índice antigo para o novo principalmente com base nas configurações de modo indexação. Configurações de modo indexação desempenham uma função de maior do que outros valores como caminhos incluído/excluído, os tipos de índice e precisions. 

Se os antigas e novas políticas ambos utilizam indexação consistente, base de dados do Azure Cosmos efetua uma transformação de índice online. Não é possível aplicar outro indexação alterações na política que tenha o modo de indexação consistente enquanto a transformação está em curso. No entanto, pode passar para Lazy ou nenhum modo de indexação a uma transformação está em curso: 

* Quando mover para Lazy, a alteração de política do índice tem efeita imediato. BD do Azure do Cosmos inicia a recriação do índice no modo assíncrono. 
* Quando move como None, o índice é removido imediatamente. Mover para nenhum é útil quando pretender cancelar uma transformação em curso e começar do início com uma outra política de indexação. 

O fragmento de código seguinte mostra como modificar política de indexação de uma coleção do modo de indexação consistente para o modo de indexação lento. Se estiver a utilizar o SDK .NET, pode iniciar uma alteração de política de indexação ao utilizar a nova **ReplaceDocumentCollectionAsync** método.

**Modificar a política de indexação de Consistent para Lazy**

    // Switch to Lazy indexing mode.
    Console.WriteLine("Changing from Default to Lazy IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.Lazy;

    await client.ReplaceDocumentCollectionAsync(collection);

**Controlar o progresso da transformação do índice**

Pode acompanhar o progresso de percentagem da transformação índice para um índice consistente utilizando o **IndexTransformationProgress** propriedade de resposta de um **ReadDocumentCollectionAsync** chamada. Outros SDKs e a API REST, suportam métodos e propriedades equivalentes para efetuar alterações de política de indexação. Pode verificar o progresso de uma transformação de índice para um índice consistente chamando **ReadDocumentCollectionAsync**: 

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
> * O **IndexTransformationProgress** propriedade é aplicável apenas quando transformar num índice consistente. Utilize o **ResourceResponse.LazyIndexingProgress** propriedade para transformações de controlo para um índice em diferido.
> * O **IndexTransformationProgress** e **LazyIndexingProgress** propriedades são povoadas apenas para uma coleção particionada não, ou seja, uma coleção que é criada sem uma chave de partição.
>

Pode remover o índice de uma coleção, movendo para nenhum modo de indexação. Pode ser uma ferramenta operacional útil se pretender cancelar uma transformação em curso e, em seguida, iniciar imediatamente um novo.

**Remova o índice de uma coleção**

    // Switch to Lazy indexing mode.
    Console.WriteLine("Dropping index by changing to to the None IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.None;

    await client.ReplaceDocumentCollectionAsync(collection);

Quando seria são efetuadas alterações de política de indexação a suas coleções de BD do Cosmos do Azure? Seguem-se os casos de utilização mais comuns:

* Servir resultados consistentes durante o funcionamento normal, mas reverta para o modo de indexação lento durante a importação de dados em massa.
* Começar a utilizar novas funcionalidades de indexação nas coleções de BD do Cosmos Azure atuais. Por exemplo, pode utilizar geoespacial consultar, que requer o tipo de índice geográfico ou ordenar BY / consultas de intervalo, o que exige a cadeia de tipo de índice de intervalo de cadeia.
* Mão-selecionar as propriedades ser indexados e alterá-los ao longo do tempo.
* Otimize a indexação precisão para melhorar o desempenho de consulta ou para reduzir o armazenamento consumido.

> [!NOTE]
> Para modificar a política de indexação utilizando **ReplaceDocumentCollectionAsync**, tem de utilizar a versão 1.3.0 ou uma versão posterior do SDK do .NET.
> 
> Para transformação do índice concluir com êxito, certifique-se de que existe espaço suficiente espaço de armazenamento livre disponível na coleção. Se a coleção atingir a quota de armazenamento, a transformação do índice é colocada em pausa. Transformação de índice retoma automaticamente quando o espaço de armazenamento está disponível, por exemplo, se não eliminar alguns documentos.
> 
> 

## <a name="performance-tuning"></a>Otimização do desempenho
As APIs do SQL Server fornecem informações sobre as métricas de desempenho, tais como o armazenamento de índice utilizado e o custo de débito (unidades de pedido) para cada operação. Pode utilizar estas informações para comparar várias políticas de indexação e para a otimização de desempenho.

Para verificar a quota de armazenamento e a utilização de uma coleção, execute um **HEAD** ou **obter** pedido contra o recurso da coleção. Em seguida, Inspecione o **x-ms-pedido-quota** e **x-ms-pedido-utilização** cabeçalhos. SDK .NET, o [DocumentSizeQuota](http://msdn.microsoft.com/library/dn850325.aspx) e [DocumentSizeUsage](http://msdn.microsoft.com/library/azure/dn850324.aspx) propriedades no [ResourceResponse < T\> ](http://msdn.microsoft.com/library/dn799209.aspx) conter estes valores correspondentes.

     // Measure the document size usage (which includes the index size) against   
     // different policies.
     ResourceResponse<DocumentCollection> collectionInfo = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));  
     Console.WriteLine("Document size quota: {0}, usage: {1}", collectionInfo.DocumentQuota, collectionInfo.DocumentUsage);


Para medir a sobrecarga de indexação de cada operação de escrita (criar, atualizar ou eliminar), Inspecione o **x-ms-pedido-encargos** cabeçalho (ou equivalente [RequestCharge](http://msdn.microsoft.com/library/dn799099.aspx) propriedade no [ ResourceResponse < T\> ](http://msdn.microsoft.com/library/dn799209.aspx) no SDK do .NET) para medir o número de unidades de pedido que são consumidos por estas operações.

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
Uma alteração no esquema de política de indexação foi introduzida 7 de Julho de 2015, com a API de REST versão 2015-06-03. As classes correspondentes nas versões do SDK tem novas implementações para fazer corresponder o esquema. 

As seguintes alterações foram implementadas na especificação de JSON:

* Política de indexação suporta índices de intervalo para cadeias.
* Cada caminho pode ter várias definições de índice. Pode ter um para cada tipo de dados.
* A indexação de precisão suporta entre 1 e 8 para números, 1 a 100 cadeias e -1 (precisão máxima).
* Os segmentos de caminho não necessitam de uma aspas para cada caminho de escape dupla. Por exemplo, pode adicionar um caminho para   **/título /?** em vez de **/ "title" /?**.
* O caminho de raiz que representa a "todos os caminhos" pode ser representado como  **/ \***  (além  **/** ).

Se tiver código que Aprovisiona coleções com uma política de indexação personalizada escrita com a versão 1.1.0 do .NET SDK ou uma versão anterior, mover para o SDK versão 1.2.0, tem de alterar o código da aplicação para processar estas alterações. Se não tiver o código que configura a política de indexação ou, se pretender continuar a utilizar uma versão anterior do SDK, não são necessárias alterações.

Para ver uma comparação prática, segue-se um exemplo de uma política de indexação personalizada escrito utilizando a API de REST versão 2015-06-03, seguido da mesma política de indexação escrita utilizando a API de REST versão 2015-04-08 anterior.

**Atual JSON (API REST versão 2015-06-03) de política de indexação**

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


**O JSON (API REST versão 2015-04-08) de política de indexação anteriormente**

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
Para exemplos de gestão de política de índice e para obter mais informações sobre o idioma de consulta de base de dados do Azure Cosmos, consulte as seguintes ligações:

* [Exemplos de código da gestão de índice de .NET de API do SQL Server](https://github.com/Azure/azure-documentdb-net/blob/master/samples/code-samples/IndexManagement/Program.cs)
* [Operações de coleção de REST de API do SQL Server](https://msdn.microsoft.com/library/azure/dn782195.aspx)
* [Consulta com o SQL Server](sql-api-sql-query.md)

