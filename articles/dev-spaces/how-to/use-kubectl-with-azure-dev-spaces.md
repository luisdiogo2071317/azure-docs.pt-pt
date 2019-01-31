---
title: Como utilizar o kubectl com espaços de desenvolvimento do Azure | Documentos da Microsoft
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
author: zr-msft
ms.author: zarhoads
ms.date: 05/11/2018
ms.topic: article
description: Desenvolvimento rápido da Kubernetes com contentores e microsserviços no Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contentores
ms.openlocfilehash: b0ceccc4f8b16c21e8a66a5f1b8cf0e7b6f47a4f
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55469316"
---
# <a name="use-kubectl-with-an-azure-dev-space"></a>Utilizar o kubectl com um espaço de desenvolvimento do Azure

Pode aceder ao cluster de Kubernetes dentro de um espaço de desenvolvimento do Azure e utilizar ferramentas do Kubernetes existentes como `kubectl`.

Em execução `az aks use-dev-spaces` comando irá adicionar automaticamente um `kubectl` contexto de configuração para si, para que o kubectl já deverá estar ligado ao seu cluster de Kubernetes de espaços de desenvolvimento do Azure. Exemplos:
- Confirme o contexto atual: `kubectl config current-context`
- Lista de todos os contextos disponíveis: `kubectl config get-contexts`. 
- Alterar o contexto: `kubectl config use-context <context-name>`
- Ver o dashboard do Kubernetes: executar `kubectl proxy`, em seguida, abra o browser para o endereço que este comando emite (acrescentar `/ui` para o URL para navegar para o dashboard do Kubernetes).
- Lista de serviços em execução no espaço de espaços de desenvolvimento do Azure de padrão com o nome *predefinição*: `kubectl get services --namespace=default`

