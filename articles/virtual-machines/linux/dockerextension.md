---
title: Utilizar a extensão da VM do Azure Docker | Microsoft Docs
description: Saiba como utilizar a extensão de VM de Docker rapidamente e segura implementar num ambiente de Docker no Azure utilizando modelos do Resource Manager e o 2.0 CLI do Azure
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
ms.assetid: 936d67d7-6921-4275-bf11-1e0115e66b7f
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/18/2017
ms.author: iainfou
ms.openlocfilehash: 6cf77a6fa5e2cb7f9ce349e72444e76d4c687f49
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36937657"
---
# <a name="create-a-docker-environment-in-azure-using-the-docker-vm-extension"></a>Criar um ambiente de Docker no Azure através da extensão de VM de Docker
Docker é uma gestão de contentores populares e a plataforma de processamento de imagens que permite-lhe trabalhar rapidamente com contentores no Linux. No Azure, existem várias formas como pode implementar Docker consoante as suas necessidades. Este artigo foca-se utilizando a extensão de VM de Docker e modelos Azure Resource Manager com o 2.0 CLI do Azure. 

> [!WARNING]
> A extensão de VM de Docker do Azure para Linux foi preterida e será descontinuada Novembro de 2018.
> A extensão instala simplesmente Docker, para que alternativas como nuvem init ou a extensão de Script personalizado são uma melhor forma de instalar a versão de Docker à escolha. Para obter mais informações sobre como utilizar init de nuvem, consulte [personalizar uma VM com Linux com a nuvem init](tutorial-automate-vm-deployment.md).

## <a name="azure-docker-vm-extension-overview"></a>Descrição geral de extensão de VM de Docker do Azure
A extensão da VM do Azure Docker instala e configura o daemon de Docker, o cliente de Docker e o Docker Compose na sua máquina virtual (VM) do Linux. Ao utilizar a extensão de VM de Docker do Azure, terá de mais controlo e funcionalidades que simplesmente a utilização do computador de Docker ou criar o anfitrião de Docker por si. Adicionais estas funcionalidades, tais como [Docker Compose](https://docs.docker.com/compose/overview/), marca a extensão da VM do Azure Docker adequada para ambientes mais robustos de programador ou de produção.

Para obter mais informações sobre os métodos de implementação diferentes, incluindo a utilização de Docker máquina e serviços de contentor do Azure, consulte os artigos seguintes:

* Para criar rapidamente protótipos uma aplicação, pode criar um único anfitrião Docker utilizando [Docker máquina](docker-machine.md).
* Para criar ambientes prontos para produção, dimensionáveis, que fornecem ferramentas adicionais de agendamento e de gestão, pode implementar um [Kubernetes](../../container-service/kubernetes/index.yml) ou [Docker Swarm](../../container-service/dcos-swarm/index.yml) cluster nos serviços de contentor do Azure.


## <a name="deploy-a-template-with-the-azure-docker-vm-extension"></a>Implementar um modelo com a extensão de VM de Docker do Azure
Vamos utilizar um modelo existente de início rápido para criar uma VM com Ubuntu que utiliza a extensão de VM de Docker do Azure para instalar e configurar o anfitrião de Docker. Pode ver o modelo aqui: [Simple implementação de uma VM com Ubuntu com Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). Tem a versão mais recente [Azure CLI 2.0](/cli/azure/install-az-cli2) instalado e registado para uma conta do Azure utilizando [início de sessão az](/cli/azure/reference-index#az_login).

Primeiro, crie um grupo de recursos com [az group create](/cli/azure/group#az_group_create). O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Em seguida, implementar uma VM com [criar a implementação do grupo az](/cli/azure/group/deployment#az_group_deployment_create) que inclui a extensão de VM de Docker do Azure a partir do [este modelo Azure Resource Manager no GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). Quando solicitado, forneça os seus próprios valores exclusivos para *newStorageAccountName*, *adminUsername*, *adminPassword*, e *dnsNameForPublicIP*:

```azurecli
az group deployment create --resource-group myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Demora alguns minutos para a implementação para concluir.


## <a name="deploy-your-first-nginx-container"></a>Implementar o contentor NGINX primeiro
Para ver os detalhes da sua VM, incluindo o nome DNS, utilize [mostrar de vm az](/cli/azure/vm#az_vm_show):

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --show-details \
    --query [fqdns] \
    --output tsv
```

SSH para o novo anfitrião do Docker. Forneça o seu próprio nome de utilizador e o nome DNS dos passos anteriores:

```bash
ssh azureuser@mypublicdns.eastus.cloudapp.azure.com
```

Depois de a sessão para o anfitrião de Docker, vamos executar um contentor NGINX:

```bash
sudo docker run -d -p 80:80 nginx
```

O resultado é semelhante ao seguinte exemplo de como a imagem NGINX é transferida e um contentor iniciado:

```bash
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
a48df1751a97: Pull complete
8ddc2d7beb91: Pull complete
Digest: sha256:2ca2638e55319b7bc0c7d028209ea69b1368e95b01383e66dfe7e4f43780926d
Status: Downloaded newer image for nginx:latest
b6ed109fb743a762ff21a4606dd38d3e5d35aff43fa7f12e8d4ed1d920b0cd74
```

Verifique o estado dos contentores em execução no anfitrião do Docker da seguinte forma:

```bash
sudo docker ps
```

O resultado é semelhante ao exemplo seguinte, que mostra que o contentor NGINX está em execução e as portas TCP 80 e 443 e a ser reencaminhadas:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

Para ver o contentor em ação, abrir um browser e introduza o nome DNS do seu anfitrião de Docker:

![Contentor de ngnix em execução](./media/dockerextension/nginxrunning.png)

## <a name="azure-docker-vm-extension-template-reference"></a>Referência de modelo de extensão de VM de Docker do Azure
O exemplo anterior utiliza um modelo existente de início rápido. Também pode implementar a extensão de VM de Docker do Azure com os seus próprios modelos do Resource Manager. Para tal, adicione o seguinte para os modelos do Resource Manager, que define o `vmName` da sua VM corretamente:

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "[concat(variables('vmName'), '/DockerExtension'))]",
  "apiVersion": "2015-05-01-preview",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
  ],
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "DockerExtension",
    "typeHandlerVersion": "1.*",
    "autoUpgradeMinorVersion": true,
    "settings": {},
    "protectedSettings": {}
  }
}
```

Pode encontrar mais instruções sobre como utilizar os modelos do Resource Manager lendo [descrição geral do Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

## <a name="next-steps"></a>Passos Seguintes
Pode pretender [configurar o daemon de Docker a porta TCP](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), compreender [segurança Docker](https://docs.docker.com/engine/security/security/), ou implementar contentores utilizando [Docker Compose](https://docs.docker.com/compose/overview/). Para obter mais informações sobre a extensão de VM do Azure Docker em si, consulte o [GitHub projeto](https://github.com/Azure/azure-docker-extension/).

Ler mais informações sobre as opções de implementação de Docker adicionais no Azure:

* [Utilizar o Docker máquina com o controlador do Azure](docker-machine.md)  
* [Introdução ao Docker e Compose para definir e executar uma aplicação de contentor multi numa máquina virtual do Azure](docker-compose-quickstart.md).
* [Implementar um cluster do Azure Container Service](../../container-service/dcos-swarm/container-service-deployment.md)

