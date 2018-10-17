---
title: Utilizar o Azure Event Grid para automatizar o redimensionamento de imagens carregadas | Microsoft Docs
description: O Azure Event Grid pode acionar carregamentos de blobs no Armazenamento do Azure. Pode utilizá-lo para enviar ficheiros de imagem carregados para o Armazenamento do Azure para outros serviços, como as Funções do Azure, para redimensionar e outras melhorias.
services: event-grid, functions
author: ggailey777
manager: cfowler
editor: ''
ms.service: event-grid
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/29/2018
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 2d94389ade02cb6e61f192e9b9e8adb8f8ceec31
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/01/2018
ms.locfileid: "47585582"
---
# <a name="automate-resizing-uploaded-images-using-event-grid"></a>Automatizar o redimensionamento de imagens carregadas com o Event Grid

O [Azure Event Grid](overview.md) é um serviço de eventos para a cloud. O Event Grid permite criar subscrições para eventos gerados pelos serviços do Azure ou recursos de terceiros.  

Este tutorial é a segunda parte de uma série de tutoriais sobre Armazenamento. Complementa o [tutorial de Armazenamento anterior][previous-tutorial] para adicionar a geração automática de miniaturas sem servidor com o Azure Event Grid e as Funções do Azure. O Event Grid permite às [Funções do Azure](..\azure-functions\functions-overview.md) responder a eventos do [Armazenamento de Blobs do Azure](..\storage\blobs\storage-blobs-introduction.md) e gerar miniaturas das imagens carregadas. É criada uma subscrição de evento relativamente ao evento de criação de armazenamento de Blobs. Quando é adicionado um blob a um contentor de armazenamento de Blobs específico, é chamado um ponto final de função. Os dados transmitidos ao enlace de função a partir do Event Grid são utilizados para aceder ao blob e gerar a imagem em miniatura.

Utilize a CLI do Azure e o portal do Azure para adicionar a funcionalidade de redimensionamento a uma aplicação de carregamento de imagens existente.

![Aplicação Web publicada no browser Microsoft Edge](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png)

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma conta de Armazenamento do Azure geral
> * Implementar código sem servidor com as Funções do Azure
> * Criar uma subscrição de evento de armazenamento de Blobs no Event Grid

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial:

Tem de ter concluído o tutorial de armazenamento de Blobs anterior: [Carregar dados de imagem na cloud com o Armazenamento do Azure][previous-tutorial].

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

Se não registou o fornecedor de recursos do Event Grid na sua subscrição anteriormente, confirme que está registado.

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.EventGrid
```

```azurecli-interactive
az provider register --namespace Microsoft.EventGrid
```

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e usar a CLI localmente, este tutorial requer a execução da versão 2.0.14 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure]( /cli/azure/install-azure-cli). 

Se não estiver a utilizar o Cloud Shell, tem primeiro de iniciar sessão com o `az login`.

## <a name="create-an-azure-storage-account"></a>Criar uma conta de Armazenamento do Azure

As Funções do Azure requerem uma conta de armazenamento geral. Crie uma conta de armazenamento geral separada no grupo de recursos com o comando [az storage account create](/cli/azure/storage/account#az-storage-account-create).

Os nomes das contas do Storage devem ter entre 3 e 24 carateres de comprimento e apenas podem conter números e letras minúsculas. 

No comando seguinte, substitua o nome da conta de armazenamento geral globalmente exclusivo onde vir o marcador de posição `<general_storage_account>`. 

```azurecli-interactive
az storage account create --name <general_storage_account> \
--location westcentralus --resource-group myResourceGroup \
--sku Standard_LRS --kind storage
```

## <a name="create-a-function-app"></a>Criar uma aplicação de função  

Precisa de uma aplicação de funções para alojar a execução da sua função. A aplicação Function App proporciona um ambiente para a execução sem servidor do código da sua função. Utilize o comando [az functionapp create](/cli/azure/functionapp#az-functionapp-create) para criar uma aplicação Function App. 

No comando seguinte, substitua o nome da aplicação de funções exclusivo onde vir o marcador de posição `<function_app>`. O nome da aplicação de funções vai ser utilizado como o domínio DNS predefinido para a aplicação de funções, por isso o nome tem de ser exclusivo em todas as aplicações no Azure. Para `<general_storage_account>`, substitua o nome da conta de armazenamento geral que criou.

```azurecli-interactive
az functionapp create --name <function_app> --storage-account  <general_storage_account>  \
--resource-group myResourceGroup --consumption-plan-location westcentralus
```

Agora tem de configurar a aplicação de funções para ligar à Conta de armazenamento de blobs que criou no [tutorial anterior][previous-tutorial].

## <a name="configure-the-function-app"></a>Configurar a aplicação de funções

A função precisa da cadeia de ligação para ligar à Conta de armazenamento de blobs. O código de função que implementar no Azure no passo seguinte procura a cadeia de ligação na definição da aplicação myblobstorage_STORAGE e procura o nome do contentor da imagem da miniatura na definição da aplicação myContainerName. Obtenha a cadeia de ligação com o comando [az storage account show-connection-string](/cli/azure/storage/account#show-connection-string). Configure as definições da aplicação com o comando [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#set).

Nos seguintes comandos da CLI, `<blob_storage_account>` é o nome da conta de armazenamento de Blobs que criou no tutorial anterior.

```azurecli-interactive
storageConnectionString=$(az storage account show-connection-string \
--resource-group myResourceGroup --name <blob_storage_account> \
--query connectionString --output tsv)

