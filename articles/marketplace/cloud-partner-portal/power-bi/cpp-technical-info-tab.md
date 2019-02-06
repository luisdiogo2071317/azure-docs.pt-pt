---
title: Informações técnicas para uma oferta de aplicação do Power BI - Azure Marketplace | Documentos da Microsoft
description: Configure os campos de informações técnicas para uma oferta de aplicação do Power BI para o Microsoft AppSource Marketplace.
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
ms.openlocfilehash: d96ef2fd318d6164e1b7dfc5c4b72d6957af0f3e
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/05/2019
ms.locfileid: "55744439"
---
# <a name="power-bi-apps-technical-info-tab"></a>Separador de informações técnicas de aplicações de BI de energia

O **informações técnicas** separador da **nova oferta** página é onde o instalador do Power BI é fornecer o URL do pacote e quaisquer informações adicionais necessárias para a validação da nova oferta.  Na versão inicial, todas as aplicações do Power BI são gratuito, disponível para download do AppSource sem encargos adicionais. Como resultado, não será capaz de definir qualquer stock manter unidades (SKU) para este tipo de oferta.

![Separador de informações técnica](./media/technical-info-tab.png)


## <a name="technical-info-fields"></a>Campos de informações técnicos 

Na **informações técnicas** guia, tem de fornecer os seguintes campos.  Um anexado asterisco (*) na etiqueta de campo indica que é necessário.

|        Campo          |  Descrição                                                                 |
|    ---------------    |  ----------------------------------------------------------------------------|
| **URL de instalador**     | Endereço gerado pelo Power BI, quando publicar a aplicação e promovê-lo para produção.  Para obter mais informações sobre como gerar o URL, consulte [publicar o serviço de aplicações no Power BI](https://docs.microsoft.com/power-bi/service-create-distribute-apps).  |
|  **Instruções de validação**  |  Instruções de texto opcional (carateres máx 3000) para a equipa de validação da Microsoft ajudar a configurar, ligar e testar a sua aplicação, incluindo: definições de configuração típica, contas de teste ou parâmetros que podem ser usados para testar a opção "Ligar dados", etc. Estas informações só serão visíveis para a equipe de validação e só são utilizadas para fins de validação.  |
| **Esta aplicação é criada como um pacote de conteúdos do Power BI?** | Atualmente, este é um campo utilizado internamente. Deixe o valor definido para o valor predefinido, `No`; caso contrário, este campo para a alteração `Yes` poderia impedir a publicação.  |  
|  |  |


## <a name="next-steps"></a>Passos Seguintes

Nos próximos [detalhes de loja](./cpp-storefront-details-tab.md) guia, irá fornecer informações legais e de marketing para a sua aplicação.

