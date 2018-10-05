---
title: FAQ do Marketplace de módulo do IoT Edge | Documentos da Microsoft
description: FAQ do Marketplace módulo do IoT Edge.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/19/2018
ms.author: pbutlerm
ms.openlocfilehash: 2c2e7729eb70a5dd37dc4df10605eec9429e1043
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48810368"
---
# <a name="iot-edge-module-marketplace-frequently-asked-questions"></a>Marketplace de módulo do IoT Edge perguntas mais frequentes


## <a name="what-is-the-edge-module-marketplace"></a>O que é o mercado de módulo Edge?


O mercado de módulo do IoT Edge é uma lista de *certified* previamente criados módulos de extremidade que são *detetável* através do Azure Marketplace.

![Módulos de IoT Edge](./media/cloud-partner-portal-iot-edge-module-faq/iot-edge-modules.png)

## <a name="where-will-edge-modules-be-visible"></a>Em que os módulos do Edge serão visíveis? 


Na [do Azure marketplace portal](https://ms.portal.azure.com/) (experiência de autenticação), na categoria de Internet das coisas, marcado como "Módulo do IoT Edge".

E, no [Web site do Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/internet-of-things?page=1) (experiência de anónima) sob a categoria de Internet das coisas, marcado como "Módulo do IoT Edge".

## <a name="is-it-open-to-partners"></a>É aberta a parceiros?


Ainda não. Atualmente, só os módulos criados pela Microsoft são publicados na seção do IoT Edge do marketplace. 

## <a name="why-should-partners-publish-their-iot-edge-modules"></a>Por que os parceiros devem publicar seus módulos do IoT Edge?


-   Para aumentar sua visibilidade do produto ao adicionar este canal de comercialização e que mostra suas soluções.

-   Para obter oportunidades potenciais mais. Como parte do Azure Marketplace, podem obter detalhes sobre quem está interessado em sua solução.

-   Seja entre os primeiros a tirar partido das capacidades de monetização mais.

## <a name="what-is-the-onboarding-process"></a>O que é o processo de integração?


Embora não ainda aberto, o processo de integração será efetuado através do Azure Marketplace. Diretrizes detalhadas estão no [guia de publicação do Azure Marketplace e AppSource](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide). Consulte transact listagem tipo para contentores. 

Parceiros terá primeiro de se tornar um publicador para o Azure Marketplace. Em seguida, eles poderão para integrar seus módulos do Edge através da [Cloud Partner Portal](./cloud-partner-portal-getting-started-with-the-cloud-partner-portal.md).

## <a name="are-there-any-monetization-capabilities"></a>Existem quaisquer capacidades de monetização?


Não prontos a utilizar por agora. Nossa primeira meta é abrir um mercado com *gratuita* ou *traga a sua própria licença* módulos de extremidade. Estamos a adicionar mais capacidades de monetização, como um modelo de faturação do consumo.

## <a name="what-is-bring-your-own-license-byol"></a>O que é bring-your-própria licença (BYOL)?


A definição oficial no [políticas de participação do Microsoft Azure Marketplace](https://azure.microsoft.com/support/legal/marketplace/participation-policies/) é:

- *Os clientes obter fora do Azure Marketplace, o direito a aceder ou utilizar a oferta e não são cobrados do Azure markeplace pela utilização da oferta no Azure Marketplace.*

Cabe-lhe para parceiros de licenciamento do seu software e recolher as receitas para ele.

## <a name="can-partners-publish-a-freemium-module"></a>Parceiros de publicar um módulo de "freemium"?


Sim freemium módulos, que são módulos disponíveis gratuitamente, mas com algumas restrições, serão considerados como qualquer outro tipo de publicação.

## <a name="is-intellectual-property-protected"></a>Propriedade intelectual está protegida?


Um módulo Edge é um contentor do Docker compatível. Cabe-lhe para parceiros para proteger sua propriedade intelectual (IP) empacotada nos contentores distribuídos.

## <a name="where-will-the-modules-be-hosted"></a>Onde serão alojados os módulos?


Módulos do IoT Edge vão ser alojados num registo de contentor pertencentes à Microsoft que poderão anonimamente consulta-, como o Docker Hub.

## <a name="what-are-the-integration-plans-between-the-azure-marketplace-and-the-azure-iot-tools"></a>Quais são os planos de integração entre o Azure Marketplace e as ferramentas do Azure IoT?

Vamos criar uma integração maior entre o Azure Marketplace e as ferramentas do Azure IoT. São exemplos de como o que temos em mente para ativar a algumas opções de navegação do marketplace de módulo do IoT Edge diretamente a partir do portal do IoT Hub ou diretamente a partir do Visual Studio Code.

## <a name="which-operating-system-or-architecture-should-my-container-support"></a>Qual sistema operacional ou a arquitetura deve suportar a meu contêiner?

Módulos do IoT Edge tem de suportar o mesmo sistema operacional / matriz de arquitetura como o IoT Edge em produção. Esta lista é mantida na [suporte do Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/support). Por exemplo, um módulo atualmente tem suporte Linux x64 e ARM32 de Linux.

## <a name="are-there-any-other-certification-constraints-to-be-aware-of"></a>Existem outras limitações de certificação de conhecer?

Estamos ainda a trabalhar no conjunto exato de restrições de certificação. Nossos princípios básicos são:

-   Promover a qualidade com quantidade.

-   Contentores de específicos do IoT Edge (aqueles não aleatórios).

-   Nível de produção.

-   implementação 1 Clique (no mínimo, com um conjunto de predefinição valores de configuração fornecido).

## <a name="any-other-questions"></a>Outras perguntas?


Contacte [do Azure IoT Edge integração](mailto:azureiotedgeonboarding@service.microsoft.com).
