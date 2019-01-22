---
title: Início rápido - executar uma aplicação no Azure Container Instances - PowerShell
description: Neste início rápido, vai utilizar do Azure PowerShell para implementar uma aplicação de contentor do Docker no Azure Container Instances com o Azure PowerShell
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: quickstart
ms.date: 10/02/2018
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: 87cf0f09cbeeee90492e070b25b713546c6631e5
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2019
ms.locfileid: "54438869"
---
# <a name="quickstart-run-a-container-application-in-azure-container-instances-with-azure-powershell"></a>Início rápido: Execute uma aplicação de contentor no Azure Container Instances com o Azure PowerShell

Utilize o Azure Container Instances para executar contentores de Docker no Azure com simplicidade e celeridade. Não necessita de implementar máquinas virtuais nem de utilizar uma plataforma de orquestração de contentores completa, como o Kubernetes. Neste início rápido, vai utilizar o portal do Azure para criar um contentor do Windows no Azure e disponibilizar a sua aplicação na Internet com um nome de domínio completamente qualificado (FQDN). Alguns segundos depois de executar um comando de implementação única, pode navegar para a aplicação em execução:

![Aplicação implementada com o Azure Container Instances vista no browser][qs-powershell-01]

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/) antes de começar.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este tutorial requer a versão 5.5 ou posterior do módulo Azure PowerShell. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/azurerm/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Connect-AzureRmAccount` para criar uma ligação com o Azure.

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

O Azure Container Instances, como todos os recursos do Azure, tem de ser implementados num grupo de recursos. Os grupos de recursos permitem organizar e gerir recursos relacionados do Azure.

Primeiro, crie um grupo de recursos denominado *myResourceGroup* na localização *eualeste* através do comando seguinte [New-AzureRmResourceGroup][New-AzureRmResourceGroup]:

 ```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-container"></a>Criar um contentor

Agora que tem um grupo de recursos, pode executar um contentor no Azure. Para criar uma instância de contentor com o Azure PowerShell, forneça um nome de um grupo de recursos, o nome da instância de contentor e a imagem do contentor de Docker para o cmdlet [New-AzureRmContainerGroup] [ New-AzureRmContainerGroup]. Pode expor os seus contentores à Internet, especificando uma ou mais portas a abrir, uma etiqueta de nome DNS ou ambos. Neste início rápido, vai implementar um contentor com uma etiqueta de nome DNS que aloja os serviços de informação Internet (IIS) em execução no servidor Nano.

Execute o seguinte comando para iniciar a instância de um contentor. O valor `-DnsNameLabel` tem de ser exclusivo na região do Azure em que cria a instância. Se receber uma mensagem de erro "A etiqueta de nome DNS não está disponível ", experimente uma etiqueta de nome DNS diferente.

 ```azurepowershell-interactive
New-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer -Image microsoft/iis:nanoserver -OsType Windows -DnsNameLabel aci-demo-win
```

Alguns segundos depois, deverá receber uma resposta do Azure. O estado do contentor `ProvisioningState` inicialmente é **A criar**, mas deve passar para um **Com êxito** ao fim de um ou dois minutos. Verifique o estado de implementação com o cmdlet [Get-AzureRmContainerGroup][Get-AzureRmContainerGroup]:

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

Quando o estado do contentor `ProvisioningState` passar a **Com êxito**, navegue para o `Fqdn` no seu browser. Se lhe for apresentada uma página Web semelhante à seguinte, parabéns! Implementou com êxito uma aplicação em execução num contentor de Docker para Azure.

![IIS implementado com o Azure Container Instances visto no browser][qs-powershell-01]

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não precisar do contentor, pode removê-lo com o cmdlet [Remove-AzureRmContainerGroup][Remove-AzureRmContainerGroup]:

 ```azurepowershell-interactive
Remove-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

## <a name="next-steps"></a>Passos Seguintes

Neste início rápido, criou uma instância de contentor do Azure a partir de uma imagem no registo do Hub do Docker público. Se deseja criar uma imagem do contentor e implementá-la partir de um registo de contentor privado do Azure, prossiga para o tutorial do Azure Container Instances.

> [!div class="nextstepaction"]
> [Tutorial do Azure Container Instances](./container-instances-tutorial-prepare-app.md)

<!-- IMAGES -->
[qs-powershell-01]: ./media/container-instances-quickstart-powershell/qs-powershell-01.png

<!-- LINKS -->
[New-AzureRmResourceGroup]: /powershell/module/azurerm.resources/new-azurermresourcegroup
[New-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[Get-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/get-azurermcontainergroup
[Remove-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/remove-azurermcontainergroup
