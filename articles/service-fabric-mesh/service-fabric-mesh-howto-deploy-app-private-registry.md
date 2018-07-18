---
title: Implementar a aplicação de um registo privado para o modo de malha do Azure Service Fabric | Documentos da Microsoft
description: Saiba como implementar uma aplicação que utiliza o registo de contentor privado para a malha de recursos de infraestrutura de serviço com a CLI do Azure.
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
ms.date: 07/16/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: af92d3c6ea881d00ec687a5560bf4db35aa431c5
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39089491"
---
# <a name="deploy-a-service-fabric-mesh-app-from-a-private-container-image-registry"></a>Implementar uma aplicação Mesh do Service Fabric a partir de um registo de imagem de contentor privado

Este artigo mostra como implementar uma aplicação de malha do Azure Service Fabric de mensagens em fila que utilize um registo de imagem de contentor privado.

## <a name="prerequisites"></a>Pré-requisitos

### <a name="set-up-service-fabric-mesh-cli"></a>Configurar a CLI do Service Fabric de malha 
Pode utilizar o Azure Cloud Shell ou uma instalação local da CLI do Azure para concluir esta tarefa. Instalar o módulo de extensão de CLI de malha do Azure Service Fabric através destas [instruções](service-fabric-mesh-howto-setup-cli.md).

### <a name="install-docker"></a>Instalar Docker

Instale o Docker para suportar as aplicações do Service Fabric em contentores utilizadas por recursos de infraestrutura de serviço de malha.

### <a name="windows-10"></a>Windows 10

Transfira e instale a versão mais recente do [Docker Community Edition para Windows][download-docker]. 

Durante a instalação, selecione **contentores do Windows de utilização, em vez de contentores do Linux** quando lhe for pedido. Precisará, em seguida, termine sessão e inicie sessão novamente. Depois de voltar a iniciar sessão, se não tiver ativado anteriormente Hyper-V, pode ser solicitado para ativar o Hyper-V. Ativar o Hyper-V e, em seguida, reinicie o computador.

Depois de reiniciar o computador, Docker irá solicitar-lhe para ativar a **contentores** de recursos, ativá-la e reiniciar o computador.

### <a name="windows-server-2016"></a>Windows Server 2016

Utilize os seguintes comandos do PowerShell para instalar o Docker. Para obter mais informações, consulte [Docker Enterprise Edition para o Windows Server][download-docker-server].

```powershell
Install-Module DockerMsftProvider -Force
Install-Package Docker -ProviderName DockerMsftProvider -Force
Install-WindowsFeature Containers
```

Reinicie o computador.

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no Azure e definir a subscrição ativa:

```azurecli-interactive
az login
az account set --subscription "<subscriptionName>"
```

## <a name="create-a-container-registry-and-push-an-image-to-it"></a>Criar um registo de contentor e enviar uma imagem ao mesmo

