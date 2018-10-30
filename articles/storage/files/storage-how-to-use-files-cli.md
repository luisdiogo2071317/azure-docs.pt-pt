---
title: Início Rápido para gerir partilhas de ficheiros do Azure com a CLI do Azure
description: Utilize este início rápido para saber como utilizar a CLI do Azure para gerir Ficheiros do Azure.
services: storage
author: wmgries
ms.service: storage
ms.topic: quickstart
ms.date: 10/18/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: aab248ac7c9adf7d996406ec35e0317594ce0b68
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49945023"
---
# <a name="quickstart-create-and-manage-azure-file-shares-using-azure-cli"></a>Início Rápido: criar e gerir partilhas de ficheiros do Azure com a CLI do Azure
Este guia orienta-o pelas noções básicas da utilização de [partilhas de ficheiros do Azure](storage-files-introduction.md) com a CLI do Azure. As partilhas de ficheiros do Azure são como outras partilhas de ficheiros, mas armazenadas na cloud e apoiadas pela plataforma do Azure. As partilhas de ficheiros do Azure suportam o protocolo SMB padrão do setor e permite a partilha de ficheiros entre várias máquinas, aplicações e instâncias. 

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI do Azure localmente, para seguir os passos neste artigo, tem de executar a versão 2.0.4 da CLI do Azure ou uma versão posterior. Execute **az --version** para encontrar a versão da CLI do Azure. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure](/cli/azure/install-azure-cli). 