az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings myblobstorage_STORAGE=$storageConnectionString \
myContainerName=thumbnails FUNCTIONS_EXTENSION_VERSION=~2
```

A definição `FUNCTIONS_EXTENSION_VERSION=~2` determina que a aplicação de funções seja executada na versão 2.x do runtime das Funções do Azure.

Agora, pode implementar um projeto de código de função nesta aplicação de funções.

## <a name="deploy-the-function-code"></a>Implementar o código de função 

# <a name="nettabnet"></a>[\.NET](#tab/net)

O redimensionamento do script (.csx) do C# de exemplo está disponível no [GitHub](https://github.com/Azure-Samples/function-image-upload-resize). Implemente este projeto de código de Funções na aplicação de funções com o comando [az functionapp deployment source config](/cli/azure/functionapp/deployment/source#config). 

No comando seguinte, `<function_app>` é o nome da aplicação de funções que criou anteriormente.

```azurecli-interactive
az functionapp deployment source config --name <function_app> \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/function-image-upload-resize
```

# <a name="nodejstabnodejs"></a>[Node.js](#tab/nodejs)
A função de redimensionamento do Node.js de exemplo está disponível no [GitHub](https://github.com/Azure-Samples/storage-blob-resize-function-node). Implemente este projeto de código de Funções na aplicação de funções com o comando [az functionapp deployment source config](/cli/azure/functionapp/deployment/source#config).

No comando seguinte, `<function_app>` é o nome da aplicação de funções que criou anteriormente.

```azurecli-interactive
az functionapp deployment source config --name <function_app> \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-resize-function-node
```
---

A função de redimensionamento da imagem é acionada por pedidos de HTTP enviados a partir do serviço Event Grid. Indica ao Event Grid que pretende obter estas notificações no URL da sua função ao criar uma subscrição de evento. Neste tutorial, subscreve eventos criados no blob.

Os dados transmitidos para a função a partir da notificação do Event Grid incluem o URL do blob. Esse URL é transmitido para o enlace de entrada para obter a imagem carregada a partir do armazenamento de Blobs. A função gera uma imagem em miniatura e escreve o fluxo resultante num contentor separado no armazenamento de Blobs. 

Este projeto utiliza `EventGridTrigger` para o tipo de acionador. É recomendado utilizar o acionador do Event Grid através de acionadores HTTP genéricos. O Event Grid valida automaticamente os acionadores de função do Event Grid. Com os acionadores HTTP genéricos, tem de implementar a [resposta de validação](security-authentication.md#webhook-event-delivery).

Para obter mais informações sobre esta função, veja os [ficheiros function.json e run.csx](https://github.com/Azure-Samples/function-image-upload-resize/tree/master/imageresizerfunc).
 
O código de projeto de função é implementado diretamente a partir do repositório de exemplo público. Para saber mais sobre as opções de implementação para as Funções do Azure, veja [Implementação contínua para Funções do Azure](../azure-functions/functions-continuous-deployment.md).

## <a name="create-an-event-subscription"></a>Criar uma subscrição de evento

Uma subscrição de evento indica que eventos gerados pelo fornecedor quer que sejam enviados para um ponto final específico. Neste caso, o ponto final é exposto pela sua função. Utilize os passos seguintes para criar uma subscrição de evento que envia notificações para a sua função no portal do Azure: 

1. No [portal do Azure](https://portal.azure.com), clique na seta na parte inferior esquerda para expandir todos os serviços, escreva *funções* no campo **Filtro** e selecione **Aplicações de Funções**. 

    ![Navegar para Aplicações de Funções no portal do Azure](./media/resize-images-on-storage-blob-upload-event/portal-find-functions.png)

2. Expanda a aplicação de funções, selecione a função **imageresizerfunc** e selecione **Adicionar uma subscrição do Event Grid**.

    ![Navegar para Aplicações de Funções no portal do Azure](./media/resize-images-on-storage-blob-upload-event/add-event-subscription.png)

3. Utilize as definições de subscrição de evento especificadas na tabela.
    
    ![Criar uma subscrição de evento a partir da função no portal do Azure](./media/resize-images-on-storage-blob-upload-event/event-subscription-create-flow.png)

    | Definição      | Valor sugerido  | Descrição                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nome** | imageresizersub | Nome que identifica a nova subscrição de evento. | 
    | **Tipo de tópico** |  Contas de armazenamento | Selecione o fornecedor de eventos da conta de Armazenamento. | 
    | **Subscrição** | A sua subscrição do Azure | Por predefinição, a subscrição do Azure atual está selecionada.   |
    | **Grupo de recursos** | myResourceGroup | Selecione **Utilizar existente** e selecione o grupo de recursos que tem utilizado neste tutorial.  |
    | **Instância** |  A sua conta de armazenamento de Blobs |  Selecione a conta de armazenamento de Blobs que criou. |
    | **Tipos de evento** | Criado pelo Blob | Desmarque todos os tipos diferentes de **Criado pelo Blob**. Apenas os tipos de evento de `Microsoft.Storage.BlobCreated` são transmitidos à função.| 
    | **Tipo de subscritor** |  Hook de Web |  As opções são Hook de Web ou Hubs de Eventos. |
    | **Ponto final do subscritor** | gerado automaticamente | Utilize o URL de ponto final gerado. | 
    | **Filtro de prefixo** | /blobServices/default/containers/images/blobs/ | Filtra os eventos de armazenamento para apenas os existentes no contentor de **imagens**.| 

4. Clique em **Criar** para adicionar a subscrição de evento. Esta ação cria uma subscrição de evento que aciona `imageresizerfunc` quando um blob é adicionado ao contentor de *imagens*. A função redimensiona as imagens e adiciona-as ao contentor de *miniaturas*.

Agora que os serviços de back-end estão configurados, teste a funcionalidade de redimensionamento de imagens na aplicação Web de exemplo. 

## <a name="test-the-sample-app"></a>Testar a aplicação de exemplo

Para testar o redimensionamento de imagens na aplicação Web, navegue para o URL da aplicação publicada. O URL predefinido da aplicação Web é `https://<web_app>.azurewebsites.net`.

