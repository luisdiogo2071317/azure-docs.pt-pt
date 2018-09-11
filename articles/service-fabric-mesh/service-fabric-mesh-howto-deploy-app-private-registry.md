---
title: Implementar uma aplicação Mesh de recursos de infraestrutura do serviço de registo de imagem de contentor privado para o modo de malha do Azure Service Fabric | Documentos da Microsoft
description: Saiba como implementar uma aplicação a partir de um registo de imagem de contentor privado para a malha de recursos de infraestrutura de serviço com a CLI do Azure.
services: service-fabric-mesh
documentationcenter: .net
author: rwike77
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: azure-cli
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/20/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: a2791c86512790f36477e562cb82718174df8dc3
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/10/2018
ms.locfileid: "44296597"
---
# <a name="deploy-a-service-fabric-mesh-app-from-a-private-container-image-registry"></a>Implementar uma aplicação Mesh do Service Fabric a partir de um registo de imagem de contentor privado

Este artigo mostra como implementar uma aplicação de malha do Azure Service Fabric de mensagens em fila que utilize um registo de imagem de contentor privado.

## <a name="prerequisites"></a>Pré-requisitos

### <a name="set-up-service-fabric-mesh-cli"></a>Configurar a CLI do Service Fabric mesh

Utilize uma instalação local da CLI do Azure para concluir esta tarefa. Instale o módulo de extensão da CLI do Azure Service Fabric Mesh através destas [instruções](service-fabric-mesh-howto-setup-cli.md).

## <a name="install-docker"></a>Instalar o Docker

#### <a name="windows-10"></a>Windows 10

Transfira e instale a versão mais recente do [Docker Community Edition para Windows][download-docker] para suportar as aplicações do Service Fabric em contentores utilizadas pelo Service Fabric Mesh.

Durante a instalação, selecione **Utilizar contentores do Windows em vez de contentores do Linux** quando lhe for pedido. 

Se o Hyper-V não estiver ativado no seu computador, o instalador do Docker poderá ativá-lo. Clique em **OK** para fazer isso, se lhe for pedido.

#### <a name="windows-server-2016"></a>Windows Server 2016

Se não tiver a função do Hyper-V ativada, abra o PowerShell como administrador e execute o seguinte comando para ativar o Hyper-V e, em seguida, reinicie o computador. Para obter mais informações, veja [Docker Enterprise Edition para Windows Server][download-docker-server].

```powershell
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools
```

Reinicie o computador.

Abra o PowerShell como administrador e execute os seguintes comandos para instalar o Docker:

```powershell
Install-Module DockerMsftProvider -Force
Install-Package Docker -ProviderName DockerMsftProvider -Force
Install-WindowsFeature Containers
```

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no Azure e definir a subscrição ativa.

```azurecli-interactive
az login
az account set --subscription "<subscriptionName>"
```

## <a name="create-a-container-registry-and-push-an-image-to-it"></a>Criar um registo de contentor e enviar uma imagem ao mesmo

Utilize os seguintes passos para criar um Azure Container Registry.

### <a name="create-a-resource-group"></a>Criar um grupo de recursos

Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos. Utilize o seguinte comando para criar um grupo de recursos chamado *myResourceGroup* no *eastus* localização.

```azurecli
az group create --name myResourceGroup --location eastus
```

### <a name="create-a-container-registry"></a>Criar um registo de contentores

Criar um Azure container registry (ACR) instância com o `az acr create` comando. O nome do registo tem de ser exclusivo no Azure e pode incluir 5 a 50 carateres de alfanuméricos. No exemplo a seguir, o nome *myContainerRegistry007* é utilizado. Se obtiver um erro se o nome do registo está em utilização, escolha um nome diferente. Utilize esse nome em qualquer lugar `<acrName>` aparece nestas instruções.

```azurecli
az acr create --resource-group myResourceGroup --name myContainerRegistry007 --sku Basic
```

Quando o registo é criado, verá um resultado semelhante ao seguinte:

