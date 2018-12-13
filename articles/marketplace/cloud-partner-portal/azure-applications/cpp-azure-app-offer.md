---
title: Oferta de aplicação do Azure | Documentos da Microsoft
description: Descrição geral do processo de publicação de uma aplicação do Azure oferecem no Azure Marketplace.
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
ms.date: 12/07/2018
ms.author: pbutlerm
ms.openlocfilehash: 63b7ee4e0d9cb9d0d1f26119fe73573b124d04e8
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/10/2018
ms.locfileid: "53197339"
---
# <a name="azure-application-offer"></a>Oferta de aplicação do Azure

Esta secção explica como publicar uma nova oferta de aplicação do Azure da Microsoft <a href="https://azuremarketplace.microsoft.com">do Azure Marketplace</a>.
|    |    |
|-----------------------------------------------------------------|------------------------------------------|
| Cada aplicação do Azure contém um modelo do Azure Resource Manager que define todos os recursos técnicos usados pelo aplicativo, que normalmente inclui uma ou mais máquinas virtuais e outros serviços de suporte do Azure - ou baseado na Web. | ![Ícone de aplicações do Azure](./media/azureapp-icon1.png)  |

## <a name="benefits"></a>Benefícios

Alguns dos benefícios da listagem de seus aplicativos num mercado da Microsoft incluem:

* Atingir 100 milhões de utilizadores do Azure Active Directory no Office 365 e Dynamics 365.

* Expandir a sua equipa de vendas: alcançar utilizadores empresariais em todo o mundo e obter um canal de vendas que interage com os utilizadores finais, ajuda a gerar oportunidades potenciais e inicia conversas com os novos clientes em vários setores.

* A obter informações acionáveis: partilharemos informações sobre como a execução da sua aplicação no AppSource, o que funciona bem e como melhorar ainda mais os seus procedimentos de vendas.

## <a name="types-of-azure-applications"></a>Tipos de aplicações do Azure

Existem dois tipos de aplicações do Azure: uma aplicação gerida e um modelo de solução. Embora seja semelhante, existem algumas diferenças notáveis.

### <a name="solution-template"></a>Modelo de solução

Modelos de soluções são uma das principais formas que publicar uma solução no mercado. Este tipo de oferta é utilizado quando a sua solução requer implementação adicional e automatização de configuração para além de uma única máquina virtual (VM). Pode automatizar o fornecimento de mais do que uma VM com um modelo de solução. Isto inclui o aprovisionamento de recursos de rede e armazenamento para fornecer soluções de IaaS complexas. Para uma descrição geral dos requisitos de modelo de solução e o modelo de faturação, consulte [aplicações do Azure: modelos de soluções](https://docs.microsoft.com/azure/marketplace/marketplace-solution-templates).

### <a name="managed-application"></a>Aplicação gerida

As aplicações geridas são semelhantes aos modelos de soluções no Marketplace, com uma diferença essencial. Numa aplicação gerida, os recursos são implementados num grupo de recursos que é gerido pelo editor da aplicação. O grupo de recursos está presente na subscrição do cliente, mas uma identidade no inquilino do editor tem acesso ao grupo de recursos. Como editor, tem de especificar o custo do suporte contínuo da solução. Utilize as aplicações geridas do Azure para facilmente criar e fornecer aplicações chave na mão totalmente geridas para os seus clientes.

Para além do Marketplace, também pode oferecer aplicativos gerenciados num catálogo de serviços. O catálogo de serviços é um catálogo interno de soluções aprovadas para os utilizadores de uma organização. Utilizar o catálogo para atender aos padrões organizacionais, oferecendo soluções para grupos de uma organização. Os funcionários utilizam o catálogo para localizar facilmente aplicações que são recomendadas e aprovadas pelos departamentos de TI.

Para obter mais informações sobre as vantagens e os tipos de aplicativos gerenciados, consulte a [descrição geral de aplicativos de geridas do Azure](https://docs.microsoft.com/azure/managed-applications/overview).

## <a name="publishing-overview"></a>Descrição geral de publicação

O vídeo seguinte, [criando modelos de soluções e aplicações geridas para o Azure Marketplace](https://channel9.msdn.com/Events/Build/2018/BRK3603), é uma descrição geral sobre como criar um modelo do Azure Resource Manager para definir uma solução de aplicação do Azure e, em seguida, como em seguida, publicar a oferta da aplicação no Azure Marketplace.

>[!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK3603/player]

## <a name="publishing-process-workflow"></a>Fluxo de trabalho de processo publicação

O diagrama seguinte mostra o processo de alto nível para publicar uma oferta de aplicação do Azure.

![Fluxo de trabalho para a publicação da oferta](./media/new-offer-process.png)

## <a name="offer-components"></a>Componentes de oferta

Esta secção descreve os elementos de publicar uma oferta de aplicação gerida e destina-se como um guia para o publicador para o Azure Marketplace. A publicação dividido nas seguintes partes principais: 

* [Pré-requisitos](./cpp-prerequisites.md) -lista os requisitos técnicos e empresariais antes de criar ou publicar uma oferta de aplicação gerida. 
* [Criar a oferta](./cpp-create-offer.md) -fornece os passos necessários para criar uma entrada de oferta de aplicação gerida com o Portal de parceiro de Cloud. 
* [Publicar a oferta](./cpp-publish-offer.md)-descreve como submeter a oferta para publicação no Azure Marketplace.

## <a name="steps-in-the-publishing-process"></a>Passos no processo de publicação

Os passos de alto nível para publicar uma oferta de aplicação do Azure são:

1. Criar a oferta - fornecem informações detalhadas sobre a oferta. Estas informações incluem: a descrição da oferta, as especificações dos ativos, informações de suporte e materiais de marketing.

2. Criar os recursos técnicos e comerciais - criar os ativos empresariais (documentos legais e materiais de marketing) e ativos técnicos para a solução associada.

3. Criar o SKU - criar o SKU (s) associado com a oferta. Um SKU exclusivo é necessário para cada imagem que pretende publicar.

4. Certificar e publicar a oferta - depois da oferta e os ativos técnicos são concluídos, pode enviar a oferta. Esta submissão inicia o processo de publicação. Durante este processo, a solução é testada, validado, certified, em seguida, "entrar no ar" no Azure Marketplace.

## <a name="next-steps"></a>Passos Seguintes

Antes de considerar estes passos, tem de cumprir os [requisitos técnicos e empresariais](./cpp-prerequisites.md) para publicar uma aplicação gerida no Microsoft Azure Marketplace.