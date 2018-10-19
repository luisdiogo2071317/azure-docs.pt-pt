---
title: Início Rápido – Criar uma conta dos Serviços de Multimédia do Azure com a CLI do Azure| Microsoft Docs
description: Siga os passos deste início rápido para criar uma conta dos Serviços de Multimédia do Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: cflower
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/27/2018
ms.author: juliako
ms.openlocfilehash: abed9fd8d466b582b534b365f4be4257f4986435
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/17/2018
ms.locfileid: "45736007"
---
# <a name="quickstart-create-an-azure-media-services-account"></a>Início Rápido: Criar uma conta dos Serviços de Multimédia do Azure

> [!NOTE]
> A versão mais recente dos Serviços de Multimédia do Azure (30/3/2018) está em pré-visualização. Esta versão também é denominada v3. 

Quer seja um programador ou um criador de conteúdo multimédia, para armazenar, encriptar, codificar, gerir e transmitir conteúdo multimédia no Azure, terá de criar uma conta dos Serviços de Multimédia. Quando criar uma conta dos Serviços de Multimédia, terá de fornecer o ID de um recurso da conta de Armazenamento do Microsoft Azure. A conta de armazenamento especificada está ligada à sua conta dos Serviços de Multimédia. Este recurso da conta de armazenamento tem de estar localizado na mesma região geográfica que a conta dos Serviços de Multimédia.  

Este início rápido descreve os passos para criar uma nova conta dos Serviços de Multimédia do Azure através da CLI do Azure.  

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no [portal do Azure](http://portal.azure.com) e inicie o **CloudShell** para executar os comandos da CLI, conforme mostrado nos passos seguintes.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Se optar por instalar e usar a CLI localmente, este tópico requer a execução da versão 2.0 ou posterior da CLI do Azure. Execute `az --version` para localizar a versão atual. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure]( /cli/azure/install-azure-cli). 

## <a name="set-the-azure-subscription"></a>Definir a subscrição do Azure

No comando seguinte, forneça o ID da subscrição do Azure que quer utilizar na conta dos Serviços de Multimédia. Pode ver uma lista de subscrições a que tem acesso ao navegar até [Subscrições](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

```azurecli-interactive
az account set --subscription <mySubscriptionId>
```

## <a name="create-an-azure-resource-group"></a>Criar um Grupo de Recursos do Azure

O comando seguinte cria um grupo de recursos no qual pretende ter a conta de Armazenamento e dos Serviços de Multimédia. Substitua o marcador de posição *myresourcegroup* pelo nome que quer utilizar no grupo de recursos.

```azurecli-interactive
az group create -n <myresourcegroup> -l westus2
```

## <a name="create-an-azure-storage-account"></a>Criar uma conta de Armazenamento do Azure

Quando criar uma conta dos Serviços de Multimédia, terá de fornecer o ID de um recurso da conta de Armazenamento do Microsoft Azure. A conta de armazenamento especificada está ligada à sua conta dos Serviços de Multimédia. 

Tem de ter uma conta de armazenamento **Principal** e pode ter qualquer número de contas de armazenamento **Secundárias** associadas à conta dos Serviços de Multimédia. Os Serviços de Multimédia suportam contas para **fins gerais v2** ou **fins gerais v1**. As contas de armazenamento de blobs não são permitidas como **Principais**. Para obter mais informações sobre as contas de armazenamento, veja [Visão geral da conta de armazenamento do Azure](../../storage/common/storage-account-overview.md). 

O comando seguinte cria a conta de Armazenamento que vai ser associada à conta dos Serviços de Multimédia (principal). No script abaixo, substitua o marcador de posição *storageaccountforams*. “account_name” tem de ter menos de 24 carateres.

```azurecli-interactive
az storage account create -n <storageaccountforams> -g <myresourcegroup>
```

## <a name="create-an-azure-media-services-account"></a>Criar uma conta dos Azure Media Services

Veja a seguir os comandos da CLI do Azure que criam uma nova conta dos Serviços de Multimédia. Apenas terá de substituir os seguintes valores realçados:

* *myamsaccountname*
* *myresourcegroup*
* *storageaccountforams*

```azurecli-interactive
az ams create -n <myamsaccountname> -g <myresourcegroup> --storage-account <storageaccountforams>
```

## <a name="clean-up-resources"></a>Limpar recursos

Se já não precisar de nenhum dos recursos presentes no grupo de recursos, incluindo a conta dos Serviços de Multimédia que criou neste Início Rápido, elimine o grupo de recursos.

No **CloudShell**, execute o seguinte comando:

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Transmissão de um ficheiro](stream-files-dotnet-quickstart.md)
