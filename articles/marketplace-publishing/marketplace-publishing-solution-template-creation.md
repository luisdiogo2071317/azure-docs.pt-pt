---
title: Guia para criar um modelo de solução para o Marketplace | Microsoft Docs
description: Instruções detalhadas sobre como criar, certificar e implementar um modelo de solução da imagem de várias VMS para comprar no Azure Marketplace.
services: marketplace-publishing
documentationcenter: ''
author: msmbaldwin
manager: mbaldwin
editor: ''
ms.assetid: e14e05f2-2385-4ce0-b351-0747cb74ba19
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2016
ms.author: mbaldwin
ms.openlocfilehash: 83e69701aa5e8c11354e7551d66f3fdd18289000
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
ms.locfileid: "29937313"
---
# <a name="guide-to-create-a-solution-template-for-azure-marketplace"></a>Guia para criar um modelo de solução para o Azure Marketplace
Depois de concluir o passo 1, [conta criação e registo][link-acct-creation], iremos orientado na criação de um modelo de solução do Azure compatível em [pré-requisitos técnicos para criar um modelo de solução](marketplace-publishing-solution-template-creation-prerequisites.md). Agora iremos irá guiá-lo através dos passos para criar um modelo de solução para várias VMs no [Portal publicação] [ link-pubportal] para o Azure Marketplace.

## <a name="create-your-solution-template-offer-in-the-publishing-portal"></a>Criar a sua oferta do modelo de solução no Portal de publicação
Aceda a [ https://publish.windowsazure.com ](http://publish.windowsazure.com). Quando iniciar sessão pela primeira vez para o [Portal publicação](https://publish.windowsazure.com/), utilize a mesma conta com que foi registado o perfil de vendedor da sua empresa. Mais tarde, pode adicionar qualquer empregado da sua empresa como coadministrador no Portal de publicação.

### <a name="1-select-solution-templates"></a>1. Selecione "Modelos de solução"
  ![desenho][img-pubportal-menu-sol-templ]

### <a name="2-create-a-new-solution-template"></a>2. Criar um novo modelo de solução
  ![desenho][img-pubportal-sol-templ-new]

### <a name="3-start-with-topologies"></a>3. Começar com as topologias
Um modelo de solução é um "elemento principal" para todas as suas topologias. Pode definir vários topologias num modelo de oferta/solução. Quando é emitida uma oferta para teste, esta é instalada com todas as respetivas topologias. Siga os passos abaixo para definir a sua oferta:     

* Cria uma topologia: "Identificador de topologia" é, geralmente, o nome da topologia para o modelo de solução. O identificador de topologia é utilizado no URL, como mostrado abaixo:

  Azure Marketplace: http://azure.microsoft.com/marketplace/partners/{PublisherNamespace}/{OfferIdentifier}{TopologyIdentifier}

  Azure Portal: https://portal.azure.com/#gallery/{PublisherNamespace}.{OfferIdentifier}{TopologyIdentifier}
* Adicione uma nova versão.

### <a name="4-get-your-topology-versions-certified"></a>4. Obter as versões de topologia certificadas
Carregar um ficheiro zip que contém todos os ficheiros necessários para aprovisionar essa versão específica da topologia. Este ficheiro zip tem de conter o seguinte:

* *mainTemplate.json* e *createUiDefinition.json* ficheiro no seu diretório de raiz.
* Quaisquer modelos ligados e scripts todos requeridos.

  > [!TIP]
  > Enquanto os programadores de trabalho sobre como criar a solução de topologias de modelo e obtê-las certificadas, negócio, departamentos de marketing e/ou legais da sua empresa podem trabalhar no conteúdo de marketing e legal.
  >
  >

## <a name="next-steps"></a>Passos Seguintes
Agora que criou o seu modelo de solução e carregado o ficheiro zip, siga as instruções de [guia conteúdo de marketing Marketplace](marketplace-publishing-push-to-staging.md) antes de enviar a oferta para teste. Para ver o conjunto completo de marketplace publicação artigos, visite [introdução: como publicar uma oferta no Azure Marketplace](marketplace-publishing-getting-started.md).

Poderá também estar interessado nestes artigos relacionados:

* Imagens VM: [sobre imagens de Máquina Virtual no Azure](https://msdn.microsoft.com/library/azure/dn790290.aspx)
* Extensões VM: [agente da VM e descrição geral de extensões de VM](https://msdn.microsoft.com/library/azure/dn832621.aspx) e [extensões de VM do Azure e funcionalidades](https://msdn.microsoft.com/library/azure/dn606311.aspx)
* O Azure Resource Manager: [criação de modelos Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) e [exemplos de modelo simples](https://github.com/rjmax/ArmExamples)
* Conta de armazenamento acelera: [como monitorizar para limitação de conta de armazenamento de](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) e [armazenamento Premium](../virtual-machines/windows/premium-storage.md#scalability-and-performance-targets)

[img-pubportal-menu-sol-templ]:media/marketplace-publishing-solution-template-creation/pubportal-menu-solution-templates.png
[img-pubportal-sol-templ-new]:media/marketplace-publishing-solution-template-creation/pubportal-solution-template-new.png
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-pubportal]:https://publish.windowsazure.com
