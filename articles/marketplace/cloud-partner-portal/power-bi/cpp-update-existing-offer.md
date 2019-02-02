---
title: Atualizar uma oferta de aplicação do Power BI existente - Azure Marketplace | Documentos da Microsoft
description: Atualize uma oferta de aplicação do Power BI depois de ter sido publicado no Microsoft AppSource Marketplace.
services: Azure, AppSource, Marketplace, Cloud Partner Portal, Power BI
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: pbutlerm
ms.openlocfilehash: 635e2e71bb952aaee761df6a1d5d87c46db531f6
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/02/2019
ms.locfileid: "55666710"
---
# <a name="update-an-existing-power-bi-app-offer"></a>Atualizar uma oferta de aplicação do Power BI existente

Este artigo explica-lhe os diferentes aspectos da atualizar a sua oferta de aplicação do Power BI no [Cloud Partner Portal](https://cloudpartner.azure.com/) e, em seguida, republicar a oferta.  Existem motivos comuns para atualizar a sua oferta, incluindo:

- A atualizar o conteúdo da aplicação no Power BI e obter um URL de instalar novo de aplicação embalada recentemente
- A atualizar os metadados para a oferta do marketplace: vendas, marketing, ou informações de suporte e recursos
 
Para ajudá-lo a essas modificações, o portal oferece-a **Compare** e **histórico** funcionalidades.


## <a name="unpermitted-changes-to-offer"></a>Alterações unpermitted para oferecer

Existem alguns atributos de uma oferta de aplicação do Power BI não pode ser modificado depois da oferta está em direto no AppSource, principalmente **ID da oferta** e **ID de publicador**.


## <a name="common-update-operations"></a>Operações de atualização comuns

Embora haja uma grande variedade de características que é possível alterar uma oferta de aplicação do Power BI, as seguintes operações são comuns.


### <a name="update-app-content-in-power-bi"></a>Atualizar o conteúdo da aplicação no Power BI

É comum para a aplicação no Power BI para ser atualizado periodicamente com novo conteúdo, patches de segurança, funcionalidades adicionais e assim por diante. Em tais cenários, que pretende atualizar o URL para a nova instalação de conteúdo de aplicações com os seguintes passos:

1.  Inicie sessão para o [Portal de parceiros da Cloud](https://cloudpartner.azure.com/).
2.  Sob **todas as ofertas**, localize a oferta de atualização.
3.  Na **informações técnicas** separador, introduza um novo URL de instalador.
4.  Clique em **publicar** para iniciar o fluxo de trabalho para publicar a versão de sua nova aplicação para o AppSource.


### <a name="update-offer-marketplace-metadata"></a>Atualizar os metadados do marketplace de oferta

Utilize os seguintes passos para atualizar os metadados de marketplace — da empresa nome, logótipos, etc. — associadas a sua oferta:

1.  Inicie sessão para o [Portal de parceiros da Cloud](https://cloudpartner.azure.com/).
2.  Sob **todas as ofertas**, localize a oferta que pretende atualizar.
3.  Goto a **detalhes de Vitrine** separador, em seguida, siga as instruções no [guia detalhes de loja de aplicações do Power BI](./cpp-storefront-details-tab.md) para fazer alterações de metadados.
4.  Clique em **publicar** para iniciar o fluxo de trabalho para publicar as alterações.


## <a name="compare-feature"></a>Compare recursos

Quando fizer alterações numa oferta publicada já, pode utilizar o **comparar** funcionalidade para auditar as alterações que foram feitas. Para utilizar esta funcionalidade:

1.  Em qualquer momento no processo de edição, clique nas **comparar** botão para a sua oferta.

    ![Comparar o botão de funcionalidade](./media/compare-feature-button.png)

2.  Ver versões lado a lado dos ativos de marketing e metadados.


## <a name="history-of-publishing-actions"></a>Histórico de ações de publicação

Para ver qualquer atividade de publicação histórica, clique nas **histórico** separador na barra de menus de navegação à esquerda do Portal de parceiro de Cloud. Aqui, poderá ver timestamped ações que foram executadas durante o tempo de vida das suas ofertas do AppSource.


## <a name="next-steps"></a>Passos Seguintes

Regularmente deve utilizar o [Insights vendedor](../../cloud-partner-portal-orig/si-getting-started.md) recurso do [Cloud Partner Portal](https://cloudpartner.azure.com/#insights) para fornecer informações sobre os clientes do marketplace e o uso.  
