---
title: Guia para criar um modelo de solução para o Marketplace | Documentos da Microsoft
description: Instruções detalhadas sobre como criar, certificar e implementar um modelo de solução de imagem de várias VMS para fazer compras no Azure Marketplace.
services: marketplace-publishing
documentationcenter: ''
author: v-miclar
manager: hascipio
editor: ''
ms.assetid: e14e05f2-2385-4ce0-b351-0747cb74ba19
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2016
ms.author: hascipio; v-divte
ROBOTS: NOINDEX
ms.openlocfilehash: 914dece4ba064af00c5b2c34d43dedbb1d62e2e2
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54073746"
---
# <a name="guide-to-create-a-solution-template-for-azure-marketplace"></a>Guia para criar um modelo de solução para o Azure Marketplace
Depois de concluir o passo 1, [criação e registo de contas][link-acct-creation], podemos orientada da criação de um modelo de solução compatível com o Azure em [pré-requisitos técnicos para a criação de um modelo de solução](marketplace-publishing-solution-template-creation-prerequisites.md). Agora podemos irá guiá-lo através dos passos para criar um modelo de solução para várias VMs no [Portal de publicação] [ link-pubportal] para o Azure Marketplace.

## <a name="create-your-solution-template-offer-in-the-publishing-portal"></a>Criar a sua oferta de modelo de solução no Portal de publicação
Aceda a [ https://publish.windowsazure.com ](http://publish.windowsazure.com). Ao iniciar sessão pela primeira vez para o [Portal de publicação](https://publish.windowsazure.com/), utilize a mesma conta com que foi registado o perfil do vendedor da sua empresa. Mais tarde, pode adicionar qualquer funcionário da sua empresa como um coadmin no Portal de publicação.

### <a name="1-select-solution-templates"></a>1. Selecione "Modelos de soluções"
  ![desenho][img-pubportal-menu-sol-templ]

### <a name="2-create-a-new-solution-template"></a>2. Criar um novo modelo de solução
  ![desenho][img-pubportal-sol-templ-new]

### <a name="3-start-with-topologies"></a>3. Iniciar com as topologias
Um modelo de solução é um "elemento principal" para todas as suas topologias. Pode definir vários topologias num modelo de oferta/solução. Quando é emitida uma oferta para teste, esta é instalada com todas as respetivas topologias. Siga os passos abaixo para definir a sua oferta:     

* Crie uma topologia: "Identificador de topologia" normalmente é o nome da topologia para o modelo de solução. O identificador de topologia é utilizado no URL, conforme mostrado abaixo:

  O Azure Marketplace: http://azure.microsoft.com/marketplace/partners/{PublisherNamespace}/{OfferIdentifier}{TopologyIdentifier}

  Portal do Azure: https://portal.azure.com/#gallery/{PublisherNamespace}.{OfferIdentifier}{TopologyIdentifier}
* Adicione uma nova versão.

### <a name="4-get-your-topology-versions-certified"></a>4. Obtenha as versões de topologia certificadas
Carrega um ficheiro zip que contém todos os ficheiros necessários para aprovisionar essa versão específica da topologia. Este ficheiro zip tem de conter os seguintes ficheiros:

* *maintemplate. JSON* e *Createuidefinition* ficheiro no seu diretório de raiz.
* Quaisquer modelos ligados e os scripts necessários tudo.

  > [!TIP]
  > Enquanto os seus programadores funcionam em criar a solução topologias de modelo e obter certificá-los, a empresa, os departamentos de marketing e/ou legais da sua empresa podem trabalhar no conteúdo de marketing e legal.
  >
  >

## <a name="next-steps"></a>Passos Seguintes
Agora que criou o seu modelo de solução e carregou o ficheiro zip, siga as instruções no [guia de conteúdo de marketing de Marketplace](marketplace-publishing-push-to-staging.md) antes de enviar a oferta para teste. Para ver o conjunto completo de artigos de publicação do marketplace, visite [de introdução: Como publicar uma oferta no Azure Marketplace](marketplace-publishing-getting-started.md).

Poderá também estar interessado nestes artigos relacionados:

* Imagens de VM: [Sobre imagens de máquinas virtuais no Azure](https://msdn.microsoft.com/library/azure/dn790290.aspx)
* Extensões de VM: [Extensões VM do Azure e funcionalidades](../virtual-machines/extensions/features-windows.md)
* O Azure Resource Manager: [Criação de modelos Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) e [exemplos do modelos simples](https://github.com/rjmax/ArmExamples)
* Limitações de conta de armazenamento: [Como monitorizar para otimização de conta de armazenamento](https://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) e [o armazenamento Premium](../virtual-machines/windows/premium-storage.md#scalability-and-performance-targets)

[img-pubportal-menu-sol-templ]:media/marketplace-publishing-solution-template-creation/pubportal-menu-solution-templates.png
[img-pubportal-sol-templ-new]:media/marketplace-publishing-solution-template-creation/pubportal-solution-template-new.png
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-pubportal]:https://publish.windowsazure.com
