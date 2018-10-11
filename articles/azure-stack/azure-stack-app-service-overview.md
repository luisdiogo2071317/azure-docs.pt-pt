---
title: 'Descrição geral do serviço de aplicações: o Azure Stack | Documentos da Microsoft'
description: Descrição geral do serviço de aplicações no Azure Stack
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2018
ms.author: sethm
ms.reviewer: anwestg
ms.openlocfilehash: cf2d65e7e2927aee99e533ea0bca0818f3ab98f6
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "49079162"
---
# <a name="app-service-on-azure-stack-overview"></a>Descrição geral do Serviço de Aplicações no Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Serviço de aplicações do Azure no Azure Stack é uma oferta de plataforma-como-serviço (PaaS) do Microsoft Azure está disponível para o Azure Stack. O serviço permite aos seus clientes - internos ou externos - criar a web, API e as funções do Azure aplicativos para qualquer plataforma ou dispositivo. Podem integrar as suas aplicações com aplicações no local e Automatize os processos empresariais. Operadores de nuvem do Azure Stack podem executar aplicações de cliente em totalmente gerido de máquinas virtuais (VMs), com a sua escolha de recursos de mV partilhados ou VMs dedicadas.

Serviço de aplicações do Azure permite-lhe automatizar processos empresariais e de hospedagem APIs da nuvem. Como um serviço integrado único, o serviço de aplicações do Azure permite combinar vários componentes – sites, RESTful APIs e processos de negócios – numa única solução.

## <a name="why-offer-azure-app-service-on-azure-stack"></a>Por que oferece o serviço de aplicações do Azure no Azure Stack?

São a seguir apresentadas algumas funcionalidades-chave e capacidades do App Service:

- **Várias linguagens e arquiteturas**: serviço de aplicações tem suporte de primeira classe para ASP.NET, node. js, Java, PHP e Python. Pode também executar o Windows PowerShell e outros scripts ou executáveis em VMs do App Service.
- **Otimização de DevOps**: configurar a integração contínua e implementação com o GitHub, local Git ou o BitBucket. Promova atualizações através de ambientes de teste. Gerir as suas aplicações no serviço de aplicações com o Azure PowerShell ou a interface de linha de comandos para várias plataformas (CLI).
- **Integração do Visual Studio**: ferramentas dedicadas do Visual Studio simplificam o trabalho de criação e implantação de aplicativos.

## <a name="app-types-in-app-service"></a>Tipos de aplicação no App Service

Serviço de aplicações oferece vários tipos de aplicação, sendo que cada um concebido para alojar uma carga de trabalho específica:

- [Aplicações Web](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) para alojar sites e aplicações web.
- [Aplicações API](https://docs.microsoft.com/azure/app-service-api/app-service-api-apps-why-best-platform) para o alojamento de RESTful APIs.
- Funções do Azure para alojar o evento condicionada por cargas de trabalho sem servidor.

A aplicação word aqui refere-se ao alojamento de recursos dedicado à execução de uma carga de trabalho. Tendo a “aplicação Web” como exemplo, está provavelmente habituado a pensar numa aplicação Web como os recursos de computação e o código de aplicação que em conjunto fornecem a funcionalidade a um browser. Mas, no serviço de aplicações, uma aplicação web é os recursos de computação do Azure Stack fornece para alojar o código da aplicação.

Seu aplicativo pode ser composto por várias aplicações de serviço de aplicações de diferentes tipos. Por exemplo, se a sua aplicação é composta por um front-end da web e de fim de volta uma API RESTful, pode:

- Implementar ambos (front-end e api) numa aplicação Web única
- Implementar o código de front-end numa aplicação Web e o código de back-end numa aplicação API.

   ![Descrição geral do serviço de aplicações com dados de monitorização](media/azure-stack-app-service-overview/image01.png)

## <a name="what-is-an-app-service-plan"></a>O que é um plano do Serviço de Aplicações?

O fornecedor de recursos do serviço de aplicações utiliza o mesmo código que utiliza o serviço de aplicações do Azure. Como resultado, alguns conceitos comuns são que vale a pena que descreve. No serviço de aplicações, o contentor de preços para as aplicações denomina-se o plano do serviço de aplicações. Ele representa o conjunto de máquinas virtuais dedicadas, utilizado para armazenar as suas aplicações. Dentro de uma determinada subscrição, pode ter vários planos de serviço de aplicações.

No Azure, existem funções de trabalho dedicadas e partilhadas. Uma função de trabalho compartilhada suporta o alojamento de alta densidade aplicações de multi-inquilino e não existe apenas um conjunto de funções de trabalho partilhados. Servidores dedicados são utilizados por apenas um inquilino e são fornecidos em três tamanhos: pequeno, médio e grande. As necessidades dos clientes no local sempre não podem ser descritas usando esses termos. No serviço de aplicações no Azure Stack, os administradores do fornecedor de recursos podem definir os escalões de worker que pretendem disponibilizar. Com base nas suas necessidades exclusivas de hospedagem, pode definir vários conjuntos de funções de trabalho compartilhadas ou conjuntos diferentes de funções de trabalho dedicados. Ao utilizar as definições de escalão de worker, podem, em seguida, definir suas próprias SKUs de preços.

## <a name="portal-features"></a>Recursos de portal

Serviço de aplicações no Azure Stack utiliza a mesma interface de Usuário, que utiliza o serviço de aplicações do Azure, o mesmo acontece com o back-end. No entanto, algumas funcionalidades são desativadas e não estão funcionais no Azure Stack. As expectativas específicos do Azure ou serviços que exigem essas funcionalidades não estão atualmente disponíveis no Azure Stack.

## <a name="next-steps"></a>Passos Seguintes

- [Antes de começar com o serviço de aplicações no Azure Stack](azure-stack-app-service-before-you-get-started.md)
- [Instalar o fornecedor de recursos do serviço de aplicações](azure-stack-app-service-deploy.md)

Também pode tentar outros [plataforma como um serviço de serviço (PaaS)](azure-stack-tools-paas-services.md), como o [fornecedor de recursos do SQL Server](azure-stack-sql-resource-provider-deploy.md) e o [fornecedor de recursos do MySQL](azure-stack-mysql-resource-provider-deploy.md).
