---
title: incluir ficheiro
description: incluir ficheiro
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 04/13/2018
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: 9ecb07a2cb278f6cde4ffdc3b252cb9e816d08da
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38733303"
---
## <a name="create-a-media-services-account"></a>Criar uma conta dos Media Services

Tem primeiro de criar uma conta dos Serviços de Multimédia. Esta secção apresenta aquilo de que necessita para criar a conta com a CLI do Azure.

### <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos ao utilizar o comando seguinte. Um grupo de recursos do Azure é um contentor lógico no qual os recursos, como as contas dos Serviços de Multimédia do Azure e as contas de Armazenamento associadas, são implementados e geridos.

```azurecli-interactive
az group create --name amsResourceGroup --location westus2
```

### <a name="create-a-storage-account"></a>Criar uma conta de armazenamento

Quando criar uma conta dos Serviços de Multimédia, terá de fornecer o nome de um recurso de conta de Armazenamento do Azure. A conta de armazenamento especificada está ligada à sua conta dos Serviços de Multimédia. 

Tem de ter uma conta de armazenamento **Principal** e pode ter qualquer número de contas de armazenamento **Secundárias** associadas à conta dos Serviços de Multimédia. Os Serviços de Multimédia suportam contas para **Fins gerais v2** (GPv2) ou **Fins gerais v1** (GPv1). As contas apenas de blobs não são permitidas como **Principais**. Se quiser saber mais sobre as contas de armazenamento, veja [Opções de conta de armazenamento do Azure](../articles/storage/common/storage-account-options.md). 

Para obter mais informações sobre a forma como as contas de armazenamento são utilizadas nos Serviços de Multimédia, veja [Storage accounts](../articles/media-services/latest/storage-account-concept.md) (Contas de armazenamento).

O comando seguinte cria uma conta de Armazenamento que será associada à conta dos Serviços de Multimédia. No script abaixo, pode substituir `storageaccountforams` pelo seu valor. O nome da conta tem de ter menos de 24 carateres.

```azurecli-interactive
az storage account create --name storageaccountforams \  
--kind StorageV2 \
--sku Standard_RAGRS \
--resource-group amsResourceGroup
```

### <a name="create-a-media-services-account"></a>Criar uma conta dos Media Services

O seguinte comando da CLI do Azure cria uma nova conta dos Serviços de Multimédia. Pode substituir os seguintes valores: `amsaccount`, `storageaccountforams` (têm de corresponder ao valor que forneceu para a sua conta de armazenamento) e `amsResourceGroup` (tem de corresponder ao valor que forneceu para o grupo de recursos).

```azurecli-interactive
az ams account create --name amsaccount --resource-group amsResourceGroup --storage-account storageaccountforams
```
