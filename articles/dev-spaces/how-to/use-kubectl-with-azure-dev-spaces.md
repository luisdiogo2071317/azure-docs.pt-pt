---
title: Como utilizar kubectl com espaços de programador do Azure | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: article
description: Desenvolvimento rápido da Kubernetes com contentores e microsserviços no Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contentores
manager: douge
ms.openlocfilehash: 3dc0dd4b571f716bcabb67c4cbef1ea6d762eb94
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35248663"
---
# <a name="use-kubectl-with-an-azure-dev-space"></a>Utilizar kubectl com um espaço do programador do Azure

Pode aceder ao cluster Kubernetes dentro de um espaço do programador do Azure e como utilizar ferramentas de Kubernetes existentes `kubectl`.

Executar `az aks use-dev-spaces` comando irá adicionar automaticamente um `kubectl` contexto de configuração para que, pelo que kubectl já deve estar ligada ao cluster do Azure Dev espaços Kubernetes. Exemplos:
- Confirme o contexto atual: `kubectl config current-context`
- Listar todos os contextos disponíveis: `kubectl config get-contexts`. 
- Contexto de alteração: `kubectl config use-context <context-name>`
- Ver o dashboard Kubernetes: executar `kubectl proxy`, em seguida, abra o browser para o endereço que este comando emite (acrescentar `/ui` para o URL para navegar para o dashboard Kubernetes).
- A execução dos serviços de no espaço predefinido de espaços de programador do Azure com o nome de lista *predefinido*: `kubectl get services --namespace=default`

