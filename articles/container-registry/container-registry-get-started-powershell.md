---
title: "Início Rápido - Criar um registo do Docker privado no Azure com o PowerShell"
description: Aprenda rapidamente a criar um registo do contentor do Docker com o PowerShell.
services: container-registry
author: neilpeterson
manager: timlt
ms.service: container-registry
ms.topic: quickstart
ms.date: 10/08/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c7d74395b1c8b386ce190906aa5b63b48c1bb1bf
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2018
---
# <a name="create-an-azure-container-registry-using-powershell"></a>Criar um Azure Container Registry utilizando o PowerShell

O Azure Container Registry é um serviço de registo do contentor Docker gerido, utilizado para armazenar imagens de contentor do Docker privadas. Este guia detalha a criação de uma instância do Azure Container Registry com o PowerShell.

Este início rápido requer a versão 3.6 ou posterior do módulo Azure PowerShell. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de instalar ou atualizar, veja [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps)(Instalar o módulo do Azure PowerShell).

Também tem de ter o Docker instalado localmente. O Docker disponibiliza pacotes que o configuram facilmente em qualquer sistema [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) ou [Linux](https://docs.docker.com/engine/installation/#supported-platforms).

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão na sua subscrição do Azure com o comando `Login-AzureRmAccount` e siga as instruções no ecrã.

```powershell
Login-AzureRmAccount
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

Em seguida, utilize o comando [docker login](https://docs.docker.com/engine/reference/commandline/login/) para iniciar sessão na instância ACR.

```bash
docker login $registry.LoginServer -u $creds.Username -p $creds.Password
```

O comando devolve uma mensagem “Início de sessão com êxito” depois de concluir.

## <a name="push-image-to-acr"></a>Enviar imagem para o ACR

Para enviar uma imagem para um registo do Azure Container, primeiro tem de ter uma imagem. Se for necessário, execute o seguinte comando para solicitar uma imagem previamente criada do Hub do Docker.

```bash
docker pull microsoft/aci-helloworld
```

A imagem tem de ser etiquetada com o nome de servidor de início de sessão ACR. Execute o comando [Get-AzureRmContainerRegistry](/powershell/module/containerregistry/Get-AzureRmContainerRegistry) para devolver o nome do servidor de início de sessão da instância ACR.

```powershell
Get-AzureRmContainerRegistry | Select Loginserver
```

Crie uma etiqueta para a imagem com o comando [etiqueta do docker](https://docs.docker.com/engine/reference/commandline/tag/). Substitua *acrLoginServer* pelo nome do servidor de início de sessão da sua instância do ACR.

```bash
docker tag microsoft/aci-helloworld <acrLoginServer>/aci-helloworld:v1
```

Por último, utilize [docker push](https://docs.docker.com/engine/reference/commandline/push/) para enviar imagens para a instância do ACR. Substitua *acrLoginServer* pelo nome do servidor de início de sessão da sua instância do ACR.

```bash
docker push <acrLoginServer>/aci-helloworld:v1
```

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, pode utilizar o comando [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para remover o grupo de recursos, a instância ACR e todas as imagens do contentor.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Passos seguintes

Neste guia de introdução, irá criar um Azure Container Registry a CLI do Azure. Se gostaria de utilizar o Azure Container Registry com o Azure Container Instances, continue para o tutorial do Azure Container Instances.

> [!div class="nextstepaction"]
> [Tutorial do Azure Container Instances](../container-instances/container-instances-tutorial-prepare-app.md)