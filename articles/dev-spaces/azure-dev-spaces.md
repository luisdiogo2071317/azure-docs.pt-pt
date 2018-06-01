---
title: Espaços de Programador do Azure | Microsoft Docs
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: tutorial
description: Desenvolvimento rápido da Kubernetes com contentores e microsserviços no Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contentores
manager: douge
ms.openlocfilehash: 344947b7906d15e819e372e0affe4af3c34ba69b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
ms.locfileid: "34198763"
---
# <a name="azure-dev-spaces"></a>Espaços de Programador do Azure
Os Espaços de Programador do Azure ajudam-no a desenvolver rapidamente com a Kubernetes. Com os Espaços de Programador do Azure, adiciona também capacidades de desenvolvimento completas, como depuração para os contentores de Kubernetes do Azure e pode desenvolver de maneira iterativa os contentores na cloud com ferramentas familiares, como o VS Code, o Visual Studio ou a linha de comandos. Os Espaços de Programador do Azure são especialmente relevantes no desenvolvimento de equipas em que o isolamento de ramificações de código individuais nos seus próprios espaços é uma parte crucial do ciclo de vida de desenvolvimento.

## <a name="how-azure-dev-spaces-simplifies-kubernetes-development"></a>Como os Espaços de Programador do Azure simplificam o desenvolvimento da Kubernetes 

Esta abordagem acarreta várias vantagens:

* Obter um ambiente de desenvolvimento de infraestrutura que seja representativo da produção, com acesso total a recursos na cloud.
* Depurar os contentores Node.js e .NET Core diretamente na Kubernetes com o VS Code ou o Visual Studio. Todos os outros idiomas podem ser desenvolvidos com a interface de linha de comandos.
* Partilhe uma instância da Kubernetes na sua equipa de desenvolvimento para reduzir os custos e minimizar a configuração do computador local para novos membros da equipa.
* Desenvolver o seu código em isolamento e fazer um teste ponto-a-ponto com outros componentes, sem réplicas de dependências ou fictícias.

[!INCLUDE[](includes/get-started.md)]

![](media/azure-dev-spaces/vscode-overview.png)