Criar um Azure Container Registry, seguindo as instruções em [criar um registo privado do Docker no Azure com a CLI do Azure](../container-registry/container-registry-get-started-azure-cli.md). Execute os passos até a [iniciar sessão no ACR](../container-registry/container-registry-get-started-azure-cli.md#log-in-to-acr) passo. 

### <a name="push-image-to-acr"></a>Enviar imagem para o ACR

Para enviar uma imagem para um registo do Azure Container, primeiro tem de ter uma imagem. Se ainda não tiver quaisquer imagens do contentor local, execute o seguinte comando para solicitar uma imagem existente a partir do Hub do Docker.

```bash
docker pull seabreeze/azure-mesh-helloworld:1.1-alpine
```

Antes de emitir uma imagem para o registo, tem de o etiquetar com o nome completamente qualificado do seu servidor de início de sessão ACR. Execute o seguinte comando para obter o nome do servidor de início de sessão completa da instância ACR.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Crie uma etiqueta para a imagem com o comando [etiqueta do docker][docker-tag]. Substitua `<acrLoginServer>` pelo nome do servidor de início de sessão da sua instância do ACR.

```bash
docker tag seabreeze/azure-mesh-helloworld:1.1-alpine <acrLoginServer>/azure-mesh-helloworld:1.1-alpine
```
### <a name="list-container-images"></a>Listar imagens de contentor

O exemplo seguinte lista os repositórios de um registo:

```azurecli
az acr repository list --name <acrName> --output table
```

Saída:

```bash
Result
----------------
azure-mesh-helloworld
```

O exemplo a seguir lista as etiquetas no **azure-malha-helloworld** repositório.

```azurecli
az acr repository show-tags --name <acrName> --repository azure-mesh-helloworld --output table
```

Saída:

```bash
Result
--------
1.1-alpine
```
Saída do anterior confirma a presença de `azure-mesh-helloworld:1.1-alpine` no registo de contentor privado.

## <a name="retrieve-credentials-for-the-registry"></a>Obter as credenciais para o registo

Para poder implementar uma instância de contentor do registo que foi criado, tem de fornecer as credenciais durante a implantação. Permitir que o utilizador de administrador no seu registo com o seguinte comando:

```azurecli-interactive
az acr update --name <acrName> --admin-enabled true
```

Obtenha o nome do servidor de registo, nome de utilizador e palavra-passe, utilizando os seguintes comandos:

```azurecli-interactive
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
az acr credential show --name <acrName> --query username
az acr credential show --name <acrName> --query "passwords[0].value"
```

Os valores fornecidos, preceda os comandos são referenciados como `<acrLoginServer>`, `<acrUserName>`, e `<acrPassword>` no comando seguinte.


## <a name="deploy-the-template"></a>Implementar o modelo

Crie a aplicação e os recursos relacionados com o seguinte comando e forneça as credenciais do passo anterior.

O `registry-password` parâmetro no modelo é um `securestring`. Não será apresentado o estado de implementação e `az mesh service show` comandos. Certifique-se de que ele está corretamente especificado no comando seguinte.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/helloworld/mesh_rp.private_registry.linux.json --parameters "{\"location\": {\"value\": \"eastus\"}, \"registry-server\": {\"value\": \"<acrLoginServer>\"}, \"registry-username\": {\"value\": \"<acrUserName>\"}, \"registry-password\": {\"value\": \"<acrPassword>\"}}" 
```

Dentro de alguns minutos, o comando deverá devolver com:

`helloWorldPrivateRegistryApp has been deployed successfully on helloWorldPrivateRegistryNetwork with public ip address <IP Address>` 

## <a name="open-the-application"></a>Abra a aplicação
Assim que a aplicação é implementada com êxito, obtenha o endereço IP público para o ponto final de serviço e abra-o num browser. Apresenta uma página da web com o logótipo de malha do Service Fabric.

O comando de implementação devolve o endereço IP público do ponto de extremidade de serviço. Opcionalmente, também pode consultar o recurso de rede para encontrar o endereço IP público do ponto de extremidade de serviço. 
 
O nome de recurso de rede para esta aplicação está `helloWorldPrivateRegistryNetwork`, obter informações sobre ele usando o seguinte comando. 

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name helloWorldPrivateRegistryNetwork
```

## <a name="delete-the-resources"></a>Eliminar os recursos

De modo a preservar recursos limitados, atribuídos para o programa de pré-visualização, elimine os recursos com frequência. Para eliminar os recursos relacionados a este exemplo, elimine o grupo de recursos no qual eles foram implantados.

```azurecli-interactive
az group delete --resource-group myResourceGroup 
```

## <a name="next-steps"></a>Passos Seguintes
- Ver a aplicação de exemplo Hello World num [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/helloworld).
- Para saber mais sobre o modelo de recursos do Service Fabric, veja [modelo de recursos de malha do Service Fabric](service-fabric-mesh-service-fabric-resources.md).
- Para saber mais sobre a malha de recursos de infraestrutura do serviço, leia os [descrição geral do Service Fabric em malha](service-fabric-mesh-overview.md).

[download-docker-server]: https://docs.docker.com/install/windows/docker-ee/
[download-docker]: https://store.docker.com/editions/community/docker-ce-desktop-windows
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/