---
title: Criar imagens de contentor para o Azure Service Fabric | Microsoft Docs
description: "Saiba como criar imagens de contentor para uma aplicação de Service Fabric multi contentor."
services: service-fabric
documentationcenter: 
author: suhuruli
manager: timlt
editor: suhuruli
tags: servicefabric
keywords: "Docker, contentores, micro-serviços, recursos de infraestrutura de serviço, do Azure"
ms.assetid: 
ms.service: service-fabric
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/15/2017
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: 9ea5be818cfc104c243ce31cc0e2d0f10135259f
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/11/2018
---
# <a name="create-container-images-for-service-fabric"></a>Criar imagens de contentor para o Service Fabric

Este tutorial faz parte de um de uma série tutorial que demonstra como utilizar contentores num cluster de Linux Service Fabric. Neste tutorial, uma aplicação de contentor multi está preparada para utilização com o Service Fabric. Nos tutoriais subsequentes, estas imagens são utilizadas como parte de uma aplicação de Service Fabric. Neste tutorial, ficará a saber como: 

> [!div class="checklist"]
> * Origem de clone da aplicação a partir do GitHub  
> * Criar uma imagem de contentor da origem de aplicação
> * Implemente uma instância de registo de contentor do Azure (ACR)
> * Marcar uma imagem de contentor para o ACR
> * Carregue a imagem para o ACR

Este tutorial série, saiba como: 

> [!div class="checklist"]
> * Criar imagens de contentor para o Service Fabric
> * [Compilar e executar uma aplicação de recursos de infraestrutura de serviço com contentores](service-fabric-tutorial-package-containers.md)
> * [Como a ativação pós-falha e dimensionamento são processados no Service Fabric](service-fabric-tutorial-containers-failover.md)

## <a name="prerequisites"></a>Pré-requisitos

