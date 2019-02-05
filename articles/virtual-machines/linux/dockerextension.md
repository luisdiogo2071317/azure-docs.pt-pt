---
title: Utilizar a extensão de VM do Azure Docker | Documentos da Microsoft
description: Saiba como utilizar a extensão Docker VM para rapidamente e com segurança implementar um ambiente do Docker no Azure com modelos do Resource Manager e a CLI do Azure
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
ms.assetid: 936d67d7-6921-4275-bf11-1e0115e66b7f
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/18/2017
ms.author: cynthn
ms.openlocfilehash: 1bc250ac70e48a548d393c3bc6025868948dc022
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/04/2019
ms.locfileid: "55699164"
---
# <a name="create-a-docker-environment-in-azure-using-the-docker-vm-extension"></a>Criar um ambiente do Docker no Azure com a extensão Docker VM

O docker é uma plataforma de geração de imagens que lhe permite trabalhar rapidamente com contentores no Linux e gestão de contentores populares. No Azure, existem várias formas como pode implementar o Docker, de acordo com suas necessidades. Este artigo concentra-se sobre como utilizar a extensão Docker VM e os modelos Azure Resource Manager com a CLI do Azure. 

> [!WARNING]
> A extensão de VM do Azure Docker para Linux é preterida e vai ser descontinuada a Novembro de 2018.
> A extensão instala apenas Docker, para que as alternativas, como o cloud-init ou a extensão de Script personalizado são uma maneira melhor para instalar a versão do Docker à escolha. Para obter mais informações sobre como utilizar o cloud-init, veja [personalizar uma VM do Linux com o cloud-init](tutorial-automate-vm-deployment.md).

## <a name="azure-docker-vm-extension-overview"></a>Descrição geral da extensão Docker VM do Azure
A extensão Azure Docker VM instala e configura o daemon do Docker, o cliente de Docker e Docker Compose na sua máquina virtual (VM) do Linux. Ao utilizar a extensão de VM do Azure Docker, ter mais controle e recursos do que simplesmente com o Docker Machine ou criar o anfitrião do Docker por conta própria. Adicionais estas funcionalidades, como [Docker Compose](https://docs.docker.com/compose/overview/), marca a extensão Azure Docker VM adequada para ambientes de programador ou de produção mais robustos.

Para obter mais informações sobre os métodos de implementação diferentes, incluindo a utilização de máquina do Docker e o Azure Container Service, veja os artigos seguintes:

* Protótipos rapidamente uma aplicação, pode criar um único anfitrião do Docker com [Docker Machine](docker-machine.md).
* Para criar ambientes prontos para produção, dimensionáveis, que fornecem ferramentas adicionais de agendamento e gestão, pode implementar um [Kubernetes](../../container-service/kubernetes/index.yml) ou [Docker Swarm](../../container-service/dcos-swarm/index.yml) cluster nos serviços de contentor do Azure.


## <a name="deploy-a-template-with-the-azure-docker-vm-extension"></a>Implementar um modelo com a extensão de VM do Azure Docker
Vamos utilizar um modelo existente de início rápido para criar uma VM do Ubuntu que utiliza a extensão de VM do Azure Docker para instalar e configurar o anfitrião do Docker. Pode ver o modelo aqui: [Implementação simples de uma VM do Ubuntu com o Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). Tem a versão mais recente [CLI do Azure](/cli/azure/install-az-cli2) instalado e registado à utilização conta do Azure [início de sessão az](/cli/azure/reference-index).

Primeiro, crie um grupo de recursos com [az group create](/cli/azure/group). O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Em seguida, implementar uma VM com [criar a implementação do grupo az](/cli/azure/group/deployment) que inclui a extensão Azure Docker VM a partir [este modelo de Gestor de recursos do Azure no GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). Quando solicitado, forneça os seus próprios valores exclusivos para *newStorageAccountName*, *adminUsername*, *adminPassword*, e *dnsNameForPublicIP*:

```azurecli
az group deployment create --resource-group myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Demora alguns minutos para conclusão da implementação.


## <a name="deploy-your-first-nginx-container"></a>Implemente o seu primeiro contentor NGINX
Para ver os detalhes da sua VM, incluindo o nome DNS, utilize [show de vm de az](/cli/azure/vm):

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --show-details \
    --query [fqdns] \
    --output tsv
```

SSH para o novo anfitrião do Docker. Fornece seu próprio nome de utilizador e o nome DNS dos passos anteriores:

```bash
ssh azureuser@mypublicdns.eastus.cloudapp.azure.com
```

Depois de iniciar sessão para o anfitrião do Docker, vamos executar um contentor NGINX:

```bash
sudo docker run -d -p 80:80 nginx
```

O resultado é semelhante ao seguinte exemplo à medida que a imagem da NGINX é transferida e iniciado de um contentor:

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

O resultado é semelhante ao exemplo seguinte, que mostra que o contentor de NGINX está em execução e as portas TCP 80 e 443 e que está a ser reencaminhados:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

Para ver o contentor em ação, abra um browser e introduza o nome DNS do seu anfitrião do Docker:

![Contentor de ngnix em execução](./media/dockerextension/nginxrunning.png)

## <a name="azure-docker-vm-extension-template-reference"></a>Referência de modelo de extensão Docker VM do Azure
O exemplo anterior utiliza um modelo de início rápido existente. Também pode implementar a extensão de VM do Azure Docker com seus próprios modelos do Resource Manager. Para tal, adicione o seguinte para modelos do Resource Manager, definindo o `vmName` da sua VM adequadamente:

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

Pode encontrar mais detalhadas passo a passo sobre como utilizar modelos do Resource Manager lendo [descrição geral do Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

## <a name="next-steps"></a>Passos Seguintes
Pode desejar [configurar o daemon do Docker a porta TCP](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), compreender [segurança Docker](https://docs.docker.com/engine/security/security/), ou implementar contentores utilizando [Docker Compose](https://docs.docker.com/compose/overview/). Para obter mais informações sobre a extensão de VM do Azure Docker em si, consulte a [projeto GitHub](https://github.com/Azure/azure-docker-extension/).

Ler mais informações sobre as opções de implementação adicionais do Docker no Azure:

* [Utilizar o Docker Machine com o controlador do Azure](docker-machine.md)  
* [Introdução ao Docker e o Compose para definir e executar uma aplicação de vários contentores numa máquina virtual do Azure](docker-compose-quickstart.md).
* [Implementar um cluster do Azure Container Service](../../container-service/dcos-swarm/container-service-deployment.md)

