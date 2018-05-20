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
description: Desenvolvimento rápido Kubernetes com contentores e micro-serviços no Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes serviço, contentores
manager: douge
ms.openlocfilehash: 38a433a14ab977fb56a8331a057d27241f1d9783
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="use-kubectl-with-an-azure-dev-space"></a>Utilizar kubectl com um espaço do programador do Azure

Pode aceder ao cluster Kubernetes dentro de um espaço do programador do Azure e como utilizar ferramentas de Kubernetes existentes `kubectl`.

Executar `azds resource create` ou `azds resource select` irá adicionar automaticamente um `kubectl` contexto de configuração para que, pelo que kubectl já deve estar ligada ao cluster do Azure Dev espaços Kubernetes. Exemplos:
- Confirme o contexto atual: `kubectl config current-context`
- Listar todos os contextos disponíveis: `kubectl config get-contexts`. 
- Contexto de alteração: `kubectl config use-context <context-name>`
- Ver o dashboard Kubernetes: executar `kubectl proxy`, em seguida, abra o browser para o endereço que este comando emite (acrescentar `/ui` para o URL para navegar para o dashboard Kubernetes).
- A execução dos serviços de no espaço predefinido de espaços de programador do Azure com o nome de lista *predefinido*: `kubectl get services --namespace=default`