Clique na região **Carregar fotografias** para selecionar e carregar um ficheiro. Também pode arrastar uma fotografia para esta região. 

Tenha em atenção que, depois de a imagem carregada desaparecer, é apresentada uma cópia da imagem carregada no carrossel **Miniaturas geradas**. Esta imagem foi redimensionada pela função, adicionada ao contentor de *miniaturas* e transferida pelo cliente Web.

![Aplicação Web publicada no browser Microsoft Edge](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png) 

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar uma conta de Armazenamento do Azure geral
> * Implementar código sem servidor com as Funções do Azure
> * Criar uma subscrição de evento de armazenamento de Blobs no Event Grid

Avance para a terceira parte da série do tutorial de Armazenamento para saber como proteger o acesso à conta de armazenamento.

> [!div class="nextstepaction"]
> [Proteger o acesso aos dados de aplicações na cloud](../storage/blobs/storage-secure-access-application.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

+ Para saber mais sobre o Event Grid, veja [Introdução ao Azure Event Grid](overview.md). 
+ Para experimentar outro tutorial que inclua Funções do Azure, veja [Criar uma função que se integra no Azure Logic Apps](..\azure-functions\functions-twitter-email.md). 

[previous-tutorial]: ../storage/blobs/storage-upload-process-images.md