Por predefinição, os comandos da CLI do Azure devolvem JavaScript Object Notation (JSON). JSON é a forma padrão de enviar e receber mensagens a partir de APIs REST. Para facilitar a utilização das respostas JSON, alguns dos exemplos neste artigo utilizam o parâmetro *query* nos comandos da CLI do Azure. Este parâmetro utiliza a [linguagem de consulta JMESPath](http://jmespath.org/) para analisar o JSON. Para saber mais sobre como utilizar os resultados dos comandos da CLI do Azure ao seguir a linguagem de consulta do JMESPath, veja o [tutorial JMESPath](http://jmespath.org/tutorial.html).

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure
Se estiver a utilizar a CLI do Azure localmente, abra uma linha de comandos e inicie sessão no Azure, se ainda não o tiver feito.

```bash 
az login
```

## <a name="create-a-resource-group"></a>Criar um grupo de recursos
Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos. Se ainda não tiver um grupo de recursos do Azure, pode utilizar o comando [az group create](/cli/azure/group#create) para criar um. 

O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *E.U.A Leste*:

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-storage-account"></a>Criar uma conta de armazenamento
Uma conta de armazenamento é um conjunto partilhado de armazenamento no qual pode implementar partilhas de ficheiros do Azure ou outros recursos de armazenamento, como blobs ou filas. Uma conta de armazenamento pode conter um número ilimitado de partilhas de ficheiros. Uma partilha pode armazenar um número ilimitado de ficheiros, até aos limites de capacidade da conta de armazenamento.

O exemplo seguinte cria uma conta de armazenamento com o nome *mystorageaccount\<random number\>* através do comando [az storage account create](/cli/azure/storage/account#create) e, em seguida, coloca o nome dessa conta de armazenamento na variável `$STORAGEACCT`. Os nomes da conta de armazenamento devem ser exclusivos. Utilizar `$RANDOM` associa um número ao nome da conta de armazenamento para o tornar único. 

```azurecli-interactive 
STORAGEACCT=$(az storage account create \
    --resource-group "myResourceGroup" \
    --name "mystorageacct$RANDOM" \
    --location eastus \
    --sku Standard_LRS \
    --query "name" | tr -d '"')
```

### <a name="get-the-storage-account-key"></a>Obter a chave da conta de armazenamento
As chaves de contas de armazenamento controlam o acesso aos recursos numa conta de armazenamento. As chaves são criadas automaticamente quando cria uma conta de armazenamento. Pode obter as chaves da conta de armazenamento para a mesma com o comando [az storage account keys list](/cli/azure/storage/account/keys#list): 

```azurecli-interactive 
STORAGEKEY=$(az storage account keys list \
    --resource-group "myResourceGroup" \
    --account-name $STORAGEACCT \
    --query "[0].value" | tr -d '"')
```

## <a name="create-an-azure-file-share"></a>Criar uma partilha de ficheiros do Azure
Agora, pode criar a sua primeira partilha de ficheiros do Azure. Crie partilhas de ficheiros com o comando [az storage share create](/cli/azure/storage/share#create). Este exemplo cria uma partilha de ficheiros do Azure com o nome *myshare*: 

```azurecli-interactive
az storage share create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" 
```

Os nomes das partilhas só podem ter letras minúsculas, números e hífenes (mas não podem começar com um hífen). Para obter detalhes completos sobre a nomenclatura de partilhas de ficheiros e ficheiros, veja [Naming and referencing shares, directories, files, and metadata (Nomenclatura e referência de partilhas, diretórios, ficheiros e metadados)](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata).

## <a name="use-your-azure-file-share"></a>Utilizar a partilha de ficheiros do Azure
O serviço Ficheiros do Azure fornece dois métodos para utilizar ficheiros e pastas na sua partilha de ficheiros do Azure : o [protocolo SMB (Server Message Block)](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) padrão do setor e o [protocolo REST de Ficheiros](https://docs.microsoft.com/rest/api/storageservices/file-service-rest-api). 

Para montar uma partilha de ficheiros com SMB, veja o documento seguinte com base no seu SO:
- [Linux](storage-how-to-use-files-linux.md)
- [macOS](storage-how-to-use-files-mac.md)
- [Windows](storage-how-to-use-files-windows.md)

### <a name="using-an-azure-file-share-with-the-file-rest-protocol"></a>Utilizar uma partilha de ficheiros do Azure com o protocolo REST de Ficheiros 
É possível trabalhar diretamente com o protocolo REST de Ficheiros (ou seja, criar manualmente chamadas HTTP REST), mas a forma mais comum de utilizar o protocolo REST de Ficheiros é através da CLI do Azure, do [módulo AzureRM PowerShell](storage-how-to-use-files-powershell.md), ou de um SDK do Armazenamento do Azure, sendo que todos oferecem um bom wrapper para o protocolo REST de Ficheiros na linguagem de scripts/programação da sua preferência.  

Contamos que a maioria das utilizações dos Ficheiros do Azure irão querer trabalhar com a partilha de ficheiros do Azure através do protocolo SMB, pois permite-lhes utilizar as aplicações e ferramentas existentes, mas existem vários motivos pelos quais é vantajoso utilizar a API REST de Ficheiros em vez do SMB, tais como:

- Se estiver a navegar na partilha de ficheiros a partir do Azure Bash Cloud Shell (que não pode montar partilhas de ficheiros através de SMB).
- Se precisar de executar um script ou aplicação a partir de um cliente que não pode montar partilhas SMB, como clientes no local que não têm a porta 445 desbloqueada.
- Se estiver a tirar partido de recursos sem servidor, como o serviço [Funções do Azure](../../azure-functions/functions-overview.md). 

Os exemplos seguintes mostram como utilizar o módulo AzureRM PowerShell para manipular a partilha de ficheiros do Azure com o protocolo REST de Ficheiros. 

### <a name="create-a-directory"></a>Criar um diretório
Para criar um novo diretório com o nome *myDirectory* na raiz da partilha de ficheiros do Azure, utilize o comando [`az storage directory create`](/cli/azure/storage/directory#az_storage_directory_create):

```azurecli-interactive
az storage directory create \
   --account-name $STORAGEACCT \
   --account-key $STORAGEKEY \
   --share-name "myshare" \
   --name "myDirectory" 
```

### <a name="upload-a-file"></a>Carregar um ficheiro
Para demonstrar como carregar um ficheiro com o comando [`az storage file upload`](/cli/azure/storage/file#az_storage_file_upload), primeiro crie um ficheiro para carregar na unidade temporária do Cloud Shell. No exemplo seguinte, irá criar e carregar o ficheiro:

```azurecli-interactive
date > ~/clouddrive/SampleUpload.txt

az storage file upload \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --source "~/clouddrive/SampleUpload.txt" \
    --path "myDirectory/SampleUpload.txt"
```

Se estiver a executar a CLI do Azure localmente, substitua `~/clouddrive` por um caminho existente no seu computador.

Depois de carregar o ficheiro, pode utilizar o comando [`az storage file list`](/cli/azure/storage/file#az_storage_file_list) para assegurar que o ficheiro foi carregado para a partilha de ficheiros do Azure:

```azurecli-interactive
az storage file list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --path "myDirectory" \
    --output table
```

### <a name="download-a-file"></a>Transferir um ficheiro
Pode utilizar o comando [`az storage file download`](/cli/azure/storage/file#az_storage_file_download) para transferir uma cópia do ficheiro que carregou para a unidade temporária do Cloud Shell:

```azurecli-interactive
# Delete an existing file by the same name as SampleDownload.txt, if it exists, because you've run this example before
rm -rf ~/clouddrive/SampleDownload.txt

az storage file download \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare" \
    --path "myDirectory/SampleUpload.txt" \
    --dest "~/clouddrive/SampleDownload.txt"
```

### <a name="copy-files"></a>Copiar ficheiros
Uma tarefa comum é copiar ficheiros de uma partilha de ficheiros para outra, ou para um contentor de armazenamento de Blobs do Azure, ou a partir deste. Para demonstrar esta funcionalidade, crie uma nova partilha. Copie o ficheiro que carregou para esta nova partilha através do comando [az storage file copy](/cli/azure/storage/file/copy): 

```azurecli-interactive
az storage share create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare2"

az storage directory create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare2" \
    --name "myDirectory2"

az storage file copy start \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --source-share "myshare" \
    --source-path "myDirectory/SampleUpload.txt" \
    --destination-share "myshare2" \
    --destination-path "myDirectory2/SampleCopy.txt"
```

Agora, se listar os ficheiros na nova partilha, deverá ver o ficheiro copiado:

```azurecli-interactive
az storage file list \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --share-name "myshare2" \
    --output table
```

Embora o comando `az storage file copy start` seja conveniente para movimentações de ficheiros entre partilhas de ficheiros do Azure e contentores de armazenamento de Blobs do Azure, recomendamos que utilize o AzCopy para mover ficheiros maiores. (Maior em termos do número ou tamanho dos ficheiros a serem movidos.) Saiba mais sobre o [AzCopy para Linux](../common/storage-use-azcopy-linux.md) e [AzCopy para Windows](../common/storage-use-azcopy.md). O AzCopy tem de ser instalado localmente. O AzCopy não está disponível no Cloud Shell. 

## <a name="clean-up-resources"></a>Limpar recursos
Quando terminar, pode utilizar o comando [`az group delete`](/cli/azure/group#delete) para remover o grupo de recursos e todos os recursos relacionados: 

```azurecli-interactive 
az group delete --name "myResourceGroup"
```

Em alternativa, pode remover recursos individualmente.
- Para remover as partilhas de ficheiros do Azure criadas para este artigo:

    ```azurecli-interactive
    az storage share delete \
        --account-name $STORAGEACCT \
        --account-key $STORAGEKEY \
        --name "myshare" \
        --delete-snapshots include

    az storage share delete \
        --account-name $STORAGEACCT \
        --account-key $STORAGEKEY \
        --name "myshare2" \
        --delete-snapshots include
    ```

- Para remover a própria conta de armazenamento. (Isto remove implicitamente as partilhas de ficheiros do Azure que criou e quaisquer outros recursos de armazenamento que possa ter criado, como um contentor de armazenamento de Blobs do Azure.)

    ```azurecli-interactive
    az storage account delete \
        --resource-group "myResourceGroup" \
        --name $STORAGEACCT \
        --yes
    ```

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [O que são os Ficheiros do Azure?](storage-files-introduction.md)