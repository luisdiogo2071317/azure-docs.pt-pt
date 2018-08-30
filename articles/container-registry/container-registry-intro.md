---
title: Criar registos privados de contentores Docker no Azure
description: Introdução ao serviço de Registo de Contentores do Azure, que proporciona registos do Docker baseado na cloud, geridos e privados.
services: container-registry
author: stevelas
manager: jeconnoc
ms.service: container-registry
ms.topic: overview
ms.date: 05/08/2018
ms.author: stevelas
ms.custom: mvc
ms.openlocfilehash: f282d7d6950278d0c270009256cf054a0d630e60
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/28/2018
ms.locfileid: "43120640"
---
# <a name="introduction-to-private-docker-container-registries-in-azure"></a>Introdução aos registos privados de contentores Docker no Azure

O Registo de Contentores do Azure é um serviço de [registo do Docker](https://docs.docker.com/registry/) gerido e baseado no Docker Registry 2.0. Crie e mantenha registos de contentores do Azure para armazenar e gerir as suas imagens privadas do [contentor do Docker](https://www.docker.com/what-docker).

Utilize os registos de contentores no Azure com os seus pipelines atuais de programação e implementação de contentores. Utilize o Azure Container Registry Build (ACR Build) para compilar imagens de contentor no Azure. Compile a pedido ou automatize totalmente as compilações com acionadores de consolidação do código de origem e de compilação de atualização da imagem de base.

Para obter informações sobre o Docker e os contentores, veja [Docker overview](https://docs.docker.com/engine/docker-overview/) (Descrição geral do Docker).

## <a name="use-cases"></a>Casos de utilização

Extrair imagens de um registo de contentores do Azure para vários destinos de implementações:

* **Sistemas de orquestração dimensionáveis** que gerem aplicações contentorizadas em clusters de anfitriões, incluindo [CD/SO](https://docs.mesosphere.com/), [Docker Swarm](https://docs.docker.com/swarm/) e [Kubernetes](http://kubernetes.io/docs/).
* **Serviços do Azure** que suportam a criação e a execução de aplicações em escala, incluindo o [Azure Kubernetes Service (AKS)](../aks/index.yml), o [Serviço de Aplicações](../app-service/index.yml), o [Batch](../batch/index.yml), o [Service Fabric](/azure/service-fabric/) e outros.

Os programadores também podem enviar para um registo de contentores como parte de um fluxo de trabalho de desenvolvimento de contentores. Por exemplo, podem segmentar um registo de contentores de uma ferramenta de integração contínua e programação, como o [Visual Studio Team Services](https://www.visualstudio.com/docs/overview) ou o [Jenkins](https://jenkins.io/).

Configure tarefas de compilação do [ACR Build](#azure-container-registry-build) para recompilar automaticamente imagens de aplicações quando as respetivas imagens de base forem atualizadas. Utilize o ACR Build para automatizar compilações de imagens quando a sua equipa consolidar código para um repositório de Git. *O ACR Build encontra-se em pré-visualização.*

## <a name="key-concepts"></a>Conceitos-chave

* **Registo** - crie um ou mais registos de contentores na sua subscrição do Azure. Os registos estão disponíveis em três SKUs: [Básico, Standard e Premium](container-registry-skus.md), cada um dos quais suporta a integração de webhook, autenticação de registo no Azure Active Directory e a funcionalidade de eliminação. Tire partido do armazenamento local e perto da rede das imagens do seu contentor ao criar um registo na mesma localização do Azure das suas implementações. Utilize a funcionalidade de [georreplicação](container-registry-geo-replication.md) de registos Premium para replicação avançada e cenários de distribuição de imagens de contentor. Os nomes de registo completamente qualificados têm o formato `myregistry.azurecr.io`.

  Para [controlar o acesso](container-registry-authentication.md) a um registo de contentores, utilize um [principal de serviço](../active-directory/develop/app-objects-and-service-principals.md) baseado no Azure Active Directory ou uma conta de administrador fornecida. Execute o comando `docker login` padrão para se autenticar num registo.

* **Repositório** - um registo contém um ou mais repositórios, que são grupos de imagens do contentor. O Registo de Contentores do Azure suporta espaços de nomes de repositórios com múltiplos níveis. Com espaços de nomes de vários níveis, pode agrupar coleções de imagens relacionadas com uma aplicação específica ou uma coleção de aplicações para equipas de programação ou operacionais específicas. Por exemplo:

  * `myregistry.azurecr.io/aspnetcore:1.0.1` representa uma imagem transversal a toda a empresa
  * `myregistry.azurecr.io/warrantydept/dotnet-build` representa uma imagem utilizada para criar aplicações .NET, partilhadas em todo o departamento de garantias
  * `myregistry.azurecr.io/warrantydept/customersubmissions/web` representa uma imagem Web, agrupada na aplicação de submissões do cliente, que é propriedade do departamento de garantias

* **Imagem** - armazenadas num repositório, as imagens são um instantâneo só de leitura de contentores do Docker. O registo de contentores do Azure pode incluir imagens do Windows e do Linux. O utilizador controla os nomes de todas as implementações de contentores. Utilize os [comandos do Docker](https://docs.docker.com/engine/reference/commandline/) padrão para enviar imagens para um repositório ou extrair imagens de um repositório.

* **Contentor** - um contentor define uma aplicação de software e as respetivas dependências, envoltas num sistema de ficheiros completo, que inclui código, tempo de execução, ferramentas do sistema e bibliotecas. Execute os contentores do Docker com base nas imagens do Windows ou do Linux que extrai de um registo de contentores. Os contentores executados num computador individual partilham o kernel do sistema operativo. Os contentores do Docker são totalmente portáteis para as principais distribuições de Linux e para macOS e Windows.

## <a name="azure-container-registry-build-preview"></a>Azure Container Registry Build (Pré-visualização)

O [Azure Container Registry Build](container-registry-build-overview.md) (ACR Build) é um conjunto de funcionalidades no Azure Container Registry que fornece compilações de imagens de contentor do Docker simplificadas e eficientes no Azure. Utilize o ACR Build para expandir o seu ciclo interno de programação para a cloud ao descarregar operações `docker build` para o Azure. Configure tarefas de compilação para automatizar o SO do contentor e o pipeline de aplicação de patches de arquitetura e compilar imagens automaticamente quando a sua equipa consolidar código para o controlo de origem.

[!INCLUDE [container-registry-build-preview-note](../../includes/container-registry-build-preview-note.md)]

## <a name="next-steps"></a>Passos seguintes

* [Create a container registry using the Azure portal (Criar um registo de contentores com o portal do Azure)](container-registry-get-started-portal.md)
* [Create a container registry using the Azure CLI (Criar um registo de contentores com a CLI do Azure)](container-registry-get-started-azure-cli.md)
* [Automatizar o SO e a aplicação de patches com o ACR Build](container-registry-build-overview.md) (Pré-visualização)
