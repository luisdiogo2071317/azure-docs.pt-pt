---
title: 'DevOps para aplicações de Inteligência Artificial (IA): a criar o pipeline de integração contínua no Azure utilizando a aplicação do Docker, Kubernetes e Python Flask'
description: 'DevOps para aplicações de Inteligência Artificial (IA): a criar o pipeline de integração contínua no Azure utilizando o Docker e Kubernetes'
services: machine-learning, team-data-science-process
documentationcenter: ''
author: jainr
manager: deguhath
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2018
ms.author: jainr
ms.openlocfilehash: 4d95fc25ed6f2f2efec8313e5b208b3cccbb619f
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38968796"
---
# <a name="devops-for-artificial-intelligence-ai-applications-creating-continuous-integration-pipeline-on-azure-using-docker-and-kubernetes"></a>DevOps para aplicações de Inteligência Artificial (IA): a criar o pipeline de integração contínua no Azure utilizando o Docker e Kubernetes
Para uma aplicação de IA, há frequentemente dois fluxos de trabalho, criação de modelos de machine learning e os desenvolvedores de aplicativos criando o aplicativo e expô-lo para os utilizadores finais consumam os cientistas de dados. Neste artigo, demonstraremos como implementar uma integração contínua (CI) / pipeline de entrega contínua (CD) para uma aplicação de IA. Aplicação de IA é uma combinação de código da aplicação incorporada com um modelo de pré-preparadas com machine learning (ML). Neste artigo, estamos buscando um modelo pretrained de uma conta de armazenamento de Blobs do Azure privado, pode ser uma conta AWS S3 também. Utilizamos uma aplicação de web do flask python simples para o artigo.

> [!NOTE]
> Essa é uma das várias formas de QUE CI/CD pode ser executada. Existem alternativas para as ferramentas e outros pré-requisitos mencionados abaixo. Como desenvolver o conteúdo adicional, publicaremos os.
>
>

## <a name="github-repository-with-document-and-code"></a>Repositório do GitHub com código e documentos
Pode baixar o código-fonte [GitHub](https://github.com/Azure/DevOps-For-AI-Apps). R [tutorial detalhado](https://github.com/Azure/DevOps-For-AI-Apps/blob/master/Tutorial.md) também está disponível.

## <a name="pre-requisites"></a>Pré-requisitos
Seguem-se os pré-requisitos para seguir o pipeline de CI/CD descrito abaixo:
* [Conta do Visual Studio Team Services](https://docs.microsoft.com/vsts/accounts/create-account-msa-or-work-student)
* [CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
* [Cluster do Container Service (AKS) do Azure em execução no Kubernetes](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-tutorial-kubernetes-deploy-cluster)
* [Conta de inventário de contentor (ACR) do Azure](https://docs.microsoft.com/azure/container-registry/container-registry-get-started-portal)
* [Instale o Kubectl para executar comandos no cluster de Kubernetes.](https://kubernetes.io/docs/tasks/tools/install-kubectl/) Temos esta opção para obter a configuração do cluster do ACS. 
* Bifurcar o repositório à sua conta do GitHub.

## <a name="description-of-the-cicd-pipeline"></a>Descrição do pipeline CI/CD
A pipeline entra em ação desativar para cada consolidação de novo, execute o conjunto de testes, se a passes de teste assume a compilação mais recente, os pacotes-lo num contentor do Docker. O contentor, em seguida, é implementado com o serviço de contentor do Azure (ACS) e imagens estão armazenadas em segurança no Azure container registry (ACR). ACS está em execução no Kubernetes para o gerenciamento de cluster de contentor, mas pode escolher o Docker Swarm ou o Mesos.

A aplicação em segurança obtém o modelo mais recente de uma conta de armazenamento do Azure e os pacotes que como parte do aplicativo. A aplicação implementada tem o código da aplicação e o modelo de ML empacotadas como contentor único. Isso dissocia os aplicação os programadores e cientistas de dados, para se certificar de que a aplicação de produção está sempre em execução o código mais recente com o modelo de ML mais recente.

A arquitetura de pipeline é indicada abaixo. 

![Arquitetura](./media/ci-cd-flask/Architecture.PNG?raw=true)

## <a name="steps-of-the-cicd-pipeline"></a>Passos para o pipeline de CI/CD
1. Para desenvolvedores trabalhar num IDE de sua escolha no código da aplicação.
2. Eles consolidar o código ao controle de fonte de sua escolha (VSTS tem bom suporte para vários controles da fonte)
3. Separadamente, cientista de dados funcionam sobre como desenvolver o seu modelo.
4. Depois de satisfeito, publicam o modelo para um repositório de modelo, neste caso estamos a utilizar uma conta de armazenamento de Blobs. Isso pode ser facilmente substituído com o serviço de gestão de modelos do Azure ML Workbench através de suas APIs de REST.
5. Uma compilação é iniciada no VSTS com base na consolidação no GitHub.
6. Pipeline de compilação VSTS obtém o modelo mais recente do contentor de BLOBs e cria um contentor.
7. VSTS envia a imagem para o repositório de imagem privada no Azure Container Registry
8. Numa agenda definida (noturna), o pipeline de lançamento é iniciada.
9. Imagem mais recente do ACR é obtida e implementada no cluster de Kubernetes no ACS.
10. Pedido de utilizadores da aplicação passa pelo servidor DNS.
11. Servidor DNS passa a solicitação ao balanceador de carga e envia a resposta de volta para o utilizador.

## <a name="next-steps"></a>Passos Seguintes
* Consulte a [tutorial]((https://github.com/Azure/DevOps-For-AI-Apps/blob/master/Tutorial.md)) para seguir os detalhes e implementar seu próprio pipeline CI/CD para a sua aplicação.

## <a name="references"></a>Referências
* [Processo de ciência de dados de equipa (TDSP)](https://aka.ms/tdsp)
* [Azure Machine Learning (AML)](https://docs.microsoft.com/azure/machine-learning/service/)
* [Visual Studio Team Services (VSTS)](https://www.visualstudio.com/vso/)
* [Serviços de Kubernetes do Azure (AKS)](https://docs.microsoft.com/azure/aks/intro-kubernetes)