```json
{
  "adminUserEnabled": false,
  "creationDate": "2017-09-08T22:32:13.175925+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry007",
  "location": "eastus",
  "loginServer": "myContainerRegistry007.azurecr.io",
  "name": "myContainerRegistry007",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

### <a name="push-the-image-to-azure-container-registry"></a>Enviar a imagem para o Azure Container Registry

Para enviar uma imagem para um Azure container registry (ACR), primeiro tem de ter uma imagem de contentor. Se ainda não tiver quaisquer imagens de contentor local, execute o seguinte comando para solicitar a uma imagem do Docker Hub (poderá ter de mudar o Docker para trabalhar com imagens do Linux ao clicar com o botão direito no ícone do docker e ao selecionar **mudar para contentores do Linux**).

```bash
docker pull seabreeze/azure-mesh-helloworld:1.1-alpine
```

Antes de emitir uma imagem para o registo, tem de o etiquetar com o nome completamente qualificado do seu servidor de início de sessão ACR.

Execute o seguinte comando para obter o nome do servidor de início de sessão completa da sua instância do ACR.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

O nome do servidor de início de sessão completa que é retornado será referido como `<acrLoginServer>` em todo o resto deste artigo.

Agora Etiquetar a imagem do docker com o [etiqueta do docker] [ docker-tag] comando. No comando abaixo, substitua `<acrLoginServer>` com o nome do servidor de início de sessão comunicado pelo comando acima. O exemplo seguinte marcas seabreeze/azure-malha-helloworld:1.1-alpine imagem. Se estiver a utilizar uma imagem diferente, substitua o nome da imagem no comando seguinte.

```bash
docker tag seabreeze/azure-mesh-helloworld:1.1-alpine <acrLoginServer>/seabreeze/azure-mesh-helloworld:1.1-alpine
```

Por exemplo: `docker tag seabreeze/azure-mesh-helloworld:1.1-alpine myContainerRegistry007.azurecr.io/seabreeze/azure-mesh-helloworld:1.1-alpine`

Inicie sessão no Azure Container Registry.

```bash
az acr login -n <acrName>
```

Por exemplo: `az acr login -n myContainerRegistry007`

Envie a imagem para o azure container registry com o seguinte comando:

```bash
docker push <acrLoginServer>/seabreeze/azure-mesh-helloworld:1.1-alpine
```

Por exemplo: `docker push myContainerRegistry007.azurecr.io/seabreeze/azure-mesh-helloworld:1.1-alpine`

### <a name="list-container-images"></a>Listar imagens de contentor

O exemplo seguinte lista os repositórios num registo. Os exemplos que se seguem partem do princípio de que está a utilizar o azure-mesh-helloworld:1.1-alpine imagem. Se estiver a utilizar uma imagem diferente, substitua o respetivo nome em que a imagem azure-malha-helloworld é usada.

```azurecli
az acr repository list --name <acrName> --output table
```
Por exemplo: `az acr repository list --name myContainerRegistry007 --output table`

Saída:

```bash
Result
-------------------------------
seabreeze/azure-mesh-helloworld
```

O exemplo a seguir lista as etiquetas no **azure-malha-helloworld** repositório.

```azurecli
az acr repository show-tags --name <acrName> --repository seabreeze/azure-mesh-helloworld --output table
```

Por exemplo: `az acr repository show-tags --name myContainerRegistry007 --repository seabreeze/azure-mesh-helloworld --output table`

Saída:

```bash
Result
--------
1.1-alpine
```

Saída do anterior confirma a presença de `azure-mesh-helloworld:1.1-alpine` no registo de contentor privado.

## <a name="retrieve-credentials-for-the-registry"></a>Obter as credenciais para o registo

> [!IMPORTANT]
> Ativar o utilizador de administrador num registo de contentor do Azure não é recomendada para cenários de produção. Isso é feito aqui para manter esta demonstração em breve. Para cenários de produção, utilize um [principal de serviço](https://docs.microsoft.com/azure/container-registry/container-registry-auth-service-principal) para autenticação de utilizador e o sistema em cenários de produção.

Para poder implementar uma instância de contentor do registo que foi criado, tem de fornecer as credenciais durante a implantação. Permitir que o utilizador de administrador no seu registo com o seguinte comando:

```azurecli-interactive
az acr update --name <acrName> --admin-enabled true
```

Por exemplo: `az acr update --name myContainerRegistry007 --admin-enabled true`

Obtenha o nome do servidor de registo, nome de utilizador e palavra-passe, utilizando os seguintes comandos:

```azurecli-interactive
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
az acr credential show --name <acrName> --query username
az acr credential show --name <acrName> --query "passwords[0].value"
```

Os valores fornecidos, preceda os comandos são referenciados como `<acrLoginServer>`, `<acrUserName>`, e `<acrPassword>` abaixo.

## <a name="deploy-the-template"></a>Implementar o modelo

Crie a aplicação e os recursos relacionados com o seguinte comando e forneça as credenciais do passo anterior.

O `registry-password` parâmetro no modelo é uma cadeia segura. Não será apresentado o estado de implementação e `az mesh service show` comandos.

Se estiver a utilizar uma consola de Bash, execute o seguinte:

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/helloworld/mesh_rp.private_registry.linux.json --parameters "{\"location\": {\"value\": \"eastus\"}, \"registry-server\": {\"value\": \"<acrLoginServer>\"}, \"registry-username\": {\"value\": \"<acrUserName>\"}, \"registry-password\": {\"value\": \"<acrPassword>\"}}"
```

