---
title: Ferramenta de migração de bases de dados do Azure Cosmos DB | Microsoft Docs
description: Saiba como utilizar as ferramentas open source de migração de bases de dados do Azure Cosmos DB para importar dados para o Azure Cosmos DB a partir de várias origens, incluindo ficheiros MongoDB, SQL Server, armazenamento de Tabelas, Amazon DynamoDB, CSV e JSON. Conversão de CSV para JSON.
keywords: csv para json, ferramentas de migração de bases de dados, converter de csv para json
services: cosmos-db
author: deborahc
manager: kfile
editor: monicar
ms.service: cosmos-db
ms.devlang: na
ms.topic: tutorial
ms.date: 03/30/2018
ms.author: dech
ms.custom: mvc
ms.openlocfilehash: af6faa6abcc54ef11e066d3a348dac28b23c7af4
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "49079094"
---
# <a name="use-data-migration-tool-to-migrate-your-data-to-azure-cosmos-db"></a>Utilize a ferramenta de migração de dados para migrar os dados para o Azure Cosmos DB 

Este tutorial fornece instruções sobre como utilizar a ferramenta de Migração de Dados do Azure Cosmos DB, que pode importar dados de várias origens para coleções e tabelas do Azure Cosmos DB. É possível importar a partir de ficheiros JSON, CSV, SQL, MongoDB, armazenamento de Tabelas do Azure, Amazon DynamoDB e até mesmo coleções de APIs de SQL do Azure Cosmos DB, e pode migrar esses dados para coleções e tabelas, de modo a utilizá-los com o Azure Cosmos DB. A ferramenta de Migração de Dados também pode ser utilizada ao migrar de uma coleção de uma partição individual para uma coleção de várias partições para a API de SQL.

Que API vai ser utilizada com o Azure Cosmos DB? 

* **[API de SQL](documentdb-introduction.md)** - pode utilizar qualquer uma das opções de origem fornecidas na ferramenta de Migração de Dados para importar dados.
* **[API de Tabela](table-introduction.md)** - pode utilizar a ferramenta de Migração de Dados ou AzCopy para importar dados. Veja [Importar dados para utilização com a API de Tabela do Azure Cosmos DB](table-import.md) para obter mais informações.
* **[API do MongoDB](mongodb-introduction.md)** - a ferramenta de Migração de Dados não suporta atualmente a API do MongoDB do Azure Cosmos DB como origem nem como destino. Se pretender migrar os dados dentro ou fora de coleções de APIs do MongoDB do Azure Cosmos DB, veja [Azure Cosmos DB: como migrar dados para a API do MongoDB](mongodb-migrate.md) para obter instruções. Pode também utilizar a ferramenta de Migração de Dados para exportar dados do MongoDB para coleções de APIs de SQL do Azure Cosmos DB, para utilização com a API de SQL. 
* **[API do Gremlin](graph-introduction.md)** – a ferramenta de Migração de Dados não é uma ferramenta de importação suportada por contas da API do Gremlin neste momento. 

Este tutorial abrange as seguintes tarefas:

> [!div class="checklist"]
> * Instalar a ferramenta de Migração de Dados
> * Importar dados de origens de dados diferentes
> * Exportar do Azure Cosmos DB para JSON

## <a id="Prerequisites"></a>Pré-requisitos
Antes de seguir as instruções deste artigo, certifique-se de que os seguintes elementos estão instalados:

