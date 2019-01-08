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
ms.openlocfilehash: 2bfa10441cf5531c9383527a21b033da26322b34
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54073243"
---
<a name="update-an-existing-offer-for-azure-marketplace"></a>Atualizar uma oferta existente para o Azure Marketplace 
==============================================

Existem vários tipos de atualizações que pode querer fazer sua oferta depois de entrar no ar, por exemplo:

1.  A adicionar novas \"pacote\" para um SKU existente
2.  Adicionar novos SKUs para uma oferta existente
3.  A atualizar os metadados do marketplace de oferta/SKU

Irá utilizar o Portal de parceiro de Cloud para implementar essas alterações, em seguida, voltar a publicar a oferta. Este artigo explica os diferentes aspectos da atualizar a sua oferta de aplicação do Azure.

<a name="unpermitted-changes-to-azure-application-offer"></a>Unpermitted alterações à oferta de aplicação do Azure 
-----------------------------------------------

Existem atributos de um oferta/SKU da aplicação do Azure que não podem ser modificadas depois da oferta for executada no Azure Marketplace.

* ID de oferta e o ID de publicador da oferta.
* ID de SKU de SKUs existentes.
* Atualize um pacote que tenha sido publicado.

<a name="adding-a-new-package-to-an-existing-sku"></a>Adicionar um novo pacote para um SKU existente 
---------------------------------------

O publicador pode desejar adicionar uma nova versão do pacote para atualizar um pacote existente. Esta adição pode ser feita através do carregamento de um novo pacote com um número de versão diferentes.

1.  Entrar para a [Portal de parceiros da Cloud](http://cloudpartner.azure.com)
2.  Na **todas as ofertas**, localize a oferta que pretende atualizar
3.  Sobre o **SKUs** de formulário, clique na SKU que\'pacote s tinha pretender atualizar
4.  Clique em \"novo pacote\" e fornecer uma nova versão
5.  Carregar um novo arquivo. zip que contém o ficheiro de modelo atualizado
6.  Clique em publicar para iniciar o fluxo de trabalho de publicar para que o seu novo SKU ativadas.

<a name="adding-a-new-sku-to-an-existing-offer"></a>Adicionar um novo SKU para uma oferta existente
-------------------------------------

Pode optar por disponibilizar um SKU de novo para a sua oferta existente os seguintes passos:

1.  Inicie sessão para o [Portal de parceiros da Cloud](http://cloudpartner.azure.com)
2.  Na **todas as ofertas**, localize a oferta que pretende atualizar
3.  Sobre o **SKUs** de formulário, clique em Adicionar novo SKU e forneça um ID de SKU no pop-up.
4.  Siga o resto dos passos conforme especificado [aqui](./cloud-partner-portal-managed-app-publish.md).
5.  Clique em **publicar** para iniciar o fluxo de trabalho de publicar para que o seu novo SKU ativadas.

<a name="updating-offer-marketplace-metadata"></a>Atualizar os metadados de oferecer o Marketplace 
-----------------------------------

Pode haver situações em que precisa atualizar os metadados de marketplace associados a sua oferta, como atualizar a logótipos da empresa, entre outras. Siga os passos abaixo.

1.  Inicie sessão no Portal de parceiros de nuvem.
2.  Na **todas as ofertas**, localize a oferta que pretende atualizar
3.  Goto Marketplace formam e siga as instruções [aqui](../cloud-partner-portal/azure-applications/cpp-marketplace-tab.md) para efetuar quaisquer alterações.
4.  Clique em publicar para iniciar o fluxo de trabalho de publicar para que as suas alterações, aceda ao vivo.
