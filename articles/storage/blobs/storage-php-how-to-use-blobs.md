---
title: Como utilizar o blob storage (armazenamento de objeto) do PHP | Microsoft Docs
description: "Armazene dados não estruturados na nuvem com o Blob Storage do Azure (armazenamento de objetos)."
documentationcenter: php
services: storage
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 1af56b59-b3f0-4b46-8441-aab463ae088e
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 01/11/2018
ms.author: tamram
ms.openlocfilehash: 6bc7fd799eddca14e728f965601acd244d88870c
ms.sourcegitcommit: a0d2423f1f277516ab2a15fe26afbc3db2f66e33
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/16/2018
---
# <a name="how-to-use-blob-storage-from-php"></a>Como utilizar o blob storage do PHP
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Descrição geral
O Blob Storage do Azure é um serviço que armazena dados não estruturados na nuvem como objetos/blobs. O Blob Storage pode armazenar qualquer tipo de texto ou de dados binários, tal como um documento, um ficheiro de multimédia ou um instalador da aplicação. O Blob Storage também é referido como um armazenamento de objetos.

Este guia mostra como efetuar cenários comuns utilizando o serviço blob do Azure. Os exemplos são escritos no PHP e a utilização de [biblioteca cliente de Blob de armazenamento do Azure para PHP][download]. Os cenários abrangidos incluem **carregar**, **listagem**, **transferir**, e **eliminar** blobs. Para obter mais informações sobre os blobs, consulte o [passos](#next-steps) secção.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Criar uma aplicação PHP
O único requisito para criar uma aplicação PHP que acede ao serviço blob do Azure é que a referência de classes no [biblioteca de clientes do Storage do Azure para PHP] [ download] de dentro do seu código. Pode utilizar as ferramentas de desenvolvimento para criar a sua aplicação, incluindo o bloco de notas.

Neste guia, utilize as funcionalidades de serviço de armazenamento de BLOBs, que podem ser chamadas dentro de uma aplicação PHP localmente ou numa código em execução dentro de uma função da web do Azure, a função de trabalho ou o Web site.

## <a name="get-the-azure-client-libraries"></a>Obtenha as bibliotecas de cliente do Azure
### <a name="install-via-composer"></a>Instalar através do compositor
1. Crie um ficheiro denominado **Composer** na raiz do projeto e adicione o seguinte código:
   
    ```json
    {
      "require": {
        "microsoft/azure-storage-blob": "*"
      }
    }
    ```
2. Transferir  **[composer.phar] [ composer-phar]**  na raiz do projeto.
3. Abra uma linha de comandos e execute o seguinte comando na raiz do projeto
   
    ```
    php composer.phar install
    ```

Em alternativa, vá para o [biblioteca de clientes de PHP de armazenamento do Azure] [ download] no GitHub para clonar o código de origem.

## <a name="configure-your-application-to-access-the-blob-service"></a>Configurar a sua aplicação para aceder ao serviço blob
Para utilizar as APIs do serviço blob do Azure, tem de:

1. Referenciar o ficheiro do Carregador automático utilizando o [require_once] declaração, e
2. Referência a quaisquer classes que poderá utilizar.

O exemplo seguinte mostra como inclui o Carregador automático ficheiros e a referência a **BlobRestProxy** classe.

```php
require_once 'vendor/autoload.php';
use MicrosoftAzure\Storage\Blob\BlobRestProxy;
```

Nos exemplos abaixo, o `require_once` instrução é sempre apresentada, mas apenas as classes de necessárias para o exemplo para executar são referenciadas.

## <a name="set-up-an-azure-storage-connection"></a>Configurar uma ligação de armazenamento do Azure
Ao instanciar um cliente do serviço blob do Azure, primeiro tem de ter uma cadeia de ligação válido. O formato para a cadeia de ligação de serviço blob é:

Para aceder a um serviço em direto:

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

Para aceder ao emulador de armazenamento:

```php
UseDevelopmentStorage=true
```

Para criar um cliente do serviço Blob do Azure, terá de utilizar o **BlobRestProxy** classe. Pode:

* passar a cadeia de ligação direta ou
* Utilize variáveis de ambiente na sua aplicação Web para armazenar a cadeia de ligação. Consulte [definições de configuração de aplicação web do Azure](../../app-service/web-sites-configure.md) documento para configurar as cadeias de ligação.

Para os exemplos aqui descritos, a cadeia de ligação é transmitida diretamente.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Blob\BlobRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

$blobClient = BlobRestProxy::createBlobService($connectionString);
```

## <a name="create-a-container"></a>Criar um contentor
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

A **BlobRestProxy** objeto permite-lhe criar um contentor do blob com o **createContainer** método. Ao criar um contentor, pode definir as opções no contentor, mas fazê-lo, por isso, não é necessário. (O exemplo seguinte mostra como definir a lista de controlo de acesso (ACL) de contentor e os metadados do contentor.)

```php
require_once 'vendor\autoload.php';

use MicrosoftAzure\Storage\Blob\BlobRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;
use MicrosoftAzure\Storage\Blob\Models\CreateContainerOptions;
use MicrosoftAzure\Storage\Blob\Models\PublicAccessType;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create blob client.
$blobClient = BlobRestProxy::createBlobService($connectionString);


// OPTIONAL: Set public access policy and metadata.
// Create container options object.
$createContainerOptions = new CreateContainerOptions();

// Set public access policy. Possible values are
// PublicAccessType::CONTAINER_AND_BLOBS and PublicAccessType::BLOBS_ONLY.
// CONTAINER_AND_BLOBS:
// Specifies full public read access for container and blob data.
// proxys can enumerate blobs within the container via anonymous
// request, but cannot enumerate containers within the storage account.
//
// BLOBS_ONLY:
// Specifies public read access for blobs. Blob data within this
// container can be read via anonymous request, but container data is not
// available. proxys cannot enumerate blobs within the container via
// anonymous request.
// If this value is not specified in the request, container data is
// private to the account owner.
$createContainerOptions->setPublicAccess(PublicAccessType::CONTAINER_AND_BLOBS);

// Set container metadata.
$createContainerOptions->addMetaData("key1", "value1");
$createContainerOptions->addMetaData("key2", "value2");

try    {
    // Create container.
    $blobClient->createContainer("mycontainer", $createContainerOptions);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Chamar **setPublicAccess (PublicAccessType::CONTAINER\_e\_BLOBS)** torna os dados de blob e contentor acessíveis via pedidos anónimos. Chamar **setPublicAccess(PublicAccessType::BLOBS_ONLY)** faz com que apenas acessíveis através de pedidos anónimos de dados de Blobs. Para mais informações sobre as ACLs do contentor, consulte [conjunto contentor ACL (REST API)][container-acl].

Para obter mais informações sobre códigos de erro do serviço Blob, consulte [códigos de erro do serviço Blob][error-codes].

## <a name="upload-a-blob-into-a-container"></a>Carregar um blob para um contentor
Para carregar um ficheiro como um blob, utilize o **BlobRestProxy -> createBlockBlob** método. Esta operação cria o blob, se não existir ou substitui-lo se existir. Exemplo de código seguinte assume que o contentor já foi criado e utiliza [fopen] [ fopen] para abrir o ficheiro como uma transmissão em fluxo.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Blob\BlobRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create blob REST proxy.
$blobClient = BlobRestProxy::createBlobService($connectionString);

$content = fopen("c:\myfile.txt", "r");
$blob_name = "myblob";

try    {
    //Upload blob
    $blobClient->createBlockBlob("mycontainer", $blob_name, $content);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Tenha em atenção que a anterior exemplo carrega um blob como uma transmissão em fluxo. No entanto, um blob pode também ser também carregado como uma cadeia utilizando, por exemplo, o [ficheiro\_obter\_conteúdo] [ file_get_contents] função. Para fazê-lo utilizando o exemplo anterior, altere `$content = fopen("c:\myfile.txt", "r");` para `$content = file_get_contents("c:\myfile.txt");`.

## <a name="list-the-blobs-in-a-container"></a>Listar os blobs num contentor
Para listar os blobs num contentor, utilize o **BlobRestProxy -> listBlobs** método com um **foreach** ciclo para ciclo através de resultado. O código seguinte mostra o nome de cada blob como saída num contentor e seu respetivo URI para o browser.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Blob\BlobRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;

// Create blob REST proxy.
$blobClient = BlobRestProxy::createBlobService($connectionString);

try    {
    // List blobs.
    $blob_list = $blobClient->listBlobs("mycontainer");
    $blobs = $blob_list->getBlobs();

    foreach($blobs as $blob)
    {
        echo $blob->getName().": ".$blob->getUrl()."<br />";
    }
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="download-a-blob"></a>Transferir um blob
Para transferir um blob, chame o **BlobRestProxy -> getBlob** método, em seguida, chame o **getContentStream** método no resultante **GetBlobResult** objeto.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Blob\BlobRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;

// Create blob REST proxy.
$blobClient = BlobRestProxy::createBlobService($connectionString);


try    {
    // Get blob.
    $blob = $blobClient->getBlob("mycontainer", "myblob");
    fpassthru($blob->getContentStream());
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Tenha em atenção que o exemplo acima obtém um blob como um recurso de fluxo (o comportamento predefinido). No entanto, pode utilizar o [fluxo\_obter\_conteúdo] [ stream-get-contents] função para converter o fluxo devolvido numa cadeia.

## <a name="delete-a-blob"></a>Eliminar um blob
Para eliminar um blob, transmitir o nome do contentor e o nome do blob para **BlobRestProxy -> deleteBlob**.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Blob\BlobRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;

// Create blob REST proxy.
$blobClient = BlobRestProxy::createBlobService($connectionString);

try    {
    // Delete blob.
    $blobClient->deleteBlob("mycontainer", "myblob");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="delete-a-blob-container"></a>Eliminar um contentor do blob
Por fim, para eliminar um contentor de blob, transmitir o nome do contentor para **BlobRestProxy -> deleteContainer**.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Blob\BlobRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;

// Create blob REST proxy.
$blobClient = BlobRestProxy::createBlobService($connectionString);

try    {
    // Delete container.
    $blobClient->deleteContainer("mycontainer");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="next-steps"></a>Passos Seguintes
Agora que aprendeu as noções básicas do serviço blob do Azure, siga estas ligações para saber mais sobre as tarefas de armazenamento mais complexas.

* Visite o [referência da API para a biblioteca de clientes do Storage do Azure PHP](http://azure.github.io/azure-storage-php/)
* Consulte o [exemplo de Blob avançadas](https://github.com/Azure/azure-storage-php/blob/master/samples/BlobSamples.php).

[download]: https://github.com/Azure/azure-storage-php
[container-acl]: http://msdn.microsoft.com/library/azure/dd179391.aspx
[error-codes]: http://msdn.microsoft.com/library/azure/dd179439.aspx
[file_get_contents]: http://php.net/file_get_contents
[require_once]: http://php.net/require_once
[fopen]: http://www.php.net/fopen
[stream-get-contents]: http://www.php.net/stream_get_contents
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[composer-phar]: http://getcomposer.org/composer.phar
