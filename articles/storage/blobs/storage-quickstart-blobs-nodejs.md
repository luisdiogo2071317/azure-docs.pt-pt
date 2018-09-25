---
title: Início Rápido do Azure – Criar um blob no armazenamento de objetos com Node.js | Microsoft Docs
description: Neste início rápido, crie uma conta de armazenamento e um contentor no armazenamento de objetos (Blobs). Em seguida, utilize a biblioteca de clientes de armazenamento para Node.js, para carregar um blob para o Armazenamento do Microsoft Azure, transferir um blob e listar os blobs num contentor.
services: storage
author: craigshoemaker
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 04/09/2018
ms.author: cshoe
ms.openlocfilehash: b1cb7d327d8bfd9a7c6fe9d466445c50620f8b45
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46976898"
---
# <a name="quickstart-upload-download-and-list-blobs-using-nodejs"></a>Início Rápido: carregar, transferir e listar blobs através de Node.js

Neste início rápido, vai aprender a utilizar o Node.js, para carregar, transferir e listar blobs de blocos num contentor com o Armazenamento de blobs do Azure.

Para concluir este início rápido, precisa de uma [subscrição do Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

## <a name="download-the-sample-application"></a>Transferir a aplicação de exemplo

O [exemplo de aplicação](https://github.com/Azure-Samples/storage-blobs-node-quickstart.git) deste início rápido é uma aplicação de consola Node.js simples. Para começar, clone o repositório para o seu computador com o seguinte comando:

```bash
git clone https://github.com/Azure-Samples/storage-blobs-node-quickstart.git
```

Para abrir a aplicação, procure a pasta *storage-blobs-node-quickstart* e abra-a no seu ambiente de edição de código preferido.

[!INCLUDE [storage-copy-connection-string-portal](../../../includes/storage-copy-connection-string-portal.md)]

## <a name="configure-your-storage-connection-string"></a>Configurar a cadeia de ligação de armazenamento

Antes de executar a aplicação, tem de indicar a cadeia de ligação da sua conta de armazenamento. O repositório de exemplo inclui um ficheiro denominado *. env.example*. Pode mudar o nome deste ficheiro ao remover a extensão *.example*, o que resulta num ficheiro denominado *.env*. Dentro do ficheiro *.env*, adicione o valor da cadeia de ligação a seguir à chave *AZURE_STORAGE_CONNECTION_STRING*.

## <a name="install-required-packages"></a>Instalar pacotes necessários

No diretório da aplicação, execute *npm install* para instalar os pacotes necessários para a aplicação.

```bash
npm install
```

## <a name="run-the-sample"></a>Executar o exemplo
Agora que as dependências estão instaladas, pode executar o exemplo através da transferência de comandos para o script. Por exemplo, para criar um contentor de blobs execute o seguinte comando:

```bash
node index.js --command createContainer
```

Os comandos disponíveis incluem:

| Comando | Descrição |
|---------|---------|
|*createContainer* | Cria um contentor denominado *test-container* (uma operação bem-sucedida mesmo que o contentor já exista) |
|*upload*          | Carrega o ficheiro *example.txt* no contentor *test-container* |
|*download*        | Transfere o conteúdo do blob *example* para *example.downloaded.txt* |
|*eliminar*          | Elimina o blob *example* |
|*list*            | Lista o conteúdo do contentor *test-container* na consola |


## <a name="understanding-the-sample-code"></a>Compreender o código de exemplo
Este exemplo de código utiliza alguns módulos para servir de interface com o sistema de ficheiros e a linha de comandos. 

```javascript
if (process.env.NODE_ENV !== 'production') {
    require('dotenv').load();
}
const path = require('path');
const args = require('yargs').argv;
const storage = require('azure-storage');
```

O objetivo dos módulos é o seguinte: 

- *dotenv* carrega as variáveis de ambiente definidas num ficheiro denominado *.env* no contexto da execução atual
- *path* é necessário para determinar o caminho absoluto do ficheiro a carregar no armazenamento de blobs
- *yargs* expõe uma interface simples para aceder aos argumentos da linha de comandos
- *azure-storage* é o módulo do [SDK do Armazenamento do Azure](https://docs.microsoft.com/javascript/api/azure-storage) para o Node.js

Em seguida, é inicializada uma série de variáveis:

```javascript
const blobService = storage.createBlobService();
const containerName = 'test-container';
const sourceFilePath = path.resolve('./example.txt');
const blobName = path.basename(sourceFilePath, path.extname(sourceFilePath));
```

As variáveis são definidas para os seguintes valores:

- *blobService* é definido para uma nova instância do serviço Blob do Azure
- *containerName* é definido para o nome do contentor
- *sourceFilePath* é definido para o caminho absoluto do ficheiro a carregar
- *blobName* é criado através do aproveitamento do nome de ficheiro e a remoção da extensão do ficheiro

Na seguinte implementação, cada uma das funções *blobService* é encapsulada num wrapper *Promise*, o que permite o acesso à função *async* e ao operador *await* de JavaScript para simplificar o cariz de chamada de retorno da [API de Armazenamento do Azure](https://docs.microsoft.com/javascript/api/azure-storage/azurestorage.services.blob.blobservice.blobservice?view=azure-node-latest). Quando é devolvida uma resposta bem-sucedida para cada função, a promessa (Promise) é resolvida com dados relevantes, juntamente com uma mensagem específica para a ação.

### <a name="create-a-blob-container"></a>Criar um contentor de blobs

A função *createContainer* chama [createContainerIfNotExists](https://docs.microsoft.com/javascript/api/azure-storage/azurestorage.services.blob.blobservice.blobservice?view=azure-node-latest#createcontainerifnotexists) e define o nível de acesso apropriado para o blob.

```javascript
const createContainer = () => {
    return new Promise((resolve, reject) => {
        blobService.createContainerIfNotExists(containerName, { publicAccessLevel: 'blob' }, err => {
            if(err) {
                reject(err);
            } else {
                resolve({ message: `Container '${containerName}' created` });
            }
        });
    });
};
```

O segundo parâmetro (*options*) para **createContainerIfNotExists** aceita um valor para [publicAccessLevel](https://docs.microsoft.com/javascript/api/azure-storage/azurestorage.services.blob.blobservice.blobservice?view=azure-node-latest#createcontainerifnotexists). O valor *blob* para *publicAccessLevel* especifica que dados de blob específicos estão expostos ao público. Esta definição contrasta com o acesso de nível *container*, que concede a capacidade de listar o conteúdo do contentor.

A utilização de **createContainerIfNotExists** permite que a aplicação execute o comando *createContainer* várias vezes sem devolver erros quando o contentor já existe. Num ambiente de produção, o mais frequente é **createContainerIfNotExists** só ser chamado uma vez, já que o mesmo contentor é utilizado em toda a aplicação. Nestes casos, pode criar o contentor antecipadamente através do portal ou através da CLI do Azure.

### <a name="upload-a-blob-to-the-container"></a>Carregar um blob no contentor

A função *upload* utiliza a função [createBlockBlobFromLocalFile](https://docs.microsoft.com/javascript/api/azure-storage/azurestorage.services.blob.blobservice.blobservice?view=azure-node-latest#createblockblobfromlocalfile) para carregar e escrever ou substituir um ficheiro do sistema de ficheiros no armazenamento de blobs. 

```javascript
const upload = () => {
    return new Promise((resolve, reject) => {
        blobService.createBlockBlobFromLocalFile(containerName, blobName, sourceFilePath, err => {
            if(err) {
                reject(err);
            } else {
                resolve({ message: `Upload of '${blobName}' complete` });
            }
        });
    });
};
```
No contexto do exemplo de aplicação, o ficheiro denominado *example.txt* é carregado num blob com o nome *example* dentro de um contentor chamado *test-container*. Outras abordagens disponíveis para carregar conteúdo nos blobs incluem trabalhar com [texto](https://docs.microsoft.com/javascript/api/azure-storage/azurestorage.services.blob.blobservice.blobservice?view=azure-node-latest#createblockblobfromtext) e [fluxos](https://docs.microsoft.com/javascript/api/azure-storage/azurestorage.services.blob.blobservice.blobservice?view=azure-node-latest#createblockblobfromstream).

Para confirmar se o ficheiro está carregado no armazenamento de blobs, pode utilizar o [Explorador de Armazenamento do Azure](https://azure.microsoft.com/features/storage-explorer/) para ver os dados na sua conta.

### <a name="list-the-blobs-in-a-container"></a>Listar os blobs num contentor

A função *list* chama o método [listBlobsSegmented](https://docs.microsoft.com/javascript/api/azure-storage/azurestorage.services.blob.blobservice.blobservice?view=azure-node-latest#listblobssegmented) para devolver uma lista de metadados do blob num contentor. 

```javascript
const list = () => {
    return new Promise((resolve, reject) => {
        blobService.listBlobsSegmented(containerName, null, (err, data) => {
            if(err) {
                reject(err);
            } else {
                resolve({ message: `Items in container '${containerName}':`, data: data });
            }
        });
    });
};
```

Chamar *listBlobsSegmented* devolve os metadados do blob como uma matriz de instâncias [BlobResult](https://docs.microsoft.com/javascript/api/azure-storage/azurestorage.services.blob.blobservice.blobservice.blobresult?view=azure-node-latest). Os resultados são devolvidos em lotes (segmentos) de 5000 incrementos. Se existirem mais de 5000 blobs num contentor, os resultados incluem um valor para **continuationToken**. Para listar os segmentos subsequentes do contentor do blobs, pode transmitir o token de continuação novamente para o **listBlobSegmented** como um segundo argumento.

### <a name="download-a-blob-from-the-container"></a>Transferir um blob do contentor

A função *download* utiliza [getBlobToLocalFile](https://docs.microsoft.com/javascript/api/azure-storage/azurestorage.services.blob.blobservice.blobservice?view=azure-node-latest#getblobtolocalfile) para transferir o conteúdo do blob para o caminho absoluto do ficheiro especificado.

```javascript
const download = () => {
    const dowloadFilePath = sourceFilePath.replace('.txt', '.downloaded.txt');
    return new Promise((resolve, reject) => {
        blobService.getBlobToLocalFile(containerName, blobName, dowloadFilePath, err => {
            if(err) {
                reject(err);
            } else {
                resolve({ message: `Download of '${blobName}' complete` });
            }
        });
    });
};
```
A implementação mostrada aqui altera o caminho do ficheiro de origem para acrescentar *.downloaded.txt* ao nome do ficheiro. Em contextos do mundo real, pode alterar a localização, bem como o nome do ficheiro, quando selecionar um destino de transferência.

### <a name="delete-blobs-in-the-container"></a>Eliminar blobs no contentor

A função *deleteBlock* (utilizada como alias do comando de consola *delete*) chama a função [deleteBlobIfExists](https://docs.microsoft.com/javascript/api/azure-storage/azurestorage.services.blob.blobservice.blobservice?view=azure-node-latest#deleteblobifexists). Tal como o nome sugere, esta função não devolve um erro se o blob já tiver sido eliminado.

```javascript
const deleteBlock = () => {
    return new Promise((resolve, reject) => {
        blobService.deleteBlobIfExists(containerName, blobName, err => {
            if(err) {
                reject(err);
            } else {
                resolve({ message: `Block blob '${blobName}' deleted` });
            }
        });
    });
};
```

### <a name="upload-and-list"></a>Carregar e listar

Uma das vantagens inerentes à utilização de promessas reside na capacidade de encadear comandos. A função **uploadAndList** demonstra como é fácil listar o conteúdo de um Blob imediatamente a seguir ao carregamento de um ficheiro.

```javascript
const uploadAndList = () => {
    return _module.upload().then(_module.list);
};
```

### <a name="calling-code"></a>Código de chamada

A fim de expor as funções implementadas na linha de comandos, cada uma das funções é mapeada para um objeto literal.

```javascript
const _module = {
    "createContainer": createContainer,
    "upload": upload,
    "download": download,
    "delete": deleteBlock,
    "list": list,
    "uploadAndList": uploadAndList
};
```

Com o *_module* agora implementado, cada um dos comandos está disponível a partir da linha de comandos.

```javascript
const commandExists = () => exists = !!_module[args.command];
```

Se um comando especificado não existir, as propriedades de *_module* são apresentadas na consola como texto de ajuda para o utilizador. 

A função *executeCommand* é uma função *async*, a qual chama o comando especificado com o operador *await* e regista as mensagens nos dados da consola.

```javascript
const executeCommand = async () => {
    const response = await _module[args.command]();

    console.log(response.message);

    if (response.data) {
        response.data.entries.forEach(entry => {
            console.log('Name:', entry.name, ' Type:', entry.blobType)
        });
    }
};
```

Por último, o código de execução primeiro chama *commandExists* para confirmar que é transmitido um comando conhecido para o script. Se for selecionado um comando existente, o comando é executado e os eventuais erros são registados na consola.

```javascript
try {
    const cmd = args.command;

    console.log(`Executing '${cmd}'...`);

    if (commandExists()) {
        executeCommand();
    } else {
        console.log(`The '${cmd}' command does not exist. Try one of these:`);
        Object.keys(_module).forEach(key => console.log(` - ${key}`));
    }
} catch (e) {
    console.log(e);
}
```

## <a name="clean-up-resources"></a>Limpar recursos
Se não tenciona utilizar os dados ou as contas criadas neste artigo, é conveniente eliminá-los para evitar qualquer faturação indesejada. Para eliminar o blob e os contentores, pode utilizar os métodos [deleteBlobIfExists](https://docs.microsoft.com/javascript/api/azure-storage/azurestorage.services.blob.blobservice.blobservice?view=azure-node-latest#deleteblobifexists) e [deleteContainerIfExists](https://docs.microsoft.com/javascript/api/azure-storage/azurestorage.services.blob.blobservice.blobservice?view=azure-node-latest#deletecontainerifexists). Também pode eliminar a conta de armazenamento [através do portal](../common/storage-create-storage-account.md).

## <a name="resources-for-developing-nodejs-applications-with-blobs"></a>Recursos para desenvolver aplicações Node.js com blobs

Veja estes recursos adicionais para o desenvolvimento de Node.js com o Armazenamento de blobs:

### <a name="binaries-and-source-code"></a>Binários e código fonte

- Consulte e instale o [código fonte da biblioteca de cliente do Node.js](https://github.com/Azure/azure-storage-node) para o Armazenamento do Azure no GitHub.

### <a name="client-library-reference-and-samples"></a>Referência e exemplos da biblioteca de cliente

- Veja a [Referência da API de Node.js](https://docs.microsoft.com/javascript/api/overview/azure/storage) para obter mais informações sobre a biblioteca de cliente do Node.js.
- Explore os [Exemplos de armazenamento de blobs](https://azure.microsoft.com/resources/samples/?sort=0&service=storage&platform=nodejs&term=blob) escritos com a biblioteca de cliente do Node.js.

## <a name="next-steps"></a>Passos seguintes

Este início rápido demonstra como carregar um ficheiro entre um disco local e o Armazenamento de blobs do Azure com o Node.js. Para saber mais sobre a utilização do armazenamento de Blobs, avance para os procedimentos do armazenamento de blobs.

> [!div class="nextstepaction"]
> [Procedimentos de Operações de Armazenamento de Blobs](storage-nodejs-how-to-use-blob-storage.md)

Para a referência Node-js para Armazenamento do Microsoft Azure, consulte [pacote de armazenamento do azure](https://docs.microsoft.com/javascript/api/azure-storage).
