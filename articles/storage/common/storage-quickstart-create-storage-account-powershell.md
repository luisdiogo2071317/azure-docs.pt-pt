---
title: "Início Rápido do Azure – Criar uma conta de armazenamento com o PowerShell | Microsoft Docs"
description: Aprenda a criar rapidamente uma nova conta de armazenamento com o PowerShell
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 06/29/2017
ms.author: tamram
ms.openlocfilehash: b4b917adfb3644cca71b6696df005fbf9e295240
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/29/2017
---
# <a name="create-a-storage-account-using-powershell"></a>Criar uma conta de armazenamento com o PowerShell

O módulo do Azure PowerShell é utilizado para criar e gerir recursos do Azure a partir da linha de comandos do PowerShell ou em scripts. Este guia fornece detalhes sobre a utilização do PowerShell para criar uma conta de Armazenamento do Azure. 

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

Este guia de introdução requer a versão do módulo 3.6 ou posterior do Azure PowerShell. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de instalar ou atualizar, veja [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps)(Instalar o módulo do Azure PowerShell).

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão na sua subscrição do Azure com o comando `Login-AzureRmAccount` e siga as instruções no ecrã.

```powershell
Login-AzureRmAccount
```

Se não souber qual a localização que quer utilizar, pode listar as localizações disponíveis. Depois de a lista ser apresentada, localize a que quer utilizar. Este exemplo utilizará **eastus**. Armazene-a numa variável e utilize-a de forma a poder alterá-la num único local.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-resource-group"></a>Criar grupo de recursos

Crie um grupo de recursos do Azure com [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos. 

```powershell
# put resource group in a variable so you can use the same group name going forward
#   without hardcoding it repeatedly
$resourceGroup = "contoso-storage-accounts"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location 
```

## <a name="create-a-general-purpose-standard-storage-account"></a>Criar uma conta de armazenamento padrão para fins gerais

Existem vários tipos de contas de armazenamento, consoante o modo de utilização e para que serviço (blobs, ficheiros, tabelas ou filas). Esta tabela mostra as possibilidades.

|**Tipo de conta de armazenamento**|**Standard para fins gerais**|**Premium para fins gerais**|**Armazenamento de blobs, camadas de acesso frequente e esporádico**|
|-----|-----|-----|-----|
|**Serviços suportados**| Serviços Blob, Ficheiro, Tabela e Fila | Serviço Blob | Serviço Blob|
|**Tipos de blobs suportados**|Blobs de blocos, blobs de páginas e blobs de acréscimo | Blobs de páginas | Blobs de blocos e blobs de acréscimo|

Utilize [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount) para criar uma conta de armazenamento padrão para fins gerais que possa utilizar para os quatro serviços. Atribua o nome *contosomvcstandard* à conta de armazenamento e configure-a para ter o Armazenamento Localmente Redundante e a encriptação de blobs ativados.

```powershell
New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name "contosomvcstandard" `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage `
  -EnableEncryptionService Blob
```

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, pode utilizar o comando [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para remover o grupo de recursos e todos os recursos relacionados. Neste caso, remove a conta de armazenamento que criou.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Passos seguintes

Neste início rápido, criou uma conta de armazenamento padrão para fins gerais. Para saber como carregar e transferir blobs com a sua conta de armazenamento, avance para o início rápido de armazenamento de Blobs.
> [!div class="nextstepaction"]
> [Transferir objetos de/para o armazenamento de Blobs do Azure com o PowerShell](../blobs/storage-quickstart-blobs-powershell.md)