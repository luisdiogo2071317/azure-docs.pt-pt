---
title: Início Rápido - Criar um registo do Docker privado no Azure com o PowerShell
description: Aprenda rapidamente a criar um registo do contentor do Docker com o PowerShell.
services: container-registry
author: neilpeterson
manager: jeconnoc
ms.service: container-registry
ms.topic: quickstart
ms.date: 03/03/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 3097696d85c738730e725ede84437d0e36ec6bc4
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-create-an-azure-container-registry-using-powershell"></a>Início Rápido: Criar um Azure Container Registry com o PowerShell

O Azure Container Registry é um serviço de registo do contentor Docker gerido, utilizado para armazenar imagens de contentor do Docker privadas. Este guia detalha a criação de uma instância do Azure Container Registry através do PowerShell, o envio de uma imagem de contentor para o registo e, por fim, a implementação do contentor a partir do registo no Azure Container Instances (ACI).

Este início rápido requer a versão 3.6 ou posterior do módulo Azure PowerShell. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de instalar ou atualizar, veja [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps)(Instalar o módulo do Azure PowerShell).

Também tem de ter o Docker instalado localmente. O Docker disponibiliza pacotes que o configuram facilmente em qualquer sistema [Mac][docker-mac], [Windows][docker-windows] ou [Linux][docker-linux].

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão na sua subscrição do Azure com o comando `Connect-AzureRmAccount` e siga as instruções no ecrã.

```powershell
Connect-AzureRmAccount
```

## <a name="create-resource-group"></a>Criar grupo de recursos

Crie um grupo de recursos do Azure com [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos.

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-container-registry"></a>Criar um registo de contentores

Criar uma instância ACR utilizando o comando [New-AzureRMContainerRegistry](/powershell/module/containerregistry/New-AzureRMContainerRegistry).

O nome do registo tem de ser exclusivo no Azure e pode incluir de 5 a 50 carateres alfanuméricos. No exemplo seguinte, é utilizado *myContainerRegistry007*. Atualize para um valor exclusivo.

```powershell
$registry = New-AzureRMContainerRegistry -ResourceGroupName "myResourceGroup" -Name "myContainerRegistry007" -EnableAdminUser -Sku Basic
```

## <a name="log-in-to-acr"></a>Iniciar sessão no ACR

Antes de emitir e solicitar imagens de contentor, tem de iniciar sessão na instância do ACR. Em primeiro lugar, utilize o comando [Get-AzureRmContainerRegistryCredential](/powershell/module/containerregistry/get-azurermcontainerregistrycredential) para obter as credenciais de administrador para a instância ACR.

```powershell
$creds = Get-AzureRmContainerRegistryCredential -Registry $registry
```

Em seguida, utilize o comando [docker login][docker-login] para iniciar sessão na instância ACR.

```powershell
docker login $registry.LoginServer -u $creds.Username -p $creds.Password
```

O comando devolve `Login Succeeded` depois de estar concluído. Também poderá ver um aviso de segurança que recomenda a utilização do parâmetro `--password-stdin`. Enquanto a sua utilização está fora do âmbito deste artigo, recomendamos que siga esta melhor prática. Veja a referência do comando [início de sessão do docker][docker-login] para obter mais informações.

## <a name="push-image-to-acr"></a>Enviar imagem para o ACR

Para enviar uma imagem para um registo do Azure Container, primeiro tem de ter uma imagem. Se for necessário, execute o seguinte comando para solicitar uma imagem previamente criada do Hub do Docker.

```powershell
docker pull microsoft/aci-helloworld
```

A imagem tem de ser etiquetada com o nome de servidor de início de sessão ACR. Utilize o comando [docker tag][docker-tag] para efetuar este procedimento.

```powershell
$image = $registry.LoginServer + "/aci-helloworld:v1"
docker tag microsoft/aci-helloworld $image
```

Por último, utilize [docker push][docker-push] para enviar a imagem para o ACR.

```powershell
docker push $image
```

## <a name="deploy-image-to-aci"></a>Implementar a imagem no ACI
Para implementar a imagem como uma instância de contentor no Azure Container Instances (ACI), converta primeiro a credencial de registo numa PSCredential.

```powershell
$secpasswd = ConvertTo-SecureString $creds.Password -AsPlainText -Force
$pscred = New-Object System.Management.Automation.PSCredential($creds.Username, $secpasswd)
```

Para implementar a imagem de contentor do registo de contentor com 1 núcleo de CPU e 1 GB de memória, execute o seguinte comando:

```powershell
New-AzureRmContainerGroup -ResourceGroup myResourceGroup -Name mycontainer -Image $image -Cpu 1 -MemoryInGB 1 -IpAddressType public -Port 80 -RegistryCredential $pscred
```

Deve obter uma resposta inicial do Azure Resource Manager com os detalhes sobre o contentor. Para monitorizar o estado do contentor e verificar quando está em execução, repita o comando [Get-AzureRmContainerGroup][Get-AzureRmContainerGroup]. Deve demorar menos de um minuto.

```powershell
(Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer).ProvisioningState
```

Exemplo de saída: `Succeeded`

## <a name="view-the-application"></a>Ver a aplicação
Após a implementação com êxito para o ACI, obtenha o endereço IP público do contentor com o comando [Get-AzureRmContainerGroup][Get-AzureRmContainerGroup]:

```powershell
(Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer).IpAddress
```

Exemplo de saída: `"13.72.74.222"`

Para ver a aplicação em execução, navegue até ao endereço IP público no seu browser favorito. Deverá ter um aspeto semelhante a:

![Aplicação Hello World no browser][qs-portal-15]

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, pode utilizar o comando [Remove-AzureRmResourceGroup][Remove-AzureRmResourceGroup] para remover o grupo de recursos, o Azure Container Registry e todos os Azure Container Instances.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Passos seguintes

Neste início rápido, criou um Azure Container Registry com a CLI do Azure e iniciou uma instância do mesmo no Azure Container Instances. Avance para o tutorial do Azure Container Instances para ver mais detalhadamente o ACI.

> [!div class="nextstepaction"]
> [Tutorial do Azure Container Instances](../container-instances/container-instances-tutorial-prepare-app.md)

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- Links - internal -->
[Get-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/get-azurermcontainergroup
[Remove-AzureRmResourceGroup]: /powershell/module/azurerm.resources/remove-azurermresourcegroup

<!-- IMAGES> -->
[qs-portal-15]: ./media/container-registry-get-started-portal/qs-portal-15.png
