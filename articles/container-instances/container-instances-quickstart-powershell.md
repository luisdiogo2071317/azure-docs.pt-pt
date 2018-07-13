---
title: Guia de introdução - Criar o seu primeiro contentor do Azure Container Instances com o PowerShell
description: Neste manual de início rápido, vai utilizar a Azure PowerShell para implementar um contentor Windows nas Azure Container Instances
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: quickstart
ms.date: 05/11/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 4a1d338304dbd5e2845768b7bf0273eed23af0ec
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38453571"
---
# <a name="quickstart-create-your-first-container-in-azure-container-instances"></a>Início Rápido: crie o seu primeiro contentor no Azure Container Instances

O Azure Container Instances facilita a criação e a gestão de contentores do Docker no Azure, sem ter de aprovisionar as máquinas virtuais ou adotar um serviço de nível mais elevado. Neste manual de início rápido, vai criar um contentor do Windows e colocá-lo na Internet com nome de domínio completamente qualificado (FQDN). Esta operação é concluída com um único comando. Dentro de alguns instantes, pode ver a aplicação em execução no seu browser:

![Aplicação implementada com o Azure Container Instances vista no browser][qs-powershell-01]

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/) antes de começar.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este tutorial requer a versão 5.5 ou posterior do módulo Azure PowerShell. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Connect-AzureRmAccount` para criar uma ligação com o Azure.

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos do Azure com [New-AzureRmResourceGroup][New-AzureRmResourceGroup]. Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos.

 ```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-container"></a>Criar um contentor

Para criar um contentor, pode indicar um nome, uma imagem do Docker e um grupo de recursos do Azure ao cmdlet [New-AzureRmContainerGroup][New-AzureRmContainerGroup]. Como opção, pode expor o contentor na Internet com uma etiqueta de nome DNS.

Execute o seguinte comando para iniciar um contentor Nano Server que execute os Serviços de Informação Internet (IIS). O valor `-DnsNameLabel` tem de ser exclusivo dentro da região do Azure na qual cria a instância, pelo que poderá precisar de modificar este valor para garantir a exclusividade.

 ```azurepowershell-interactive
New-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer -Image microsoft/iis:nanoserver -OsType Windows -DnsNameLabel aci-demo-win
```

Dentro de alguns segundos, deve receber uma resposta ao seu pedido. O contentor está inicialmente no estado **A criar**, mas deverá ser iniciado dentro de um ou dois minutos. Pode verificar o estado de implementação com o cmdlet [Get-AzureRmContainerGroup][Get-AzureRmContainerGroup]:

 ```azurepowershell-interactive
Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

O estado de aprovisionamento do contentor, o nome de domínio completamente qualificado (FQDN) e o endereço IP aparecem na saída do cmdlet:

```console
PS Azure:\> Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer


ResourceGroupName        : myResourceGroup
Id                       : /subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerInstance/containerGroups/mycontainer
Name                     : mycontainer
Type                     : Microsoft.ContainerInstance/containerGroups
Location                 : eastus
Tags                     :
ProvisioningState        : Creating
Containers               : {mycontainer}
ImageRegistryCredentials :
RestartPolicy            : Always
IpAddress                : 52.226.19.87
DnsNameLabel             : aci-demo-win
Fqdn                     : aci-demo-win.eastus.azurecontainer.io
Ports                    : {80}
OsType                   : Windows
Volumes                  :
State                    : Pending
Events                   : {}
```

Depois do contentor **ProvisioningState** ser movido para `Succeeded`, navegue para o respetivo `Fqdn` no browser:

![IIS implementado com o Azure Container Instances visto no browser][qs-powershell-01]

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não precisar do contentor, pode removê-lo com o cmdlet [Remove-AzureRmContainerGroup][Remove-AzureRmContainerGroup]:

 ```azurepowershell-interactive
Remove-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

## <a name="next-steps"></a>Passos seguintes

Neste início rápido, criou uma instância de contentor do Azure a partir de uma imagem no registo do Hub do Docker público. Se quiser criar uma imagem de contentor sozinho e implementá-lo no Azure Container Instances a partir de um registo de contentor do Azure, avance para o tutorial do Azure Container Instances.

> [!div class="nextstepaction"]
> [Tutorial do Azure Container Instances](./container-instances-tutorial-prepare-app.md)

<!-- IMAGES -->
[qs-powershell-01]: ./media/container-instances-quickstart-powershell/qs-powershell-01.png

<!-- LINKS -->
[New-AzureRmResourceGroup]: /powershell/module/azurerm.resources/new-azurermresourcegroup
[New-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[Get-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/get-azurermcontainergroup
[Remove-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/remove-azurermcontainergroup
