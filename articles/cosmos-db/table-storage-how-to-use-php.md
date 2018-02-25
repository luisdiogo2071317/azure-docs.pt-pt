---
title: "Como utilizar o serviço tabela de armazenamento do Azure ou a API de tabela do Azure Cosmos DB de PHP | Microsoft Docs"
description: "Saiba como utilizar a API do serviço de tabela do PHP para criar e eliminar uma tabela e inserir, eliminar e consultar a tabela."
services: cosmos-db
documentationcenter: php
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: 1e57f371-6208-4753-b2a0-05db4aede8e3
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: php
ms.topic: article
ms.date: 02/22/2018
ms.author: mimig
ms.openlocfilehash: 4965247d77e8a3a9f5dbaa2e70952993b4bdf4ff
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/24/2018
---
# <a name="how-to-use-azure-storage-table-service-or-cosmos-db-table-api-from-php"></a>Como utilizar o serviço tabela de armazenamento do Azure ou a API de tabela do Cosmos BD do PHP
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

## <a name="overview"></a>Descrição geral
Este guia mostra como efetuar cenários comuns utilizando o serviço de tabela de armazenamento do Azure e a API de tabela de base de dados do Azure Cosmos. Os exemplos são escritos no PHP e a utilização de [Azure SDK para PHP][download]. Os cenários abrangidos incluem **criar e eliminar uma tabela**, e **inserir, eliminar e consultar entidades numa tabela**. Para obter mais informações sobre o serviço de tabelas do Azure, consulte o [passos](#next-steps) secção.

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

## <a name="create-an-azure-service-account"></a>Criar uma conta de serviço do Azure

Pode trabalhar com tabelas utilizando o Table storage do Azure ou a API de tabela de base de dados do Azure Cosmos. Pode saber mais sobre as diferenças entre os serviços ao ler [tabela ofertas](table-introduction.md#table-offerings). Terá de criar uma conta para o serviço que vai utilizar. 

### <a name="create-an-azure-storage-account"></a>Criar uma conta de Armazenamento do Azure

É a forma mais fácil para criar a primeira conta de armazenamento utilizando o [portal do Azure](https://portal.azure.com). Para saber mais, veja [Criar uma conta de armazenamento](../storage/common/storage-create-storage-account.md#create-a-storage-account).

Também pode criar uma conta de armazenamento utilizando [Azure PowerShell](../storage/common/storage-powershell-guide-full.md) ou [CLI do Azure](../storage/common/storage-azure-cli.md).

Se preferir não criar uma conta de armazenamento neste momento, também pode utilizar o emulador de armazenamento do Azure para executar e testar o seu código num ambiente local. Para obter mais informações, veja [Utilizar o emulador de armazenamento do Azure para programação e teste](../storage/common/storage-use-emulator.md).

### <a name="create-an-azure-cosmos-db-account"></a>Criar uma conta do Azure Cosmos DB

Para obter instruções sobre como criar uma conta de base de dados do Azure Cosmos, consulte [criar uma conta de API de tabela](create-table-dotnet.md#create-a-database-account).

## <a name="create-a-php-application"></a>Criar uma aplicação PHP

É o único requisito para criar uma aplicação PHP para aceder ao serviço tabela de armazenamento ou a API de tabela de base de dados do Azure Cosmos referenciar classes na tabela de armazenamento do azure SDK para PHP a partir de dentro do seu código. Pode utilizar as ferramentas de desenvolvimento para criar a sua aplicação, incluindo o bloco de notas.

Neste guia, utiliza o serviço de tabela de armazenamento ou as funcionalidades de base de dados do Azure Cosmos que podem ser chamadas de dentro de uma aplicação PHP localmente, ou no código em execução dentro de uma função da web do Azure, a função de trabalho ou o Web site.

## <a name="get-the-client-library"></a>Obter a biblioteca de clientes

1. Crie um ficheiro denominado Composer na raiz do projeto e adicione o seguinte código:
```json
{
  "require": {
    "microsoft/azure-storage-table": "*"
  }
}
```
2. Transferir [composer.phar](http://getcomposer.org/composer.phar) na sua raiz. 
3. Abra uma linha de comandos e execute o seguinte comando na raiz do projeto:
```
php composer.phar install
```
Em alternativa, vá para o [biblioteca de clientes do PHP do Azure Storage tabela](https://github.com/Azure/azure-storage-php/tree/master/azure-storage-table) no GitHub para clonar o código de origem.


## <a name="add-required-references"></a>Adicione referências necessárias
Para utilizar o serviço tabela de armazenamento ou APIs de BD do Cosmos do Azure, tem de:

* Referenciar o ficheiro do Carregador automático utilizando o [require_once] [ require_once] declaração, e
* Referência a todas as classes que utiliza.

O exemplo seguinte mostra como inclui o Carregador automático ficheiros e a referência a **TableRestProxy** classe.

```php
require_once 'vendor/autoload.php';
use MicrosoftAzure\Storage\Table\TableRestProxy;
```

Nos exemplos abaixo, o `require_once` instrução sempre é apresentada, mas apenas as classes de necessárias para o exemplo para executar são referenciadas.

## <a name="add-a-storage-table-service-connection"></a>Adicionar uma ligação de serviço tabela de armazenamento
Ao instanciar um cliente do serviço tabela de armazenamento, primeiro tem de ter uma cadeia de ligação válido. O formato da cadeia de ligação de serviço tabela de armazenamento é:

```php
$connectionString = "DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]"
```

## <a name="add-an-azure-cosmos-db-connection"></a>Adicionar uma ligação de base de dados do Azure Cosmos
Ao instanciar um cliente de tabelas de base de dados do Cosmos do Azure, primeiro tem de ter uma cadeia de ligação válido. O formato de cadeia de ligação de base de dados do Azure Cosmos está:

```php
$connectionString = "DefaultEndpointsProtocol=[https];AccountName=[myaccount];AccountKey=[myaccountkey];TableEndpoint=[https://myendpoint/]";
```

## <a name="add-a-storage-emulator-connection"></a>Adicionar uma ligação de emulador de armazenamento
Aceder ao armazenamento de emulador:

```php
UseDevelopmentStorage = true
```

Para criar um cliente do serviço de Azure Table ou a base de dados do Azure Cosmos cliente, terá de utilizar o **TableRestProxy** classe. Pode:

* passar a cadeia de ligação direta ou
* Utilize o **CloudConfigurationManager (CCM)** para verificar a várias origens externas para a cadeia de ligação:
  * Por predefinição, este inclui suporte para uma origem externa - variáveis de ambientais.
  * Pode adicionar novas origens ao alargar o `ConnectionStringSource` classe.

Para os exemplos aqui descritos, a cadeia de ligação é transmitida diretamente.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;

$tableClient = TableRestProxy::createTableService($connectionString);
```

## <a name="create-a-table"></a>Criar uma tabela
A **TableRestProxy** objeto permite-lhe criar uma tabela com o **createTable** método. Ao criar uma tabela, pode definir o limite de tempo de serviço tabela. (Para obter mais informações sobre o limite de tempo de serviço tabela, consulte [tempos limite de definição para operações de serviço tabela][table-service-timeouts].)

```php
require_once 'vendor\autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create Table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

try    {
    // Create table.
    $tableClient->createTable("mytable");
}
catch(ServiceException $e){
    $code = $e->getCode();
    $error_message = $e->getMessage();
    // Handle exception based on error codes and messages.
    // Error codes and messages can be found here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
}
```

Para informações sobre restrições de nomes de tabela, consulte [compreender o modelo de dados do serviço tabela][table-data-model].

## <a name="add-an-entity-to-a-table"></a>Adicionar uma entidade a uma tabela
Para adicionar uma entidade a uma tabela, crie um novo **entidade** de objeto e transmiti-lo para **TableRestProxy -> insertEntity**. Tenha em atenção que quando criar uma entidade, terá de especificar um `PartitionKey` e `RowKey`. Estes são os identificadores exclusivos para uma entidade e são valores que podem ser consultados muito mais rapidamente do que outras propriedades de entidade. O sistema utiliza `PartitionKey` para distribuir automaticamente o entidades da tabela através de vários nós de armazenamento. Entidades com o mesmo `PartitionKey` são armazenados no mesmo nó. (Efetuar operações em várias entidades armazenadas no mesmo nó melhor do que em entidades armazenadas em nós diferentes.) O `RowKey` é o ID exclusivo de uma entidade dentro de uma partição.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

$entity = new Entity();
$entity->setPartitionKey("tasksSeattle");
$entity->setRowKey("1");
$entity->addProperty("Description", null, "Take out the trash.");
$entity->addProperty("DueDate",
                        EdmType::DATETIME,
                        new DateTime("2012-11-05T08:15:00-08:00"));
$entity->addProperty("Location", EdmType::STRING, "Home");

try{
    $tableClient->insertEntity("mytable", $entity);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
}
```

Para obter informações sobre as propriedades de tabela e tipos, consulte [compreender o modelo de dados do serviço tabela][table-data-model].

O **TableRestProxy** classe fornece dois métodos alternativos para inserir entidades: **insertOrMergeEntity** e **insertOrReplaceEntity**. Para utilizar estes métodos, crie um novo **entidade** e transmita-o como um parâmetro para cada método. Cada método de inserir a entidade se não existir. Se a entidade já existir, **insertOrMergeEntity** atualiza os valores de propriedade se as propriedades já existem e adiciona novas propriedades se não existir, enquanto **insertOrReplaceEntity** completamente substitui uma entidade existente. O exemplo seguinte mostra como utilizar **insertOrMergeEntity**. Se a entidade com `PartitionKey` "tasksSeattle" e `RowKey` "1" ainda não existir, será inserido. No entanto, se anteriormente tiver sido introduzida (conforme ilustrado no exemplo acima), o `DueDate` propriedade é atualizada e o `Status` propriedade é adicionada. O `Description` e `Location` propriedades também são atualizadas, mas com valores que efetivamente deixe-os inalterado. Se estas duas propriedades última não foram adicionadas, conforme mostrado no exemplo, mas existiam na entidade de destino, os respetivos valores existentes permanecerá inalterados.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

//Create new entity.
$entity = new Entity();

// PartitionKey and RowKey are required.
$entity->setPartitionKey("tasksSeattle");
$entity->setRowKey("1");

// If entity exists, existing properties are updated with new values and
// new properties are added. Missing properties are unchanged.
$entity->addProperty("Description", null, "Take out the trash.");
$entity->addProperty("DueDate", EdmType::DATETIME, new DateTime()); // Modified the DueDate field.
$entity->addProperty("Location", EdmType::STRING, "Home");
$entity->addProperty("Status", EdmType::STRING, "Complete"); // Added Status field.

try    {
    // Calling insertOrReplaceEntity, instead of insertOrMergeEntity as shown,
    // would simply replace the entity with PartitionKey "tasksSeattle" and RowKey "1".
    $tableClient->insertOrMergeEntity("mytable", $entity);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="retrieve-a-single-entity"></a>Obter uma única entidade
O **TableRestProxy -> getEntity** método permite-lhe obter uma entidade única através de consultas para o respetivo `PartitionKey` e `RowKey`. No exemplo abaixo, a chave de partição `tasksSeattle` e a chave de linha `1` são transmitidos para o **getEntity** método.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

try    {
    $result = $tableClient->getEntity("mytable", "tasksSeattle", 1);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$entity = $result->getEntity();

echo $entity->getPartitionKey().":".$entity->getRowKey();
```

## <a name="retrieve-all-entities-in-a-partition"></a>Obter todas as entidades numa partição
Consultas de entidade são construídas utilizando filtros (para obter mais informações, consulte [consultar tabelas e entidades][filters]). Para obter todas as entidades numa partição, utilize o filtro "PartitionKey eq *partition_name*". O exemplo seguinte mostra como obter todas as entidades no `tasksSeattle` partição transferindo um filtro para o **queryEntities** método.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

$filter = "PartitionKey eq 'tasksSeattle'";

try    {
    $result = $tableClient->queryEntities("mytable", $filter);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$entities = $result->getEntities();

foreach($entities as $entity){
    echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
}
```

## <a name="retrieve-a-subset-of-entities-in-a-partition"></a>Obter um subconjunto de entidades numa partição
O mesmo padrão utilizado no exemplo anterior, pode ser utilizado para obter qualquer subconjunto de entidades numa partição. O subconjunto de entidades é possível obter são determinados pelo filtro de utilizar (para obter mais informações, consulte [consultar tabelas e entidades][filters]). O exemplo seguinte mostra como utilizar um filtro para obter todas as entidades com um específico `Location` e um `DueDate` menor do que uma data especificada.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

$filter = "Location eq 'Office' and DueDate lt '2012-11-5'";

try    {
    $result = $tableClient->queryEntities("mytable", $filter);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$entities = $result->getEntities();

foreach($entities as $entity){
    echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
}
```

## <a name="retrieve-a-subset-of-entity-properties"></a>Obter um subconjunto de propriedades de entidade
Uma consulta pode obter um subconjunto de propriedades de entidade. Esta técnica, denominada *projecção*, reduz a largura de banda e pode melhorar o desempenho de consulta, especialmente para entidades grandes. Para especificar uma propriedade para obter, transmitir o nome da propriedade para o **consulta -> addSelectField** método. Pode chamar este método várias vezes para adicionar propriedades. Depois de executar **TableRestProxy -> queryEntities**, as entidades devolvidas só terão das propriedades selecionadas. (Se pretender regressar um subconjunto de entidades de tabela, utilize um filtro conforme mostrado nas consultas acima.)

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\QueryEntitiesOptions;

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

$options = new QueryEntitiesOptions();
$options->addSelectField("Description");

try    {
    $result = $tableClient->queryEntities("mytable", $options);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

// All entities in the table are returned, regardless of whether
// they have the Description field.
// To limit the results returned, use a filter.
$entities = $result->getEntities();

foreach($entities as $entity){
    $description = $entity->getProperty("Description")->getValue();
    echo $description."<br />";
}
```

## <a name="update-an-entity"></a>Atualizar uma entidade
Pode atualizar uma entidade existente ao utilizar o **entidade -> setProperty** e **entidade -> addProperty** métodos na entidade e, em seguida, chamar **TableRestProxy -> updateEntity**. O exemplo seguinte obtém uma entidade, modifica uma propriedade, remove outra propriedade e adiciona uma nova propriedade. Tenha em atenção que pode remover uma propriedade, definindo o valor como **nulo**.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

$result = $tableClient->getEntity("mytable", "tasksSeattle", 1);

$entity = $result->getEntity();
$entity->setPropertyValue("DueDate", new DateTime()); //Modified DueDate.
$entity->setPropertyValue("Location", null); //Removed Location.
$entity->addProperty("Status", EdmType::STRING, "In progress"); //Added Status.

try    {
    $tableClient->updateEntity("mytable", $entity);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="delete-an-entity"></a>Eliminar uma entidade
Para eliminar uma entidade, transmitir o nome da tabela e a entidade `PartitionKey` e `RowKey` para o **TableRestProxy -> deleteEntity** método.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

try    {
    // Delete entity.
    $tableClient->deleteEntity("mytable", "tasksSeattle", "2");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Para verificações de simultaneidade, pode definir Etag de uma entidade eliminada utilizando o **DeleteEntityOptions -> setEtag** método e transmitir o **DeleteEntityOptions** object  **deleteEntity** como um parâmetro quarto.

## <a name="batch-table-operations"></a>Operações de tabela do batch
O **TableRestProxy -> batch** método permite-lhe executar várias operações num único pedido. O padrão aqui envolve adicionar operações a **BatchRequest** objeto e, em seguida, transmitir o **BatchRequest** de objeto para o **TableRestProxy -> batch** método. Para adicionar uma operação para um **BatchRequest** objeto, pode chamar qualquer um dos seguintes métodos várias vezes:

* **addInsertEntity** (adiciona uma operação insertEntity)
* **addUpdateEntity** (adiciona uma operação updateEntity)
* **addMergeEntity** (adiciona uma operação mergeEntity)
* **addInsertOrReplaceEntity** (adiciona uma operação insertOrReplaceEntity)
* **addInsertOrMergeEntity** (adiciona uma operação insertOrMergeEntity)
* **addDeleteEntity** (adiciona uma operação deleteEntity)

O exemplo seguinte mostra como executar **insertEntity** e **deleteEntity** operações num único pedido. 

> [!NOTE]
> BD do Azure do Cosmos ainda não suporta operações de batch de tabelas. 

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;
use MicrosoftAzure\Storage\Table\Models\BatchOperations;

// Configure a connection string for Storage Table service.
$connectionString = "DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]"

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

// Create list of batch operation.
$operations = new BatchOperations();

$entity1 = new Entity();
$entity1->setPartitionKey("tasksSeattle");
$entity1->setRowKey("2");
$entity1->addProperty("Description", null, "Clean roof gutters.");
$entity1->addProperty("DueDate",
                        EdmType::DATETIME,
                        new DateTime("2012-11-05T08:15:00-08:00"));
$entity1->addProperty("Location", EdmType::STRING, "Home");

// Add operation to list of batch operations.
$operations->addInsertEntity("mytable", $entity1);

// Add operation to list of batch operations.
$operations->addDeleteEntity("mytable", "tasksSeattle", "1");

try    {
    $tableClient->batch($operations);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Para obter mais informações sobre a criação de batches de operações da tabela, consulte [efetuar transações de grupo de entidade][entity-group-transactions].

## <a name="delete-a-table"></a>Eliminar uma tabela
Por fim, para eliminar uma tabela, transmitir o nome de tabela para o **TableRestProxy -> deleteTable** método.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

try    {
    // Delete table.
    $tableClient->deleteTable("mytable");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="next-steps"></a>Passos Seguintes
Agora que aprendeu as noções básicas do serviço de tabelas do Azure e Azure Cosmos DB, siga estas ligações para saber mais.

* O [Explorador de Armazenamento do Microsoft Azure](../vs-azure-tools-storage-manage-with-storage-explorer.md) é uma aplicação autónoma e gratuita da Microsoft, que lhe permite trabalhar visualmente com dados do Armazenamento do Azure no Windows, macOS e Linux.

* [Centro para programadores do PHP](/develop/php/).

[download]: https://packagist.org/packages/microsoft/azure-storage-table
[require_once]: http://php.net/require_once
[table-service-timeouts]: https://docs.microsoft.com/rest/api/storageservices/setting-timeouts-for-table-service-operations

[table-data-model]: https://docs.microsoft.com/rest/api/storageservices/Understanding-the-Table-Service-Data-Model
[filters]: https://docs.microsoft.com/rest/api/storageservices/Querying-Tables-and-Entities
[entity-group-transactions]: https://docs.microsoft.com/rest/api/storageservices/Performing-Entity-Group-Transactions