* [Microsoft .NET Framework 4.51](https://www.microsoft.com/download/developer-tools.aspx) ou superior.

* **Aumentar débito:** a duração da migração de dados depende da quantidade de débito que configurou para uma coleção individual ou um conjunto de coleções. Aumente o débito para migrações de dados maiores. Após concluir a migração, reduza o débito para reduzir os custos. Para obter mais informações sobre como aumentar o débito no portal do Azure, veja os níveis de Desempenho e escalões de preço do Azure Cosmos DB.

* **Criar recursos do Azure Cosmos DB:** antes de começar a migração de dados, crie previamente todas as suas coleções no portal do Azure. Se estiver a migrar para uma conta do Azure Cosmos DB com débito ao nível da base de dados, confirme que proporciona uma chave de partição quando criar as coleções do Azure Cosmos DB.

## <a id="Overviewl"></a>Descrição Geral
A ferramenta de Migração de Dados é uma solução open source que importa dados para o Azure Cosmos DB a partir de uma variedade de origens, incluindo:

* Ficheiros JSON
* MongoDB
* SQL Server
* Ficheiros CSV
* Armazenamento de Tabelas do Azure
* Amazon DynamoDB
* HBase
* Coleções do Azure Cosmos DB

Embora a ferramenta de importação inclua uma interface gráfica (dtui.exe), também pode ser controlada a partir da linha de comandos (dt.exe). De facto, está disponível uma opção para apresentar o comando associado depois de configurar uma importação através da IU. Os dados de origem tabulares (por ex., ficheiros do SQL Server ou CSV) podem ser transformados de modo a que as relações hierárquicas (subdocumentos) possam ser criados durante a importação. Continue a ler para saber mais sobre as opções de origem, os comandos de exemplo para importar a partir de cada origem, as opções de destino e como visualizar os resultados da importação.

## <a id="Install"></a>Instalação
O código de origem da ferramenta de migração está disponível no GitHub [neste repositório](https://github.com/azure/azure-documentdb-datamigrationtool). Pode transferir e compilar a solução localmente ou [transferir um binário previamente compilado](https://cosmosdbportalstorage.blob.core.windows.net/datamigrationtool/2018.02.28-1.8.1/dt-1.8.1.zip) e, em seguida, executar:

* **Dtui.exe**: versão de interface gráfica da ferramenta
* **Dt.exe**: versão de linha de comandos da ferramenta

## <a name="select-data-source"></a>Selecionar origem de dados

Assim que instalar a ferramenta, está na altura de importar os seus dados. Que tipo de dados pretende importar?

* [Ficheiros JSON](#JSON)
* [MongoDB](#MongoDB)
* [Ficheiros de Exportação do MongoDB](#MongoDBExport)
* [SQL Server](#SQL)
* [Ficheiros CSV](#CSV)
* [Armazenamento de tabelas do Azure](#AzureTableSource)
* [Amazon DynamoDB](#DynamoDBSource)
* [Blob](#BlobImport)
* [Coleções do Azure Cosmos DB](#SQLSource)
* [HBase](#HBaseSource)
* [Importação em volume do Azure Cosmos DB](#SQLBulkTarget)
* [Importação de registos sequenciais do Azure Cosmos DB](#SQLSeqTarget)


## <a id="JSON"></a>Importar ficheiros JSON
A opção de importador de origem de ficheiros JSON permite importar um ou mais ficheiros de documento JSON único ou ficheiros JSON que contêm, cada um, vários documentos JSON. Ao adicionar pastas que contêm ficheiros JSON para importação, tem a opção de procurar recursivamente ficheiros em subpastas.

![Captura de ecrã das opções de origem de ficheiro JSON - ferramentas de migração de bases de dados](./media/import-data/jsonsource.png)

Seguem-se alguns exemplos de linha de comandos para importar ficheiros JSON:

    #Import a single JSON file
    dt.exe /s:JsonFile /s.Files:.\Sessions.json /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Sessions /t.CollectionThroughput:2500

    #Import a directory of JSON files
    dt.exe /s:JsonFile /s.Files:C:\TESessions\*.json /t:DocumentDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Sessions /t.CollectionThroughput:2500

    #Import a directory (including sub-directories) of JSON files
    dt.exe /s:JsonFile /s.Files:C:\LastFMMusic\**\*.json /t:DocumentDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Music /t.CollectionThroughput:2500

    #Import a directory (single), directory (recursive), and individual JSON files
    dt.exe /s:JsonFile /s.Files:C:\Tweets\*.*;C:\LargeDocs\**\*.*;C:\TESessions\Session48172.json;C:\TESessions\Session48173.json;C:\TESessions\Session48174.json;C:\TESessions\Session48175.json;C:\TESessions\Session48177.json /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:subs /t.CollectionThroughput:2500

    #Import a single JSON file and partition the data across 4 collections
    dt.exe /s:JsonFile /s.Files:D:\\CompanyData\\Companies.json /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:comp[1-4] /t.PartitionKey:name /t.CollectionThroughput:2500

## <a id="MongoDB"></a>Importar do MongoDB

> [!IMPORTANT]
> Se estiver a importar para uma conta do Azure Cosmos DB com Suporte para o MongoDB, siga estas [instruções](mongodb-migrate.md).
> 
> 

A opção de importador de origem do MongoDB permite importar de uma coleção individual do MongoDB e, opcionalmente, filtrar documentos através de uma consulta e/ou modificar a estrutura de documento com uma projeção.  

![Captura de ecrã das opções de origem do MongoDB](./media/import-data/mongodbsource.png)

A cadeia de ligação está no formato MongoDB padrão:

    mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database>

> [!NOTE]
> Utilize o comando Verificar para garantir que a instância do MongoDB especificada no campo de cadeia de ligação pode ser acedida.
> 
> 

Introduza o nome da coleção da qual os dados serão importados. Opcionalmente, pode especificar ou fornecer um ficheiro para uma consulta (por exemplo, {pop: {$gt:5000}} ) e/ou projeção (por exemplo, {loc:0} ) para filtrar e moldar os dados a serem importados.

Seguem-se alguns exemplos de linha de comandos para importar do MongoDB:

    #Import all documents from a MongoDB collection
    dt.exe /s:MongoDB /s.ConnectionString:mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database> /s.Collection:zips /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:BulkZips /t.IdField:_id /t.CollectionThroughput:2500

    #Import documents from a MongoDB collection which match the query and exclude the loc field
    dt.exe /s:MongoDB /s.ConnectionString:mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database> /s.Collection:zips /s.Query:{pop:{$gt:50000}} /s.Projection:{loc:0} /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:BulkZipsTransform /t.IdField:_id/t.CollectionThroughput:2500

## <a id="MongoDBExport"></a>Importar ficheiros de exportação do MongoDB

> [!IMPORTANT]
> Se estiver a importar para uma conta do Azure Cosmos DB com suporte para o MongoDB, siga estas [instruções](mongodb-migrate.md).
> 
> 

A opção de importador de origem de ficheiros JSON de exportação do MongoDB permite importar um ou mais ficheiros JSON produzidos a partir do utilitário mongoexport.  

![Captura de ecrã das opções de origem de exportação do MongoDB](./media/import-data/mongodbexportsource.png)

Ao adicionar pastas que contêm ficheiros JSON de exportação do MongoDB para importar, tem a opção de procurar recursivamente ficheiros em subpastas.

Eis um exemplo de linha de comandos para importação de ficheiros JSON de exportação do MongoDB:

    dt.exe /s:MongoDBExport /s.Files:D:\mongoemployees.json /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:employees /t.IdField:_id /t.Dates:Epoch /t.CollectionThroughput:2500

## <a id="SQL"></a>Importar do SQL Server
A opção de importador de origem de SQL permite importar a partir de uma base de dados individual do SQL Server e, opcionalmente, filtrar os registos para importar através de uma consulta. Além disso, pode modificar a estrutura de documento, especificando um separador de aninhamento (mais informações em breve).  

![Captura de ecrã das opções de origem de SQL - ferramentas de migração de bases de dados](./media/import-data/sqlexportsource.png)

O formato da cadeia de ligação é o formato de cadeia de ligação SQL padrão.

> [!NOTE]
> Utilize o comando Verificar para garantir que a instância do SQL Server especificada no campo de cadeia de ligação pode ser acedida.
> 
> 

A propriedade do separador de aninhamento é utilizada para criar relações hierárquicas (subdocumentos) durante a importação. Considere a seguinte consulta SQL:

*selecione CAST(BusinessEntityID AS varchar) como Id, Name, AddressType como [Address.AddressType], AddressLine1 como [Address.AddressLine1], City como [Address.Location.City], StateProvinceName como [Address.Location.StateProvinceName], PostalCode como [Address.PostalCode], CountryRegionName como [Address.CountryRegionName] de Sales.vStoreWithAddresses WHERE AddressType='Main Office'*

São devolvidos os seguintes resultados (parciais):

![Captura de ecrã dos resultados da consulta SQL](./media/import-data/sqlqueryresults.png)

Tenha em atenção os aliases como Address.AddressType e Address.Location.StateProvinceName. Ao especificar um separador de aninhamento de “.”, a ferramenta de importação cria os subdocumentos Address e Address.Location durante a importação. Eis um exemplo de um documento resultante do Azure Cosmos DB:

*{ "id": "956", "Name": "Finer Sales and Service", "Address": { "AddressType": "Main Office", "AddressLine1": "#500-75 O'Connor Street", "Location": { "City": "Ottawa", "StateProvinceName": "Ontario" }, "PostalCode": "K4B 1S2", "CountryRegionName": "Canada" } }*

Seguem-se alguns exemplos de linha de comandos para importar do SQL Server:

    #Import records from SQL which match a query
    dt.exe /s:SQL /s.ConnectionString:"Data Source=<server>;Initial Catalog=AdventureWorks;User Id=advworks;Password=<password>;" /s.Query:"select CAST(BusinessEntityID AS varchar) as Id, * from Sales.vStoreWithAddresses WHERE AddressType='Main Office'" /t:DocumentDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Stores /t.IdField:Id /t.CollectionThroughput:2500

    #Import records from sql which match a query and create hierarchical relationships
    dt.exe /s:SQL /s.ConnectionString:"Data Source=<server>;Initial Catalog=AdventureWorks;User Id=advworks;Password=<password>;" /s.Query:"select CAST(BusinessEntityID AS varchar) as Id, Name, AddressType as [Address.AddressType], AddressLine1 as [Address.AddressLine1], City as [Address.Location.City], StateProvinceName as [Address.Location.StateProvinceName], PostalCode as [Address.PostalCode], CountryRegionName as [Address.CountryRegionName] from Sales.vStoreWithAddresses WHERE AddressType='Main Office'" /s.NestingSeparator:. /t:DocumentDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:StoresSub /t.IdField:Id /t.CollectionThroughput:2500

## <a id="CSV"></a>Importar ficheiros CSV e converter CSV para JSON
A opção de importador de origem de ficheiro CSV permite importar um ou mais ficheiros CSV. Ao adicionar pastas que contêm ficheiros CSV para importar, tem a opção de procurar recursivamente ficheiros em subpastas.

![Captura de ecrã das opções de origem de CSV - CSV para JSON](media/import-data/csvsource.png)

Semelhante à origem de SQL, a propriedade do separador de aninhamento pode ser utilizada para criar relações hierárquicas (subdocumentos) durante a importação. Considere a seguinte linha de cabeçalho e linhas de dados em CSV:

![Captura de ecrã dos registos de exemplo de CSV - CSV para JSON](./media/import-data/csvsample.png)

Tenha em atenção os aliases como DomainInfo.Domain_Name e RedirectInfo.Redirecting. Ao especificar um separador de aninhamento de “.”, a ferramenta de importação irá criar os subdocumentos DomainInfo e RedirectInfo durante a importação. Eis um exemplo de um documento resultante do Azure Cosmos DB:

*{ "DomainInfo": { "Domain_Name": "ACUS.GOV", "Domain_Name_Address": "http://www.ACUS.GOV" }, "Federal Agency": "Administrative Conference of the United States", "RedirectInfo": { "Redirecting": "0", "Redirect_Destination": "" }, "id": "9cc565c5-ebcd-1c03-ebd3-cc3e2ecd814d" }*

A ferramenta de importação tenta inferir informações de tipo para os valores sem aspas nos ficheiros CSV (os valores entre aspas são sempre tratados como cadeias).  Os tipos são identificados pela seguinte ordem: número, data e hora, booleano.  

Existem dois outros aspetos a ter em atenção na importação de CSV:

1. Por predefinição, os valores sem aspas sempre são cortados relativamente a separadores e espaços, enquanto os valores entre aspas são preservados tal como estão. Este comportamento pode ser substituído com a caixa de verificação Cortar valores entre aspas ou a /s.TrimQuoted opção da linha de comandos.
2. Por predefinição, um nulo sem aspas é tratado como um valor nulo. Este comportamento pode ser substituído (ou seja, tratar um nulo sem aspas como uma cadeia “nula”), com a caixa de verificação Tratar NULO sem aspas como cadeia ou a opção /s.NoUnquotedNulls da linha de comandos.

Eis um exemplo de linha de comandos para a importação de CSV:

    dt.exe /s:CsvFile /s.Files:.\Employees.csv /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Employees /t.IdField:EntityID /t.CollectionThroughput:2500

## <a id="AzureTableSource"></a>Importar do Armazenamento de Tabelas do Azure
A opção de importador de origem de armazenamento de Tabelas do Azure permite importar a partir de uma tabela individual do armazenamento de Tabelas do Azure. Opcionalmente, pode filtrar as entidades da tabela a importar. 

Os dados importados do Armazenamento de Tabelas do Azure podem ser enviados para tabelas e entidades do Azure Cosmos DB, para utilização com a API de Tabela, ou para coleções e documentos, para utilização com a API de SQL. No entanto, a API de Tabela só está disponível como um destino no utilitário da linha de comandos; não é possível exportar a API de Tabela com a interface de utilizador da ferramenta de Migração de Dados. Para obter mais informações, veja [Importar dados para utilização com a API de Tabela do Azure Cosmos DB](table-import.md). 

![Captura de ecrã das opções de origem do armazenamento de Tabelas do Azure](./media/import-data/azuretablesource.png)

O formato da cadeia de ligação do armazenamento de Tabelas do Azure é:

    DefaultEndpointsProtocol=<protocol>;AccountName=<Account Name>;AccountKey=<Account Key>;

> [!NOTE]
> Utilize o comando Verificar para garantir que a instância de armazenamento de Tabelas do Azure especificada no campo de cadeia de ligação pode ser acedida.
> 
> 

Introduza o nome da tabela do Azure da qual importar. Opcionalmente, pode especificar um [filtro](../vs-azure-tools-table-designer-construct-filter-strings.md).

A opção de importador de origem do armazenamento de Tabelas do Azure inclui as seguintes opções adicionais:

1. Incluir Campos Internos
   1. Todos - incluir todos os campos internos (PartitionKey, RowKey e Carimbo de data/hora)
   2. Nenhum - excluir todos os campos internos
   3. RowKey - incluir apenas o campo RowKey
2. Selecionar Colunas
   1. Os filtros do armazenamento de Tabelas do Azure não suportam projeções. Se pretender importar apenas propriedades específicas de entidade de Tabelas do Azure, adicione-as à lista Selecionar Colunas. Todas as outras propriedades de entidade são ignoradas.

Eis um exemplo de linha de comandos para importação do armazenamento de Tabelas do Azure:

    dt.exe /s:AzureTable /s.ConnectionString:"DefaultEndpointsProtocol=https;AccountName=<Account Name>;AccountKey=<Account Key>" /s.Table:metrics /s.InternalFields:All /s.Filter:"PartitionKey eq 'Partition1' and RowKey gt '00001'" /s.Projection:ObjectCount;ObjectSize  /t:DocumentDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:metrics /t.CollectionThroughput:2500

## <a id="DynamoDBSource"></a>Importar do Amazon DynamoDB
A opção de importador de origem do Amazon DynamoDB permite importar a partir de uma tabela individual do Amazon DynamoDB e, opcionalmente, filtrar as entidades a importar. São fornecidos vários modelos para que configurar uma importação seja tão simples quanto possível.

![Captura de ecrã das opções de origem do DynamoDB - ferramentas de migração de bases de dados](./media/import-data/dynamodbsource1.png)

![Captura de ecrã das opções de origem do DynamoDB - ferramentas de migração de bases de dados](./media/import-data/dynamodbsource2.png)

O formato da cadeia de ligação do Amazon DynamoDB é:

    ServiceURL=<Service Address>;AccessKey=<Access Key>;SecretKey=<Secret Key>;

> [!NOTE]
> Utilize o comando Verificar para garantir que a instância do Amazon DynamoDB especificada no campo de cadeia de ligação pode ser acedida.
> 
> 

Eis um exemplo de linha de comandos para importação do Amazon DynamoDB:

    dt.exe /s:DynamoDB /s.ConnectionString:ServiceURL=https://dynamodb.us-east-1.amazonaws.com;AccessKey=<accessKey>;SecretKey=<secretKey> /s.Request:"{   """TableName""": """ProductCatalog""" }" /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<Azure Cosmos DB Endpoint>;AccountKey=<Azure Cosmos DB Key>;Database=<Azure Cosmos DB Database>;" /t.Collection:catalogCollection /t.CollectionThroughput:2500

## <a id="BlobImport"></a>Importar do armazenamento de Blobs do Azure
O ficheiro JSON, o ficheiro de exportação do MongoDB e as opções do importador de origem de ficheiros CSV permitem importar um ou mais ficheiros do armazenamento de Blobs do Azure. Depois de especificar um URL do contentor de Blob e a Chave da Conta, forneça uma expressão regular para selecionar os ficheiros a importar.

![Captura de ecrã das opções de origem de ficheiros Blob](./media/import-data/blobsource.png)

Eis um exemplo da linha de comandos para importar ficheiros JSON do armazenamento de Blobs do Azure:

    dt.exe /s:JsonFile /s.Files:"blobs://<account key>@account.blob.core.windows.net:443/importcontainer/.*" /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:doctest

## <a id="SQLSource"></a>Importar de uma coleção de APIs de SQL
A opção de importador de origem do Azure Cosmos DB permite importar dados de uma ou mais coleções do Azure Cosmos DB e, opcionalmente, filtrar os documentos com uma consulta.  

![Captura de ecrã das opções de origem do Azure Cosmos DB](./media/import-data/documentdbsource.png)

O formato da cadeia de ligação do Azure Cosmos DB é:

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

A cadeia de ligação da conta do Azure Cosmos DB pode ser obtida a partir da página Chaves do portal do Azure, conforme descrito em [Como gerir uma conta do Azure Cosmos DB](manage-account.md); no entanto, o nome da base de dados tem de ser anexado à cadeia de ligação no seguinte formato:

    Database=<CosmosDB Database>;

> [!NOTE]
> Utilize o comando Verificar para garantir que a instância do Azure Cosmos DB especificada no campo de cadeia de ligação pode ser acedida.
> 
> 

Para importar a partir de uma única coleção do Azure Cosmos DB, introduza o nome da coleção da qual importar os dados. Para importar de várias coleções do Azure Cosmos DB, forneça uma expressão regular para corresponder a um ou mais nomes de coleção (por exemplo, collection01 | collection02 | collection03). Opcionalmente, pode especificar ou fornecer um ficheiro para uma consulta, para filtrar e moldar os dados a serem importados.

> [!NOTE]
> Uma vez que o campo de coleção aceita expressões regulares, se estiver a importar de uma única coleção cujo nome contém carateres de expressão regular, esses carateres têm de ser de escape, em conformidade.
> 
> 

A opção de importador de origem do Azure Cosmos DB inclui as seguintes opções avançadas:

1. Incluir Campos Internos: especifica se deve ou não incluir as propriedades do sistema de documentos do Azure Cosmos DB na exportação (por exemplo, _rid, _ts).
2. Número de Tentativas em Caso de Falha: especifica o número de vezes a tentar a ligação ao Azure Cosmos DB em caso de falhas transitórias (por exemplo, interrupção de conectividade de rede).
3. Intervalo de Repetições: especifica o período de tempo de espera entre tentar estabelecer novamente a ligação ao Azure Cosmos DB em caso de falhas transitórias (por exemplo, interrupção de conectividade de rede).
4. Modo de Ligação: especifica o modo de ligação a utilizar com o Azure Cosmos DB. As opções disponíveis são DirectTcp, DirectHttps e Gateway. Os modos de ligação direta são mais rápidos, enquanto o modo de gateway é mais amigável com a firewall, pois só utiliza a porta 443.

![Captura de ecrã das opções avançadas de origem do Azure Cosmos DB](./media/import-data/documentdbsourceoptions.png)

> [!TIP]
> A predefinição da ferramenta de importação é o modo de ligação de DirectTcp. Se ocorrerem problemas de firewall, mude para o modo de ligação de Gateway, pois só requer a porta 443.
> 
> 

Seguem-se alguns exemplos de linha de comandos para importação do Azure Cosmos DB:

    #Migrate data from one Azure Cosmos DB collection to another Azure Cosmos DB collections
    dt.exe /s:DocumentDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:TEColl /t:DocumentDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:TESessions /t.CollectionThroughput:2500

    #Migrate data from multiple Azure Cosmos DB collections to a single Azure Cosmos DB collection
    dt.exe /s:DocumentDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:comp1|comp2|comp3|comp4 /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:singleCollection /t.CollectionThroughput:2500

    #Export an Azure Cosmos DB collection to a JSON file
    dt.exe /s:DocumentDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:StoresSub /t:JsonFile /t.File:StoresExport.json /t.Overwrite /t.CollectionThroughput:2500

> [!TIP]
> A Ferramenta de Importação de Dados do Azure Cosmos DB também suporta a importação de dados do [Emulador do Azure Cosmos DB](local-emulator.md). Ao importar dados de um emulador local, defina o ponto final para `https://localhost:<port>`. 
> 
> 

## <a id="HBaseSource"></a>Importar do HBase
A opção de importador de origem do HBase permite importar dados de uma tabela do HBase e, opcionalmente, filtrar os dados. São fornecidos vários modelos para que configurar uma importação seja tão simples quanto possível.

![Captura de ecrã das opções de origem do HBase](./media/import-data/hbasesource1.png)

![Captura de ecrã das opções de origem do HBase](./media/import-data/hbasesource2.png)

O formato da cadeia de ligação do HBase Stargate é:

    ServiceURL=<server-address>;Username=<username>;Password=<password>

> [!NOTE]
> Utilize o comando Verificar para garantir que a instância do HBase especificada no campo de cadeia de ligação pode ser acedida.
> 
> 

Eis um exemplo de linha de comandos para importação do HBase:

    dt.exe /s:HBase /s.ConnectionString:ServiceURL=<server-address>;Username=<username>;Password=<password> /s.Table:Contacts /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:hbaseimport

## <a id="SQLBulkTarget"></a>Importar para a API de SQL (Importação em Volume)
O importador em Volume do Azure Cosmos DB permite importar de qualquer uma das opções de origem disponíveis, utilizando um procedimento armazenado do Azure Cosmos DB para eficiência. A ferramenta suporta a importação para uma coleção com uma única partição do Azure Cosmos DB, bem como a importação fragmentada, na qual os dados são particionados em várias coleções de partições únicas do Azure Cosmos DB. Para obter mais informações sobre a criação de partições de dados, veja [Partitioning and scaling in Azure Cosmos DB](partition-data.md) (Criar partições e dimensionar no Azure Cosmos DB). A ferramenta cria, executa e, em seguida, elimina o procedimento armazenado das coleções de destino.  

![Captura de ecrã das opções de volume do Azure Cosmos DB](./media/import-data/documentdbbulk.png)

O formato da cadeia de ligação do Azure Cosmos DB é:

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

A cadeia de ligação da conta do Azure Cosmos DB pode ser obtida a partir da página Chaves do portal do Azure, conforme descrito em [Como gerir uma conta do Azure Cosmos DB](manage-account.md); no entanto, o nome da base de dados tem de ser anexado à cadeia de ligação no seguinte formato:

    Database=<CosmosDB Database>;

> [!NOTE]
> Utilize o comando Verificar para garantir que a instância do Azure Cosmos DB especificada no campo de cadeia de ligação pode ser acedida.
> 
> 

Para importar para uma única coleção, introduza o nome da coleção da qual importar os dados e clique no botão Adicionar. Para importar para várias coleções, introduza o nome de cada coleção individualmente ou utilize a seguinte sintaxe para especificar várias coleções: *collection_prefix*[start index - end index]. Ao especificar várias coleções através da sintaxe acima referida, mantenha as seguintes diretrizes em mente:

1. São suportados apenas padrões de nome de intervalo de números inteiros. Por exemplo, especificar a coleção [0-3] cria as seguintes coleções: collection0, collection1, collection2, collection3.
2. Pode utilizar uma sintaxe abreviada: collection[3] cria o mesmo conjunto de coleções mencionado no passo 1.
3. Pode ser fornecida mais do que uma substituição. Por exemplo, collection[0-1] [0-9] gera 20 nomes de coleção com zeros à esquerda (collection01, ..02, ..03).

Depois de os nomes das coleções terem sido especificados, escolha o débito pretendido das coleções (de 400 RUs a 10 000 RUs). Para um melhor desempenho de importação, escolha um maior débito. Para obter mais informações sobre os níveis de desempenho, veja [Performance levels in Azure Cosmos DB](performance-levels.md) (Níveis de desempenho no Azure Cosmos DB).

> [!NOTE]
> A definição de débito de desempenho aplica-se apenas à criação de coleções. Se a coleção especificada já existir, não é possível modificar o respetivo débito.
> 
> 

Ao importar para várias coleções, a ferramenta de importação suporta a fragmentação com base em hash. Neste cenário, especifique a propriedade de documento que pretende utilizar como a Chave de Partição (se a Chave de Partição for deixada em branco, os documentos são fragmentados aleatoriamente entre as coleções de destino).

Opcionalmente, pode especificar o campo na origem de importação que deve ser utilizado como a propriedade de id de documento do Azure Cosmos DB durante a importação (se os documentos não contiverem esta propriedade, a ferramenta de importação gera um GUID como o valor da propriedade de id).

Estão disponíveis várias opções avançadas durante a importação. Em primeiro lugar, embora a ferramenta inclua um procedimento armazenado predefinido de importação em volume (BulkInsert.js), pode optar por especificar o seu próprio procedimento armazenado de importação:

 ![Captura de ecrã da opção de sproc de inserção em volume do Azure Cosmos DB](./media/import-data/bulkinsertsp.png)

Além disso, ao importar os tipos de data (por exemplo, do SQL Server ou MongoDB), pode escolher entre três opções de importação:

 ![Captura de ecrã das opções de importação de data e hora do Azure Cosmos DB](./media/import-data/datetimeoptions.png)

* Cadeia: manter como um valor de cadeia
* Época: manter como um valor de número de Época
* Ambos: manter os valores de cadeia e de número de Época. Esta opção cria um subdocumento, por exemplo: "date_joined": { "Value": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245 }

O importador em Volume do Azure Cosmos DB inclui as seguintes opções avançadas adicionais:

1. Tamanho do Lote: a predefinição da ferramenta é um tamanho de lote de 50.  Se os documentos a importar forem grandes, considere reduzir o tamanho do lote. Por outro lado, se os documentos a importar forem pequenos, considere aumentar o tamanho do lote.
2. Tamanho Máximo de Script (bytes): a predefinição da ferramenta é um tamanho máximo de script de 512 KB.
3. Desativar a Geração Automática de Id: se todos os documentos a importar contiverem um campo de id, selecionar esta opção pode aumentar o desempenho. Os documentos em que falta um campo de id exclusivo não são importados.
4. Atualizar Documentos Existentes: a predefinição da ferramenta é não substituir os documentos existentes com conflitos de id. A seleção desta opção permite substituir os documentos existentes com ids correspondentes. Esta funcionalidade é útil para as migrações de dados agendadas que atualizam os documentos existentes.
5. Número de Tentativas em Caso de Falha: especifica o número de vezes a tentar a ligação ao Azure Cosmos DB em caso de falhas transitórias (por exemplo, interrupção de conectividade de rede).
6. Intervalo de Repetições: especifica o período de tempo de espera entre tentar estabelecer novamente a ligação ao Azure Cosmos DB em caso de falhas transitórias (por exemplo, interrupção de conectividade de rede).
7. Modo de Ligação: especifica o modo de ligação a utilizar com o Azure Cosmos DB. As opções disponíveis são DirectTcp, DirectHttps e Gateway. Os modos de ligação direta são mais rápidos, enquanto o modo de gateway é mais amigável com a firewall, pois só utiliza a porta 443.

![Captura de ecrã das opções avançadas de importação em volume do Azure Cosmos DB](./media/import-data/docdbbulkoptions.png)

> [!TIP]
> A predefinição da ferramenta de importação é o modo de ligação de DirectTcp. Se ocorrerem problemas de firewall, mude para o modo de ligação de Gateway, pois só requer a porta 443.
> 
> 

## <a id="SQLSeqTarget"></a>Importar para a API de SQL (Importação de Registos Sequenciais)
O importador de registos sequenciais do Azure Cosmos DB permite importar de qualquer uma das opções de origem disponíveis numa base de registo a registo. Poderá escolher esta opção se estiver a importar para uma coleção existente que atingiu a quota de procedimentos armazenados. A ferramenta suporta a importação para uma coleção única (com uma partição única e várias partições) do Azure Cosmos DB, bem como a importação fragmentada, na qual os dados são particionados em várias coleções com uma partição única e/ou várias partições do Azure Cosmos DB. Para obter mais informações sobre a criação de partições de dados, veja [Partitioning and scaling in Azure Cosmos DB](partition-data.md) (Criar partições e dimensionar no Azure Cosmos DB).

![Captura de ecrã das opções de importação de registos sequenciais do Azure Cosmos DB](./media/import-data/documentdbsequential.png)

O formato da cadeia de ligação do Azure Cosmos DB é:

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

A cadeia de ligação da conta do Azure Cosmos DB pode ser obtida a partir da página Chaves do portal do Azure, conforme descrito em [Como gerir uma conta do Azure Cosmos DB](manage-account.md); no entanto, o nome da base de dados tem de ser anexado à cadeia de ligação no seguinte formato:

    Database=<Azure Cosmos DB Database>;

> [!NOTE]
> Utilize o comando Verificar para garantir que a instância do Azure Cosmos DB especificada no campo de cadeia de ligação pode ser acedida.
> 
> 

Para importar para uma única coleção, introduza o nome da coleção para a qual os dados serão importados e clique no botão Adicionar. Para importar para várias coleções, introduza o nome de cada coleção individualmente ou utilize a seguinte sintaxe para especificar várias coleções: *collection_prefix*[start index - end index]. Ao especificar várias coleções através da sintaxe acima referida, mantenha as seguintes diretrizes em mente:

1. São suportados apenas padrões de nome de intervalo de números inteiros. Por exemplo, especificar a coleção [0-3] cria as seguintes coleções: collection0, collection1, collection2, collection3.
2. Pode utilizar uma sintaxe abreviada: collection[3] cria o mesmo conjunto de coleções mencionado no passo 1.
3. Pode ser fornecida mais do que uma substituição. Por exemplo, a coleção [0-1], [0-9] cria 20 nomes de coleção com zeros à esquerda (collection01, ..02, ..03).

Depois de os nomes das coleções terem sido especificados, escolha o débito pretendido das coleções (de 400 RUs a 250 000 RUs). Para um melhor desempenho de importação, escolha um maior débito. Para obter mais informações sobre os níveis de desempenho, veja [Performance levels in Azure Cosmos DB](performance-levels.md) (Níveis de desempenho no Azure Cosmos DB). Qualquer importação para coleções com um débito >10 000 RUs requer uma chave de partição. Se optar por ter mais de 250 000 RUs, terá de fazer um pedido no portal para que a sua conta seja aumentada.

> [!NOTE]
> A definição de débito aplica-se apenas à coleção ou criação de base de dados. Se a coleção especificada já existir, o respetivo débito não será modificado.
> 
> 

Ao importar para várias coleções, a ferramenta de importação suporta a fragmentação com base em hash. Neste cenário, especifique a propriedade de documento que pretende utilizar como a Chave de Partição (se a Chave de Partição for deixada em branco, os documentos são fragmentados aleatoriamente entre as coleções de destino).

Opcionalmente, pode especificar o campo na origem de importação que deve ser utilizado como a propriedade de id de documento do Azure Cosmos DB durante a importação (se os documentos não contiverem esta propriedade, a ferramenta de importação gera um GUID como o valor da propriedade de id).

Estão disponíveis várias opções avançadas durante a importação. Em primeiro lugar, ao importar os tipos de data (por exemplo, do SQL Server ou MongoDB), pode escolher entre três opções de importação:

 ![Captura de ecrã das opções de importação de data e hora do Azure Cosmos DB](./media/import-data/datetimeoptions.png)

* Cadeia: manter como um valor de cadeia
* Época: manter como um valor de número de Época
* Ambos: manter os valores de cadeia e de número de Época. Esta opção cria um subdocumento, por exemplo: "date_joined": { "Value": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245 }

O importador de registos sequenciais do Azure Cosmos DB inclui as seguintes opções avançadas adicionais:

1. Número de Pedidos Paralelos: a predefinição da ferramenta é dois pedidos paralelos. Se os documentos a importar forem pequenos, considere aumentar o número de pedidos paralelos. Se este número aumentar muito, a importação pode apresentar limite de velocidade.
2. Desativar a Geração Automática de Id: se todos os documentos a importar contiverem um campo de id, selecionar esta opção pode aumentar o desempenho. Os documentos em que falta um campo de id exclusivo não são importados.
3. Atualizar Documentos Existentes: a predefinição da ferramenta é não substituir os documentos existentes com conflitos de id. A seleção desta opção permite substituir os documentos existentes com ids correspondentes. Esta funcionalidade é útil para as migrações de dados agendadas que atualizam os documentos existentes.
4. Número de Tentativas em Caso de Falha: especifica o número de vezes a tentar a ligação ao Azure Cosmos DB em caso de falhas transitórias (por exemplo, interrupção de conectividade de rede).
5. Intervalo de Repetições: especifica o período de tempo de espera entre tentar estabelecer novamente a ligação ao Azure Cosmos DB em caso de falhas transitórias (por exemplo, interrupção de conectividade de rede).
6. Modo de Ligação: especifica o modo de ligação a utilizar com o Azure Cosmos DB. As opções disponíveis são DirectTcp, DirectHttps e Gateway. Os modos de ligação direta são mais rápidos, enquanto o modo de gateway é mais amigável com a firewall, pois só utiliza a porta 443.

![Captura de ecrã das opções avançadas de importação de registos sequenciais do Azure Cosmos DB](./media/import-data/documentdbsequentialoptions.png)

> [!TIP]
> A predefinição da ferramenta de importação é o modo de ligação de DirectTcp. Se ocorrerem problemas de firewall, mude para o modo de ligação de Gateway, pois só requer a porta 443.
> 
> 

## <a id="IndexingPolicy"></a>Especificar uma política de indexação
Ao permitir que a ferramenta de migração crie coleções de API de SQL do Azure Cosmos DB durante a importação, pode especificar a política de indexação das coleções. Na secção de opções avançadas de importação, nas opções de Volume do Azure Cosmos DB e registos Sequenciais do Azure Cosmos DB, navegue para a secção Política de Indexação.

![Captura de ecrã das opções avançadas da Política de Indexação do Azure Cosmos DB](./media/import-data/indexingpolicy1.png)

Ao utilizar a opção avançada Política de Indexação, pode selecionar um ficheiro de política de indexação, introduzir manualmente uma política de indexação ou selecionar de um conjunto de modelos predefinidos (ao clicar na caixa de texto da política de indexação).

Os modelos de política que a ferramenta fornece são:

* Predefinição. Esta política é ideal quando estiver a efetuar consultas de igualdade em relação a cadeias e a utilizar ORDENAR POR, consultas de intervalo e igualdade para números. Esta política tem uma tolerância de armazenamento de índice inferior ao Intervalo.
* Intervalo. Esta política é ideal quando estiver a utilizar ORDENAR POR, consultas de intervalo e igualdade em números e cadeias. Esta política tem uma tolerância de armazenamento de índice maior do que a Predefinição ou o Hash.

![Captura de ecrã das opções avançadas da Política de Indexação do Azure Cosmos DB](./media/import-data/indexingpolicy2.png)

> [!NOTE]
> Se não especificar uma política de indexação, a política predefinida é aplicada. Para obter mais informações sobre as políticas de indexação, veja [Políticas de indexação do Azure Cosmos DB](indexing-policies.md).
> 
> 

## <a name="export-to-json-file"></a>Exportar para ficheiro JSON
O exportador de JSON do Azure Cosmos DB permite exportar qualquer uma das opções de origem disponíveis para um ficheiro JSON que contém vários documentos JSON. A ferramenta realiza a exportação por si, ou pode optar por visualizar o comando de migração resultante e executar o comando. O ficheiro JSON resultante pode ser armazenado localmente ou no armazenamento de Blobs do Azure.

![Captura de ecrã da opção de exportação de ficheiro JSON local do Azure Cosmos DB](./media/import-data/jsontarget.png)

![Captura de ecrã da opção de exportação de armazenamento de Blobs JSON do Azure do Azure Cosmos DB](./media/import-data/jsontarget2.png)

Opcionalmente, pode optar por embelezar o JSON resultante, o que irá aumentar o tamanho do documento resultante, fazendo com que os conteúdos sejam mais legíveis por humanos.

    Standard JSON export
    [{"id":"Sample","Title":"About Paris","Language":{"Name":"English"},"Author":{"Name":"Don","Location":{"City":"Paris","Country":"France"}},"Content":"Don's document in Azure Cosmos DB is a valid JSON document as defined by the JSON spec.","PageViews":10000,"Topics":[{"Title":"History of Paris"},{"Title":"Places to see in Paris"}]}]

    Prettified JSON export
    [
     {
    "id": "Sample",
    "Title": "About Paris",
    "Language": {
      "Name": "English"
    },
    "Author": {
      "Name": "Don",
      "Location": {
        "City": "Paris",
        "Country": "France"
      }
    },
    "Content": "Don's document in Azure Cosmos DB is a valid JSON document as defined by the JSON spec.",
    "PageViews": 10000,
    "Topics": [
      {
        "Title": "History of Paris"
      },
      {
        "Title": "Places to see in Paris"
      }
    ]
    }]

Veja a seguir um exemplo da linha de comandos para exportar o ficheiro JSON para o Armazenamento de blobs do Azure:

```
dt.exe /ErrorDetails:All /s:DocumentDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB database_name>" /s.Collection:<CosmosDB collection_name>
/t:JsonFile /t.File:"blobs://<Storage account key>@<Storage account name>.blob.core.windows.net:443/<Container_name>/<Blob_name>"
/t.Overwrite
```

## <a name="advanced-configuration"></a>Configuração avançada
No ecrã de configuração Avançada, especifique a localização do ficheiro de registo no qual gostaria de escrever quaisquer erros. As seguintes regras aplicam-se a esta página:

1. Se não for fornecido um nome de ficheiro, todos os erros são devolvidos na página Resultados.
2. Se um nome de ficheiro for fornecido sem um diretório, o ficheiro é criado (ou substituído) no diretório de ambiente atual.
3. Se selecionar um ficheiro existente, o ficheiro será substituído, não existe nenhuma opção para anexar.
4. Em seguida, escolha se pretende registar todas as mensagens, só as de erros críticos ou as de inexistência de erros. Por fim, decida a frequência com a mensagem de transferência no ecrã é atualizada com o respetivo progresso.

   ![Captura de ecrã do ecrã Configuração avançada](./media/import-data/AdvancedConfiguration.png)

## <a name="confirm-import-settings-and-view-command-line"></a>Confirmar as definições de importação e ver a linha de comandos
1. Depois de especificar as informações de origem, as informações de destino e a configuração avançada, reveja o resumo da migração e, opcionalmente, veja/copie o comando resultante da migração (copiar o comando é útil para automatizar operações de importação):
   
    ![Captura de ecrã do ecrã de resumo](./media/import-data/summary.png)
   
    ![Captura de ecrã do ecrã de resumo](./media/import-data/summarycommand.png)
2. Quando estiver satisfeito com as opções de origem e destino, clique em **Importar**. O tempo decorrido, a contagem de itens transferidos e as informações das falhas (se não tiver fornecido um nome de ficheiro na configuração Avançada) são atualizados enquanto a importação está em curso. Depois de concluída, pode exportar os resultados (por exemplo, para lidar com eventuais falhas de importação).
   
    ![Captura de ecrã da opção de exportação de JSON do Azure Cosmos DB](./media/import-data/viewresults.png)
3. Também pode iniciar uma nova importação, mantendo as definições existentes (por exemplo, informações de cadeia de ligação, escolha de origem e destino, etc.) ou repondo todos os valores.
   
    ![Captura de ecrã da opção de exportação de JSON do Azure Cosmos DB](./media/import-data/newimport.png)

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, fez as seguintes tarefas:

> [!div class="checklist"]
> * Instalou a ferramenta de Migração de Dados
> * Importou dados de origens de dados diferentes
> * Exportou do Azure Cosmos DB para JSON

Agora pode avançar para o próximo tutorial e ficar a saber como consultar dados com o Azure Cosmos DB. 

> [!div class="nextstepaction"]
>[Como consultar dados?](../cosmos-db/tutorial-query-sql-api.md)
