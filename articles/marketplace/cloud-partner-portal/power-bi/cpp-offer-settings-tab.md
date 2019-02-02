---
title: Definições para uma aplicação do Power BI oferece - Azure Marketplace da oferta | Documentos da Microsoft
description: Configure definições de oferta para uma oferta de aplicação do Power BI para o Microsoft AppSource Marketplace.
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
ms.date: 01/29/2019
ms.author: pbutlerm
ms.openlocfilehash: fef2455129d94913c5b51a08c04403834861bb48
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/02/2019
ms.locfileid: "55666787"
---
# <a name="power-bi-apps-offer-settings-tab"></a>Separador de definições de oferta de aplicações de BI de energia

O **nova oferta** é aberta a página para serviço de aplicações no primeiro separador com o nome **oferecer definições**.  Deve fornecer os identificadores de principal e o nome da sua oferta neste separador.  Um anexado asterisco (*) no nome do campo indica que é necessário.

![Separador de definições da oferta](./media/offer-settings-tab.png)


## <a name="offer-settings-fields"></a>Campos de definições da oferta 

Na **oferecer definições** guia, tem de fornecer os seguintes campos obrigatórios.

|  Campo        |  Descrição                                                               |
|---------------|----------------------------------------------------------------------------|
| **ID de oferta**  | Um identificador exclusivo (dentro de um perfil do publicador) para a oferta. Este identificador estará visível no produto URLs, modelos do Resource Manager, e relatórios de faturação. Tem um comprimento máximo de 50 carateres, só pode ser composto por carateres alfanuméricos em minúsculas e hífenes (-), mas não pode terminar com um traço. Este campo não pode ser alterado depois de uma oferta entra no ar. Por exemplo, se a Contoso publica uma oferta com o ID da oferta `sample-SvcApp`, que é atribuído o URL de AppSource `https://appsource.microsoft.com/marketplace/apps/contoso.sample-SvcApp`.      |
| **Publicador** | Identificador exclusivo da sua organização no [AppSource](https://appsource.microsoft.com). Todas as suas ofertas devem ser associadas com o ID de publicador. Este valor não pode ser modificado depois da oferta é guardada.                         |
| **Nome**      | Nome a apresentar para a sua oferta. Este nome será apresentada no AppSource e no Portal de parceiros de nuvem. Pode ter um máximo de 50 carateres. Documentação de orientação aqui é incluir um nome de marca reconhecível para o seu produto. Não inclua o nome da sua organização aqui, a menos que essa é a forma como a aplicação é comercializada. Se estiver de marketing esta oferta em outros Web sites e publicações, certifique-se de que o nome é o mesmo em todas as publicações.    <br/>Se libera uma oferta durante o período de pré-visualização de aplicações do Power BI, o modo de pré-visualização, anexe a cadeia `(Preview)` para o nome da sua aplicação, por exemplo `Sample Scv App (Preview)`. |
|     |     |


## <a name="next-steps"></a>Passos Seguintes

No separador seguinte, irá especificar [informações técnicas](./cpp-technical-info-tab.md) para a sua oferta.
