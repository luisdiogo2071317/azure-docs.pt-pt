---
title: Atualizar uma oferta existente para o Azure Marketplace
description: Atualizar uma oferta existente para o Azure Marketplace
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: qianw211
manager: pbutlerm
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 5633392bdf1293ee9196fafe67cf901e0d8c8014
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48810408"
---
<a name="update-an-existing-offer-for-azure-marketplace"></a>Atualizar uma oferta existente para o Azure Marketplace 
==============================================

Existem vários tipos de atualizações que pode querer fazer sua oferta depois de entrar no ar.

1.  A adicionar novas \"pacote\" para um SKU existente
2.  Adicionar novos SKUs para uma oferta existente
3.  A atualizar os metadados do marketplace de oferta/SKU

Terá de atualizar a sua oferta no Portal de parceiros de nuvem e voltar a publicar. Este artigo explica os diferentes aspectos da atualizar a sua oferta de aplicação do Azure.

<a name="unpermitted-changes-to-azure-application-offersku"></a>Unpermitted alterações à oferta/SKU da aplicação do Azure 
--------------------------------------------------

Existem atributos de um oferta/SKU da aplicação do Azure que não podem ser modificadas depois da oferta for executada no Azure Marketplace.

1.  ID de oferta e o ID de publicador da oferta.
2.  ID de SKU de SKUs existentes.
3.  Atualize um pacote que tenha sido publicado.

<a name="adding-a-new-package-to-an-existing-sku"></a>Adicionar um novo pacote para um SKU existente 
---------------------------------------

O publicador pode desejar adicionar uma nova versão do pacote para atualizar um pacote existente. Isso poderia ser feito através do carregamento de um novo pacote com um número de versão diferentes.

1.  Entrar para a [Portal de parceiros da Cloud](http://cloudpartner.azure.com)
2.  Em todas as ofertas encontrar a oferta que pretende atualizar
3.  No formulário de SKUs, clique no SKU que\'pacote s tinha pretender atualizar
4.  Clique em \"novo pacote\" e fornecer uma nova versão
5.  Carregar um novo arquivo. zip que contém o ficheiro de modelo atualizado
6.  Clique em publicar para iniciar o fluxo de trabalho de publicar para que o seu novo SKU ativadas.

<a name="adding-a-new-sku-to-an-existing-offer"></a>Adicionar um novo SKU para uma oferta existente
-------------------------------------

Pode optar por disponibilizar um SKU de novo para a sua oferta existente. Para ativar esta opção, siga os passos abaixo.

1.  Início de sessão para o [Portal de parceiros da Cloud](http://cloudpartner.azure.com)
2.  Em todas as ofertas encontrar a oferta que pretende atualizar
3.  Sobre os SKUs de formulário, clik em Adicionar novo SKU e forneça um ID de SKU no pop-up.
4.  Siga o resto dos passos conforme especificado [aqui](./cloud-partner-portal-managed-app-publish.md).
5.  Clique em publicar para iniciar o fluxo de trabalho de publicar para que o seu novo SKU ativadas.

<a name="updating-offer-marketplace-metadata"></a>Atualizar os metadados de oferecer o Marketplace 
-----------------------------------

Pode haver situações em que precisa atualizar os metadados de marketplace associados a sua oferta, como atualizar a logótipos da empresa, entre outras. Siga os passos abaixo.

1.  Inicie sessão no Portal de parceiros de nuvem
2.  Em todas as ofertas encontrar a oferta que pretende atualizar
3.  Goto Marketplace formam e siga as instruções [aqui](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-push-to-staging) para efetuar quaisquer alterações.
4.  Clique em publicar para iniciar o fluxo de trabalho de publicar para que as suas alterações, aceda ao vivo.
