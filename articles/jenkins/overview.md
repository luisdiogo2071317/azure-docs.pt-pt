---
title: Descrição geral do Jenkins e do Azure | Microsoft Docs
description: Aloje a compilação do Jenkins e implemente o servidor de automatização no Azure e utilize os recursos de computação e armazenamento do Azure para ampliar os seus pipelines de integração e implementação contínua.
ms.topic: overview
ms.author: tarcher
author: tomarcher
manager: jpconnock
ms.service: devops
ms.custom: jenkins
ms.date: 07/25/2018
ms.openlocfilehash: ecb4ea7aee005cb539910b2cb25f0b84de7ba510
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39281560"
---
# <a name="azure-and-jenkins"></a>Azure e Jenkins

O [Jenkins](https://jenkins.io/) é um popular servidor de automatização de código aberto que é utilizado para configurar a integração e implementação contínua (CI/CD) nos seus projetos de software. Pode alojar as implementações do Jenkins no Azure ou utilizar recursos do Azure para ampliar a sua configuração atual do Jenkins. Também estão disponíveis plug-ins do Jenkins para simplificar o CI/CD das suas aplicações para o Azure.

Este artigo é uma introdução à utilização do Azure com Jenkins, pormenorizando as principais funcionalidades do Azure que estão à disposição dos utilizadores do Jenkins. Para começar a utilizar o seu próprio servidor do Jenkins no Azure, veja [Quickstart](install-jenkins-solution-template.md) (Início rápido).

## <a name="host-your-jenkins-servers-in-azure"></a>Alojar os servidores do Jenkins no Azure

Aloje o Jenkins no Azure para centralizar a automatização de compilações e dimensionar a sua implementação à medida que as necessidades dos seus projetos de software aumentam. Pode implementar o Jenkins no Azure com:
 
- [O modelo de solução do Jenkins](install-jenkins-solution-template.md) no Azure Marketplace.
- [Máquinas virtuais do Azure](/azure/virtual-machines/linux/overview). Veja o nosso [tutorial](/azure/virtual-machines/linux/tutorial-jenkins-github-docker-cicd) sobre como criar uma instância do Jenkins numa VM.
- Num cluster do Kubernetes em execução no [Azure Container Service](/azure/container-service/kubernetes/container-service-kubernetes-walkthrough); veja as [instruções](/azure/container-service/kubernetes/container-service-kubernetes-jenkins).

Monitorizar e gerir a implementação do Jenkins no Azure com o [Log Analytics](/azure/log-analytics/log-analytics-overview) e a [CLI do Azure](/cli/azure).

## <a name="scale-your-build-automation-on-demand"></a>Dimensionar a automatização da compilação a pedido

Adicione agentes de compilação à sua implementação do Jenkins existente para dimensionar a capacidade de compilação do Jenkins à medida que o número de compilações e a complexidade dos seus trabalhos e pipelines aumentam. Pode executar esses agentes de compilação em máquinas virtuais do Azure com o [plug-in Agentes de VM do Azure](jenkins-azure-vm-agents.md). Veja mais detalhes no nosso [tutorial](/azure/jenkins/jenkins-azure-vm-agents).

Quando configurados com um [principal de serviço do Azure](/azure/azure-resource-manager/resource-group-overview), os trabalhos e os pipelines do Jenkins podem utilizar esta credencial para:

- Armazenar e arquivar em segurança artefactos de compilação no [Armazenamento do Azure](/azure/storage/common/storage-introduction) com o [plug-in do Armazenamento do Azure](https://plugins.jenkins.io/windows-azure-storage). Reveja [Jenkins storage how-to](/azure/storage/common/storage-java-jenkins-continuous-integration-solution) (Instruções de armazenamento do Jenkins) para saber mais.
- Gerir e configurar os recursos do Azure com a [CLI do Azure](/azure/jenkins/execute-cli-jenkins-pipeline).

## <a name="deploy-your-code-into-azure-services"></a>Implementar o seu código nos serviços do Azure

Utilize os plug-ins do Jenkins para implementar as suas aplicações no Azure como parte dos pipelines de CI/CD do Jenkins. A implementação no [Serviço de Aplicações do Azure](/azure/app-service/) e no [Azure Container Service](/azure/container-service/kubernetes/) permite-lhe preparar, testar e lançar atualizações para as suas aplicações sem ter de gerir a infraestrutura adjacente.

 Estão disponíveis plug-ins para implementar nos seguintes serviços e ambientes:

- [Aplicação Web do Azure no Linux](/azure/app-service/containers/app-service-linux-intro). Veja o [tutorial](java-deploy-webapp-tutorial.md) para começar.
- [Aplicação Web do Azure](/azure/app-service/app-service-web-overview). Veja as [instruções](deploy-Jenkins-app-service-plugin.md) para começar.