- Ambiente de desenvolvimento do Linux estar configurada para o Service Fabric. Siga as instruções [aqui](service-fabric-get-started-linux.md) para configurar o ambiente do Linux. 
- Este tutorial requer que está a executar a CLI do Azure versão 2.0.4 ou posterior. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli). 
- Além disso, requer que tem uma subscrição do Azure disponível. Para obter mais informações sobre uma versão de avaliação gratuita, visite [aqui](https://azure.microsoft.com/free/).

## <a name="get-application-code"></a>Obter o código da aplicação

A aplicação de exemplo utilizada neste tutorial é uma aplicação de voto. A aplicação é composta por um componente de front-end web e uma instância de Redis do back-end. Os componentes são reunidos em imagens de contentor. 

Utilize o git para transferir uma cópia da aplicação para o ambiente de desenvolvimento.

```bash
git clone https://github.com/Azure-Samples/service-fabric-containers.git

cd service-fabric-containers/Linux/container-tutorial/
```

A solução inclui duas pastas e um ' docker-compse.yml' ficheiros. A pasta 'azure voto' contém o serviço de front-end do Python, juntamente com o Dockerfile utilizado para criar a imagem. O diretório 'Voto' contém o pacote de aplicação de Service Fabric é implementado para o cluster. Estes diretórios contém os recursos necessários para este tutorial.  

## <a name="create-container-images"></a>Criar imagens de contentor

Dentro do **azure voto** diretório, execute o seguinte comando para criar a imagem para o componente de web de front-end. Este comando utiliza o Dockerfile neste diretório para compilar a imagem. 

```bash
docker build -t azure-vote-front .
```

Este comando pode demorar algum tempo, uma vez que todas as dependências necessárias têm de ser solicitados a partir do Hub de Docker. Quando tiver terminado, utilize o [imagens docker](https://docs.docker.com/engine/reference/commandline/images/) comando para ver as imagens criadas.

```bash
docker images
```

Tenha em atenção que foram transferidas ou criadas duas imagens. O *voto-azure-frente* imagem contém a aplicação. Foi derivado um *python* imagem a partir do Hub de Docker.

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
azure-vote-front             latest              052c549a75bf        About a minute ago   708MB
tiangolo/uwsgi-nginx-flask   python3.6           590e17342131        5 days ago           707MB

```

## <a name="deploy-azure-container-registry"></a>Implementar o registo de contentor do Azure

Execute primeiro o **início de sessão az** comando para iniciar sessão sua conta do Azure. 

```bash
az login
```

Em seguida, utilize o **conta az** comando para escolha a sua subscrição para criar o registo de contentor do Azure. Tem de introduzir o ID de subscrição da sua subscrição do Azure em vez de < subscription_id >. 

```bash
az account set --subscription <subscription_id>
```

Quando implementar um registo de contentor do Azure, primeiro precisa de um grupo de recursos. Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos.

Crie um grupo de recursos com o comando **az group create**. Neste exemplo, um grupo de recursos denominado *myResourceGroup* é criado no *westus* região.

```bash
az group create --name <myResourceGroup> --location westus
```

Criar um registo de contentor do Azure com o **az acr criar** comando. Substitua \<acrName > com o nome do registo de contentor que pretende criar na sua subscrição. Este nome tem de ser exclusivo e alfanuméricos. 

```bash
az acr create --resource-group <myResourceGroup> --name <acrName> --sku Basic --admin-enabled true
```

Em todo o resto deste tutorial, utilizamos "acrName" como um marcador de posição para o nome do registo de contentor que escolheu. . Anote este valor. 

## <a name="log-in-to-your-container-registry"></a>Inicie sessão no seu registo de contentor

Iniciar sessão na sua instância ACR antes de enviar imagens à mesma. Utilize o **início de sessão do az acr** comando para concluir a operação. Forneça o nome exclusivo especificado no registo do contentor quando foi criado.

```bash
az acr login --name <acrName>
```

O comando devolve uma mensagem 'Início de sessão com êxito' depois de concluir.

## <a name="tag-container-images"></a>Imagens de contentor da etiqueta

Cada imagem de contentor tem de ser etiquetados com o nome de loginServer do registo. Esta etiqueta é utilizada para o encaminhamento quando enviar imagens do contentor para um registo de imagem.

Para ver uma lista de imagens atuais, utilize o [imagens docker](https://docs.docker.com/engine/reference/commandline/images/) comando.

```bash
docker images
```

Saída:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
azure-vote-front             latest              052c549a75bf        About a minute ago   708MB
tiangolo/uwsgi-nginx-flask   python3.6           590e17342131        5 days ago           707MB
```

Para obter o nome de loginServer, execute o seguinte comando:

```bash
az acr show --name <acrName> --query loginServer --output table
```

Isto produz uma tabela com os seguintes resultados. Este resultado será utilizado para a etiqueta a **voto-azure-frente** imagem antes de enviá-lo para o registo de contentor no próximo passo.

```bash
Result
------------------
<acrName>.azurecr.io
```

Agora, marcar a *voto-azure-frente* imagem com o loginServer do seu registo de contentor. Além disso, adicionar `:v1` ao fim do nome de imagem. Esta etiqueta indica a versão da imagem.

```bash
docker tag azure-vote-front <acrName>.azurecr.io/azure-vote-front:v1
```

Depois de etiquetados, executar 'imagens docker' para verificar a operação.


Saída:

```bash
REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front                       latest              052c549a75bf        23 minutes ago      708MB
<acrName>.azurecr.io/azure-vote-front   v1                  052c549a75bf       23 minutes ago      708MB
tiangolo/uwsgi-nginx-flask             python3.6           590e17342131        5 days ago          707MB

```

## <a name="push-images-to-registry"></a>Imagens de push para registo

Push de *voto-azure-frente* imagem ao registo. 

Utilizando o exemplo seguinte, substitua o nome de loginServer ACR loginServer do seu ambiente.

```bash
docker push <acrName>.azurecr.io/azure-vote-front:v1
```

Os comandos de push de docker demorar alguns minutos a concluir.

## <a name="list-images-in-registry"></a>Lista de imagens no registo

Para obter uma lista de imagens que tiver sido feito o push para o registo de contentor do Azure, utilize o [lista de repositório az acr](/cli/azure/acr/repository#list) comando. Atualize o comando com o nome da instância ACR.

```bash
az acr repository list --name <acrName> --output table
```

Saída:

```bash
Result
----------------
azure-vote-front
```

Após conclusão tutorial, a imagem de contentor tenha sido armazenada numa instância de registo de contentor do Azure privada. Esta imagem é implementada de ACR para um cluster do Service Fabric nos tutoriais subsequentes.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, uma aplicação foi solicitada a partir do Github e imagens de contentor foram criadas e enviadas por push para um registo. Foram efetuados os seguintes passos:

> [!div class="checklist"]
> * Origem de clone da aplicação a partir do GitHub  
> * Criar uma imagem de contentor da origem de aplicação
> * Implemente uma instância de registo de contentor do Azure (ACR)
> * Marcar uma imagem de contentor para o ACR
> * Carregue a imagem para o ACR

Avançar para o próximo tutorial para saber mais sobre os contentores de empacotamento para uma aplicação de Service Fabric utilizando Yeoman. 

> [!div class="nextstepaction"]
> [O pacote e implementar contentores como uma aplicação de Service Fabric](service-fabric-tutorial-package-containers.md)