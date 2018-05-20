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
ms.openlocfilehash: 3157f5db58be5735fa7b194393b0ab6d0e143a91
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
## <a name="create-a-media-services-account"></a>Criar uma conta dos Media Services

Tem primeiro de criar uma conta dos Serviços de Multimédia. Esta secção apresenta aquilo de que necessita para criar a conta com a CLI 2.0.

### <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos ao utilizar o comando seguinte. Um grupo de recursos do Azure é um contentor lógico no qual os recursos, como as contas dos Serviços de Multimédia do Azure e as contas de Armazenamento associadas, são implementados e geridos.

```azurecli-interactive
az group create --name amsResourceGroup --location westus2
```

### <a name="create-a-storage-account"></a>Criar uma conta de armazenamento

Quando criar uma conta dos Serviços de Multimédia, terá de fornecer o nome de um recurso de conta de Armazenamento do Azure. A conta de armazenamento especificada está ligada à sua conta dos Serviços de Multimédia. 

Tem de ter uma conta de armazenamento **Principal** e pode ter qualquer número de contas de armazenamento **Secundário** associadas à conta dos Serviços de Multimédia. Os Serviços de Multimédia suportam contas **v2 para fins gerais** (GPv2) ou **v1 para fins gerais**(GPv1). As contas de blob únicas não são permitidas como **Principal**. Se quiser saber mais sobre as contas de armazenamento, veja [Opções de conta de armazenamento do Azure](../articles/storage/common/storage-account-options.md). 

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
