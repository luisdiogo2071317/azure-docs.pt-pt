---
title: Utilizar o Docker Compose numa VM do Linux no Azure | Documentos da Microsoft
description: Como utilizar o Docker e Compose em máquinas de virtuais do Linux com a CLI do Azure
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 02ab8cf9-318d-4a28-9d0c-4a31dccc2a84
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 12/18/2017
ms.author: cynthn
ms.openlocfilehash: 227e6b87e5a131147ffebdeac045b9b27ab20dc8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991322"
---
# <a name="get-started-with-docker-and-compose-to-define-and-run-a-multi-container-application-in-azure"></a>Introdução ao Docker e o Compose para definir e executar uma aplicação de vários contentores no Azure
Com o [Compose](http://github.com/docker/compose), utilizar um ficheiro de texto simples para definir uma aplicação constituída por vários contentores do Docker. , Em seguida, crie a sua aplicação com um único comando que faz tudo para implementar o seu ambiente definido. Por exemplo, este artigo mostra-lhe como configurar rapidamente um blogue do WordPress com um back-end da base de dados do MariaDB SQL numa VM do Ubuntu. Também pode utilizar o Compose para configurar as aplicações mais complexas.


## <a name="set-up-a-linux-vm-as-a-docker-host"></a>Configurar uma VM com Linux como anfitrião Docker
Pode usar vários procedimentos do Azure e as imagens disponíveis ou modelos do Resource Manager no Azure Marketplace para criar uma VM do Linux e configurá-lo como anfitrião Docker. Por exemplo, veja [utilizar a extensão de VM do Docker para implementar o seu ambiente](dockerextension.md) para criar rapidamente uma VM do Ubuntu com a extensão de VM do Azure Docker, utilizando um [modelo de início rápido](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

Quando utiliza a extensão Docker VM, a VM é automaticamente configurada como um anfitrião do Docker e Compose já está instalado.


### <a name="create-docker-host-with-azure-cli"></a>Criar anfitrião do Docker com a CLI do Azure
Instalar a versão mais recente [CLI do Azure](/cli/azure/install-az-cli2) e para iniciar sessão no Azure através da conta [início de sessão az](/cli/azure/reference-index#az_login).

Primeiro, crie um grupo de recursos para o seu ambiente do Docker com [criar grupo az](/cli/azure/group#az_group_create). O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Em seguida, implementar uma VM com [criar a implementação do grupo az](/cli/azure/group/deployment#az_group_deployment_create) que inclui a extensão Azure Docker VM a partir [este modelo de Gestor de recursos do Azure no GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). Quando solicitado, forneça os seus próprios valores exclusivos para *newStorageAccountName*, *adminUsername*, *adminPassword*, e *dnsNameForPublicIP*:

```azurecli
az group deployment create --resource-group myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Demora alguns minutos para conclusão da implementação.


## <a name="verify-that-compose-is-installed"></a>Certifique-se de que a composição está instalada
Para ver os detalhes da sua VM, incluindo o nome DNS, utilize [show de vm de az](/cli/azure/vm#az_vm_show):

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

Para verificar que a composição é instalada na VM, execute o seguinte comando:

```bash
docker-compose --version
```

Ver o resultado semelhante *docker-compose 1.6.2, compilação 4d 72027*.

> [!TIP]
> Se utilizou o outro método para criar um anfitrião do Docker e tem de instalar compor por conta própria, consulte a [Compose documentação](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).


## <a name="create-a-docker-composeyml-configuration-file"></a>Criar um ficheiro de configuração de docker-Compose
Em seguida, crie um `docker-compose.yml` arquivo, que é apenas um arquivo de configuração texto, para definir os contentores de Docker para executar na VM. O ficheiro Especifica a imagem a ser executado em cada contentor (ou pode ser uma compilação a partir de um Dockerfile), variáveis de ambiente necessárias e as dependências, portas e as ligações entre contentores. Para obter detalhes sobre a sintaxe do arquivo yml, consulte [Compose referência do arquivo](https://docs.docker.com/compose/compose-file/).

Criar uma *docker-Compose* ficheiro. Utilize o seu editor de texto favorito para adicionar alguns dados para o ficheiro. O exemplo seguinte cria o ficheiro com uma linha de comandos para `sensible-editor` para escolher um editor que deseja usar:

```bash
sensible-editor docker-compose.yml
```

Cole o seguinte exemplo no seu ficheiro do Docker Compose. Esta configuração utiliza imagens a partir da [DockerHub registo](https://registry.hub.docker.com/_/wordpress/) para instalar o WordPress (o código-fonte aberto escrevendo em seu blog e conteúdo de sistema de gerenciamento) e um back-end ligado base de dados MariaDB SQL. Introduza o seu próprio *MYSQL_ROOT_PASSWORD* da seguinte forma:

```sh
wordpress:
  image: wordpress
  links:
    - db:mysql
  ports:
    - 80:80

db:
  image: mariadb
  environment:
    MYSQL_ROOT_PASSWORD: <your password>
```

## <a name="start-the-containers-with-compose"></a>Iniciar os contentores com Compose
No mesmo diretório do seu *docker-Compose* arquivo, execute o seguinte comando (dependendo do seu ambiente, poderá ter de executar `docker-compose` usando `sudo`):

```bash
docker-compose up -d
```

Este comando inicia os contentores do Docker especificados no *docker-Compose*. Demora um minuto ou dois para que este passo concluir. Verá um resultado semelhante ao seguinte exemplo:

```bash
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```

> [!NOTE]
> Certifique-se de que utilize o **-d** opção no arranque, para que os contentores executem continuamente em segundo plano.


Para verificar que os contentores de cópia de segurança, escreva `docker-compose ps`. Deverá ver algo como:

```bash
        Name                       Command               State         Ports
-----------------------------------------------------------------------------------
azureuser_db_1          docker-entrypoint.sh mysqld      Up      3306/tcp
azureuser_wordpress_1   docker-entrypoint.sh apach ...   Up      0.0.0.0:80->80/tcp
```

Pode agora ligar wordpress diretamente na VM na porta 80. Abra um browser e introduza o nome DNS da sua VM (como `http://mypublicdns.eastus.cloudapp.azure.com`). Agora, deverá ver o WordPress tela iniciar, onde pode concluir a instalação e começar a utilizar com o aplicativo.

![Ecrã inicial do WordPress][wordpress_start]

## <a name="next-steps"></a>Passos Seguintes
* Vá para o [Guia do utilizador da extensão Docker VM](https://github.com/Azure/azure-docker-extension/blob/master/README.md) para obter mais opções configurar o Docker e Compose na sua VM do Docker. Por exemplo, uma opção é colocar o arquivo yml de composição (convertido em JSON) diretamente na configuração da extensão de VM do Docker.
* Veja a [Compose referência da linha de comandos](http://docs.docker.com/compose/reference/) e [Guia do usuário](http://docs.docker.com/compose/) para obter mais exemplos de criação e implementação de aplicações de vários contentores.
* Utilizar um modelo Azure Resource Manager, optar por seu próprio ou um contributo do [Comunidade](https://azure.microsoft.com/documentation/templates/), para implementar uma VM do Azure com o Docker e um aplicativo configurado com a composição. Por exemplo, o [implementação de um blogue de WordPress com o Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) modelo utiliza o Docker e o Compose para implementar rapidamente o WordPress com um back-end MySQL numa VM do Ubuntu.
* Experimente a integrar o Docker Compose com um cluster Docker Swarm. Ver [usando Compose com o Swarn](https://docs.docker.com/compose/swarm/) para cenários.

<!--Image references-->

[wordpress_start]: media/docker-compose-quickstart/WordPress.png
