---
title: Oferta de serviços no Azure Stack | Documentos da Microsoft
description: Como um operador de cloud, pode oferecer serviços aos seus utilizadores.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2018
ms.author: jeffgilb
ms.reviewer: unknown
ms.openlocfilehash: 754a0002aca52462910abe50825cff0254a7068f
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2019
ms.locfileid: "54244159"
---
# <a name="overview-of-offering-services-in-azure-stack"></a>Descrição geral da oferta de serviços no Azure Stack

*Aplica-se a: Integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

[Microsoft Azure Stack](azure-stack-poc.md) é uma plataforma de cloud híbrida que lhe permite entregar serviços a partir do seu datacenter. Como um fornecedor de serviços, pode oferecer serviços aos seus inquilinos. Dentro de uma empresa ou uma agência do Governo, pode oferecer serviços no local aos seus funcionários. 

Pode oferecer [infraestrutura como serviço](https://azure.microsoft.com/overview/what-is-iaas/) serviços (IaaS), que seus usuários a criar uma infraestrutura de computação a pedido, aprovisionada e gerida a partir do portal de utilizador do Azure Stack.

Também pode implantar [plataforma como serviço](https://azure.microsoft.com/overview/what-is-paas/) serviços (PaaS) para o Azure Stack da Microsoft e outros fornecedores de terceiros 3. Os serviços que pode fornecer incluem, mas não estão limitados a:

- [Adicionar um fornecedor de recursos do Serviço de Aplicações ao Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-overview)

- [Adicionar um fornecedor de recursos do SQL Server para o Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-sql-resource-provider-deploy)

- [Adicionar um fornecedor de recursos do servidor MySQL ao Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-mysql-resource-provider-deploy)


É possível inclusive combinar serviços para integrar e criar soluções complexas para diferentes usuários.

Para fornecer estes serviços para os seus utilizadores, tem de criar [planos, ofertas e quotas](azure-stack-plan-offer-quota-overview.md). Os utilizadores, em seguida, podem subscrever suas ofertas para utilizar os serviços.

## <a name="plan-your-service-offers"></a>Planear as suas ofertas de serviço

Quando estiver planejando suas ofertas, mantenha os seguintes pontos em mente:

**Ofertas de avaliação**: Pode usar as ofertas de avaliação de atrair novos utilizadores, que, em seguida, podem atualizar para serviços adicionais. Para criar uma oferta de avaliação, crie um pequeno [plano base](azure-stack-plan-offer-quota-overview.md#base-plan) com um plano de suplemento maior opcional.

**Planeamento da capacidade**: Pode estar preocupado com os utilizadores que obter grandes quantidades de recursos e atrapalhando o sistema para todos os utilizadores. Para ajudar a desempenho, pode [configurar seus planos com quotas](azure-stack-plan-offer-quota-overview.md#plans) para utilização de extremidade.

**Fornecedores de delegado**: Pode conceder a outros oferece a capacidade de criar no seu ambiente. Por exemplo, se for um fornecedor de serviços, pode [delegar](azure-stack-delegated-provider.md) essa capacidade de seus revendedores. Em alternativa, se for uma organização, pode delegar a outras divisões/subsidiárias.

## <a name="next-steps"></a>Passos Seguintes

[Criar uma oferta no Azure Stack](azure-stack-create-offer.md)