Se estiver a utilizar uma consola do PowerShell, execute o seguinte:

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/helloworld/mesh_rp.private_registry.linux.json --parameters "{'location': {'value': 'eastus'}, 'registry-server': {'value': '<acrLoginServer>'}, 'registry-username': {'value': '<acrUserName>'}, 'registry-password': {'value': '<acrPassword>'}}"
```

Dentro de alguns minutos, deverá ver:

`helloWorldPrivateRegistryApp has been deployed successfully on helloWorldPrivateRegistryNetwork with public ip address <IP Address>`

## <a name="open-the-application"></a>Abrir a aplicação

Assim que a aplicação é implementada com êxito, obtenha o endereço IP público para o ponto final de serviço e abra-o num browser. Apresenta uma página da web com o logótipo de malha do Service Fabric.

O comando de implementação devolve o endereço IP público do ponto de extremidade de serviço. Opcionalmente, também pode consultar o recurso de rede para encontrar o endereço IP público do ponto de extremidade de serviço. 
 
O nome de recurso de rede para esta aplicação está `helloWorldPrivateRegistryNetwork`, obter informações sobre ele usando o seguinte comando. 

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name helloWorldPrivateRegistryNetwork
```

## <a name="delete-the-resources"></a>Eliminar os recursos

Com frequência, elimine os recursos que já não estiver a utilizar no Azure. Para eliminar os recursos relacionados a este exemplo, elimine o grupo de recursos no qual eles foram implantados (o que elimina tudo associado o grupo de recursos) com o seguinte comando:

```azurecli-interactive
az group delete --resource-group myResourceGroup
```

Se mudado Docker para trabalhar com imagens do Linux para este exercício e tem de aceder ao trabalhar com imagens do Windows, com o botão direito no ícone do docker e selecione **mudar para contentores do Windows**

## <a name="next-steps"></a>Passos Seguintes

- Ver a aplicação de exemplo Hello World num [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/helloworld).
- Para saber mais sobre o modelo de recursos do Service Fabric, veja [modelo de recursos de malha do Service Fabric](service-fabric-mesh-service-fabric-resources.md).
- Para saber mais sobre a malha de recursos de infraestrutura do serviço, leia os [descrição geral do Service Fabric em malha](service-fabric-mesh-overview.md).

[download-docker-server]: https://docs.docker.com/install/windows/docker-ee/
[download-docker]: https://store.docker.com/editions/community/docker-ce-desktop-windows
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/