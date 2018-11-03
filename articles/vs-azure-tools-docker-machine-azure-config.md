---
title: Criar anfitriões do Docker no Azure com máquina do Docker | Documentos da Microsoft
description: Descreve o uso do Docker Machine para criar anfitriões do docker no Azure.
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: ''
ms.assetid: 7a3ff6e1-fa93-4a62-b524-ab182d2fea08
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: 98b2d76f4b8ba9ba8316f3a33fd2ff5ed5899833
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969532"
---
# <a name="create-docker-hosts-in-azure-with-docker-machine"></a>Criar Anfitriões do Docker no Azure com Máquina do Docker
Em execução [Docker](https://www.docker.com/) contentores necessita de um anfitrião VM a executar o daemon do docker.
Este tópico descreve como utilizar o [docker-machine](https://docs.docker.com/machine/) comando para criar novas VMs do Linux, configurada com o daemon do Docker, em execução no Azure. 

**Nota:** 

* *Este artigo depende na versão de uma máquina docker 0.9.0-rc2 ou superior*
* *Contentores do Windows será dado por uma máquina docker em breve*

## <a name="create-vms-with-docker-machine"></a>Criar VMs com o Docker Machine
Criar VMs de anfitrião de docker no Azure com o `docker-machine create` comando com o `azure` driver. 

O controlador do Azure requer o ID da subscrição. Pode utilizar o [CLI do Azure](cli-install-nodejs.md) ou o [Portal do Azure](https://portal.azure.com) para recuperar a sua subscrição do Azure. 

**No Portal do Azure**

* Selecione **subscrições** da página de navegação à esquerda e copie o id de subscrição.

**Com a CLI do Azure**

* Tipo de ```azure account list``` e copie o id de subscrição.

Tipo de `docker-machine create --driver azure` para ver as opções e os respetivos valores predefinidos.
Também pode ver o [documentação de controlador do Docker Azure](https://docs.docker.com/machine/drivers/azure/) para obter mais informações. 

O exemplo seguinte baseia-se após a [valores predefinidos](https://github.com/docker/machine/blob/master/drivers/azure/azure.go#L22), mas ele se desejar, defina estes valores: 

* dns do Azure para o nome associado ao IP público e os certificados gerados. Este é o nome DNS da sua máquina virtual. A VM pode, em seguida, com segurança parar, o IP dinâmico de versão e fornecer a capacidade de voltar a ligar depois da vm entrar novamente com um novo IP. O prefixo do nome tem de ser exclusivo para essa região UNIQUE_DNSNAME_PREFIX.westus.cloudapp.azure.com.
* abrir a porta 80 na VM para acesso de internet de saída
* tamanho da VM para utilizar o armazenamento premium mais rápido
* armazenamento Premium, utilizado para o disco de vm

```
docker-machine create -d azure --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> --azure-dns <Your UNIQUE_DNSNAME_PREFIX> --azure-open-port 80 --azure-size Standard_DS1_v2 --azure-storage-type "Premium_LRS" mydockerhost 
```

## <a name="choose-a-docker-host-with-docker-machine"></a>Escolha um anfitrião do docker com máquina do docker
Assim que tiver uma entrada numa máquina docker para o anfitrião, pode definir o host padrão ao executar comandos do docker.

## <a name="using-powershell"></a>Com o PowerShell
```powershell
docker-machine env MyDockerHost | Invoke-Expression 
```

## <a name="using-bash"></a>Utilizar o Bash
```bash
eval $(docker-machine env MyDockerHost)
```

Agora, pode executar comandos do docker com o anfitrião especificado

```
docker ps
docker info
```

## <a name="run-a-container"></a>Executar um contentor
Com um anfitrião configurado, agora, pode executar um servidor web simples para testar se o anfitrião foi configurado corretamente.
Aqui vamos utilizar uma imagem da nginx padrão, especificar que deve escutar na porta 80 e que, se o anfitrião VM reiniciar, o contentor irá reiniciam também (`--restart=always`). 

```bash
docker run -d -p 80:80 --restart=always nginx
```

O resultado deverá ser semelhante ao seguinte:

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
83f52fbfa5f8: Pull complete
fa664caa1402: Pull complete
Digest: sha256:12127e07a75bda1022fbd4ea231f5527a1899aad4679e3940482db3b57383b1d
Status: Downloaded newer image for nginx:latest
25942c35d86fe43c688d0c03ad478f14cc9c16913b0e1c2971cb32eb4d0ab721
```

## <a name="test-the-container"></a>Testar o contentor
Examine os contentores em execução usando `docker ps`:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
d5b78f27b335        nginx               "nginx -g 'daemon off"   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp, 443/tcp   goofy_mahavira
```

E, para ver o contentor em execução, escreva `docker-machine ip <VM name>` para encontrar o endereço IP a introduzir no browser:

```
PS C:\> docker-machine ip MyDockerHost
191.237.46.90
```

![Contentor de ngnix em execução](./media/vs-azure-tools-docker-machine-azure-config/nginxsuccess.png)

## <a name="summary"></a>Resumo
Com máquina do docker, pode aprovisionar facilmente anfitriões do docker no Azure para sua validações de anfitrião do docker individuais.
Para produção de alojamento de contentores, consulte o [Azure Container Service](http://aka.ms/AzureContainerService)

Para desenvolver aplicativos do .NET Core com o Visual Studio, consulte [ferramentas do Docker para Visual Studio](http://aka.ms/DockerToolsForVS)

