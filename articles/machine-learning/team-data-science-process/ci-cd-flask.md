---
title: 'DevOps para aplicações de Artificial Intelligence (AI): a criar o pipeline de integração contínua no Azure utilizando a aplicação de Docker, Kubernetes & Python Flask'
description: 'DevOps para aplicações de Artificial Intelligence (AI): a criar o pipeline de integração contínua no Azure utilizando o Docker e Kubernetes'
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
ms.openlocfilehash: 233da393bb9e030d885ce588f4841dc1c707c1cb
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "34836271"
---
# <a name="devops-for-artificial-intelligence-ai-applications-creating-continuous-integration-pipeline-on-azure-using-docker-and-kubernetes"></a>DevOps para aplicações de Artificial Intelligence (AI): a criar o pipeline de integração contínua no Azure utilizando o Docker e Kubernetes
Para uma aplicação de AI, há frequentemente dois fluxos de trabalho, criação de modelos de machine learning e os programadores de aplicações criar a aplicação e a sua exposição para os utilizadores finais consumam de cientistas de dados. Neste artigo, vamos demonstrar como implementar uma integração contínua (CI) / pipeline de entrega contínua (CD) para uma aplicação de AI. Aplicação de AI é uma combinação de código da aplicação incorporado com um modelo de máquina pretrained learning (ML). Para este artigo, vamos estão a obter um modelo pretrained de uma conta de armazenamento de Blobs do Azure privado, poderia ser uma conta AWS S3, bem como. Utilizaremos uma aplicação de web simples python flask para o artigo.

> [!NOTE]
> Esta é uma das várias formas QUE CI/CD pode ser efetuada. Existem alternativas para a ferramentas e outros pré-requisitos mencionados abaixo. Como podemos desenvolver conteúdo adicional, vamos publicar os.
>
>

## <a name="github-repository-with-document-and-code"></a>Repositório do GitHub com código e documentos
Pode transferir o código de origem do [GitHub](https://github.com/Azure/DevOps-For-AI-Apps). A [tutorial detalhada](https://github.com/Azure/DevOps-For-AI-Apps/blob/master/Tutorial.md) também está disponível.

## <a name="pre-requisites"></a>Pré-requisitos
Seguem-se os pré-requisitos para seguir o pipeline de CI/CD descrito abaixo:
* [Conta do Visual Studio Team Services](https://docs.microsoft.com/en-us/vsts/accounts/create-account-msa-or-work-student)
* [CLI do Azure](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)
* [Cluster de serviço de contentor (AKS) do Azure em execução Kubernetes](https://docs.microsoft.com/en-us/azure/container-service/kubernetes/container-service-tutorial-kubernetes-deploy-cluster)
* [Conta de contentor Registy (ACR) do Azure](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-portal)
* [Instale Kubectl para executar comandos nos Kubernetes cluster.](https://kubernetes.io/docs/tasks/tools/install-kubectl/) É necessário para obter a configuração do cluster de ACS. 
* Copiar o repositório à sua conta do GitHub.

## <a name="description-of-the-cicd-pipeline"></a>Descrição do pipeline CI/CD
Os kicks pipeline desativado para cada consolidação novo, execute o conjunto de teste, caso o teste transmite demora a compilação mais recente, os pacotes-lo num contentor de Docker. O contentor, em seguida, é implementado utilizando o serviço de contentor do Azure (ACS) e imagens estão armazenadas em segurança no registo de contentor do Azure (ACR). ACS está a ser executado Kubernetes para gerir o cluster de contentor, mas pode escolher Docker Swarm ou Mesos.

A aplicação de forma segura obtém o modelo mais recente de uma conta de armazenamento do Azure e pacotes que como parte da aplicação. A aplicação implementada tem o código da aplicação e o modelo de ML empacotadas como contentor único. Isto desassocia as aplicação aos programadores e os dados cientistas, para se certificar de que as respetivas aplicações de produção sempre está a executar o código mais recente com o modelo de ML mais recente.

A arquitetura de pipeline é indicada abaixo. 

![Arquitetura](./media/ci-cd-flask/Architecture.PNG?raw=true)

## <a name="steps-of-the-cicd-pipeline"></a>Passos para o pipeline de CI/CD
1. Programador funcionam em IDE da sua eleição no código da aplicação.
2. São consolidar o código para controlo de código fonte da sua eleição (VSTS tem boa suporte para vários controlos de origem)
3. Separadamente, a scientist dados funcionará desenvolver o seu modelo.
4. Depois de satisfeito, publicam o modelo para um repositório de modelo, neste caso, iremos estiver a utilizar uma conta do blob storage. Isto pode ser facilmente substituído com o serviço de gestão de modelo do Azure ML Workbench através das respetivas APIs REST.
5. Uma compilação arrancou no VSTS com base na consolidação no GitHub.
6. Criar VSTS pipeline obtém o modelo mais recente a partir do contentor de Blob e cria um contentor.
7. VSTS pushes a imagem para o repositório de imagens privada no registo de contentor do Azure
8. Numa agenda definida (noturna), o pipeline de versão é arrancou.
9. Imagem mais recente do ACR é solicitada e implementada através de cluster de Kubernetes em ACS.
10. Pedido de utilizadores da aplicação passa pelo servidor DNS.
11. Servidor DNS transmite o pedido para o Balanceador de carga e envia a resposta de volta para o utilizador.

## <a name="next-steps"></a>Passos Seguintes
* Consulte o [tutorial]((https://github.com/Azure/DevOps-For-AI-Apps/blob/master/Tutorial.md)) siga os detalhes e implementar a suas próprias pipeline CI/CD para a sua aplicação.

## <a name="references"></a>Referências
* [Processo de ciência de dados de equipa (TDSP)](https://aka.ms/tdsp)
* [Aprendizagem do Azure (AML)](https://docs.microsoft.com/en-us/azure/machine-learning/service/)
* [Visual Studio Team Services (VSTS)](https://www.visualstudio.com/vso/)
* [Serviços do Azure Kubernetes (AKS)](https://docs.microsoft.com/en-us/azure/aks/intro-kubernetes)