---
title: "Guia de introdução - Criar o seu primeiro contentor do Azure Container Instances com o PowerShell"
description: "Comece a trabalhar com o Azure Container Instances ao criar uma instância de contentor do Windows com o PowerShell."
services: container-instances
documentationcenter: 
author: mmacy
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: ca10274fc6a23d7f5e7436dbaf72a6e7a918f275
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2017
---
# <a name="create-your-first-container-in-azure-container-instances"></a>Criar o seu primeiro contentor no Azure Container Instances

O Azure Container Instances facilita a criação e a gestão de contentores do Docker no Azure, sem ter de aprovisionar as máquinas virtuais ou adotar um serviço de nível mais elevado.

Neste guia de introdução, cria um contentor do Windows no Azure e expõe-no na Internet com um endereço IP público. Esta operação é concluída com um único comando. Dentro de alguns instantes, pode ver a aplicação em execução no seu browser:

![Aplicação implementada com o Azure Container Instances vista no browser][qs-powershell-01]

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este tutorial requer o módulo do Azure PowerShell versão 3.6 ou posterior. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure.

## <a name="create-resource-group"></a>Criar grupo de recursos

Crie um grupo de recursos do Azure com [New-AzureRmResourceGroup][New-AzureRmResourceGroup]. Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos.

 ```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-container"></a>Criar um contentor

Para criar um contentor, pode indicar um nome, uma imagem do Docker e um grupo de recursos do Azure ao cmdlet [New-AzureRmContainerGroup][New-AzureRmContainerGroup]. Opcionalmente, pode expor o contentor na Internet com um endereço IP público. Neste caso, iremos utilizar um contentor do Servidor Nano do Windows com os Serviços de Informação Internet (IIS).

 ```azurepowershell-interactive
New-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer -Image microsoft/iis:nanoserver -OsType Windows -IpAddressType Public
```

Dentro de alguns segundos, irá obter uma resposta ao seu pedido. Inicialmente, o contentor está no estado **A criar**, mas deverá ser iniciado dentro de um ou dois minutos. Pode verificar o estado com o cmdlet [Get-AzureRmContainerGroup][Get-AzureRmContainerGroup]:

 ```azurepowershell-interactive
Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

O estado de aprovisionamento e o endereço IP do contentor são apresentados no resultado do cmdlet:

```
ResourceGroupName        : myResourceGroup
Id                       : /subscriptions/12345678-1234-1234-1234-12345678abcd/resourceGroups/myResourceGroup/providers/Microsoft.ContainerInstance/containerGroups/mycontainer
Name                     : mycontainer
Type                     : Microsoft.ContainerInstance/containerGroups
Location                 : eastus
Tags                     :
ProvisioningState        : Creating
Containers               : {mycontainer}
ImageRegistryCredentials :
RestartPolicy            :
IpAddress                : 40.71.248.73
Ports                    : {80}
OsType                   : Windows
Volumes                  :
```

Quando o contentor **ProvisioningState** é movido para `Succeeded`, pode aceder ao mesmo no browser com o endereço IP fornecido.

![IIS implementado com o Azure Container Instances visto no browser][qs-powershell-01]

## <a name="delete-the-container"></a>Eliminar o contentor

Quando já não precisar do contentor, pode removê-lo com o cmdlet [Remove-AzureRmContainerGroup][Remove-AzureRmContainerGroup]:

 ```azurepowershell-interactive
Remove-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

## <a name="next-steps"></a>Passos seguintes

Neste guia de introdução iniciou um contentor do Windows pré-criado no Azure Container Instances. Se quiser tentar criar um contentor sozinho e implementá-lo no Azure Container Instances com o Azure Container Registry, avance para o tutorial do Azure Container Instances.

> [!div class="nextstepaction"]
> [Tutorial do Azure Container Instances](./container-instances-tutorial-prepare-app.md)

<!-- LINKS -->
[New-AzureRmResourceGroup]: /powershell/module/azurerm.resources/new-azurermresourcegroup
[New-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[Get-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/get-azurermcontainergroup
[Remove-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/remove-azurermcontainergroup

<!-- IMAGES -->
[qs-powershell-01]: ./media/container-instances-quickstart-powershell/qs-powershell-01.png