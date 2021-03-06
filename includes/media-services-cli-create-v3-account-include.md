---
title: incluir ficheiro
description: incluir ficheiro
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 11/11/2018
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: 4207031652db7ec4b2ae5468dc159320f6efdbc2
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228819"
---
## <a name="create-a-media-services-account"></a>Criar uma conta dos Media Services

Tem primeiro de criar uma conta dos Serviços de Multimédia. Esta secção mostra o que precisa para a criação de conta com a CLI do Azure.

### <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos ao utilizar o comando seguinte. Um grupo de recursos do Azure é um contentor lógico no qual os recursos, como as contas dos Serviços de Multimédia do Azure e as contas de Armazenamento associadas, são implementados e geridos.

```azurecli
az group create --name amsResourceGroup --location westus2
```

### <a name="create-a-storage-account"></a>Criar uma conta de armazenamento

Quando criar uma conta dos Serviços de Multimédia, terá de fornecer o nome de um recurso de conta de Armazenamento do Azure. A conta de armazenamento especificada está ligada à sua conta dos Serviços de Multimédia. 

Tem de ter uma conta de armazenamento **Principal** e pode ter qualquer número de contas de armazenamento **Secundárias** associadas à conta dos Serviços de Multimédia. Os Serviços de Multimédia suportam contas para **Fins gerais v2** (GPv2) ou **Fins gerais v1** (GPv1). As contas apenas de blobs não são permitidas como **Principais**. Se quiser saber mais sobre as contas de armazenamento, veja [Opções de conta de armazenamento do Azure](../articles/storage/common/storage-account-options.md). 

Para obter mais informações sobre a forma como as contas de armazenamento são utilizadas nos Serviços de Multimédia, veja [Storage accounts](../articles/media-services/latest/storage-account-concept.md) (Contas de armazenamento).

O comando seguinte cria uma conta de Armazenamento que será associada à conta dos Serviços de Multimédia. No script abaixo, pode substituir `storageaccountforams` pelo seu valor. O nome da conta tem de ter menos de 24 carateres.

```azurecli
az storage account create --name storageaccountforams \  
--kind StorageV2 \
--sku Standard_RAGRS \
--resource-group amsResourceGroup
```

### <a name="create-a-media-services-account"></a>Criar uma conta dos Media Services

O seguinte comando da CLI do Azure cria uma nova conta dos Serviços de Multimédia. Pode substituir os seguintes valores: `amsaccount`, `storageaccountforams` (têm de corresponder ao valor que forneceu para a sua conta de armazenamento) e `amsResourceGroup` (tem de corresponder ao valor que forneceu para o grupo de recursos).

```azurecli
az ams account create --name amsaccount --resource-group amsResourceGroup --storage-account storageaccountforams
```
