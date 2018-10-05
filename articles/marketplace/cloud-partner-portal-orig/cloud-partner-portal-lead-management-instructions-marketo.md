---
title: Marketo | Documentos da Microsoft
description: Configure a gestão de oportunidades potenciais para o Marketo.
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
ms.date: 09/14/2018
ms.author: pbutlerm
ms.openlocfilehash: abb0abb94d3b3e7abc4dce58cdb11fa0c2cedd34
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48810259"
---
# <a name="configure-lead-management-in-marketo"></a>Configurar a gestão de oportunidades potenciais no Marketo

Este artigo descreve como configurar o Marketo para lidar com oportunidades potenciais de venda de Microsoft.

1. Inicie sessão no Marketo.
2. Selecione **Design Studio**.
    ![Marketo Design Studio](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo1.png)

3.  Selecione **novo formulário**.
    ![Novo formulário do Marketo](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo2.png)

4.  Preencha os campos obrigatórios no novo formulário e, em seguida, selecione **criar**.
    ![Marketo criar novo formulário](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo3.png)

4.  No campo de detalhes, selecione **concluir**.
    ![Formulário de conclusão do Marketo](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo4.png)

5.  Aprovar e fechar.

6.  No separador MarketplaceLeadBacked, selecione **incorporar código**.
    ![Opção de código de incorporação do Marketo](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo5.png)

7.  Código de incorporação do Marketo apresenta o código semelhante ao seguinte exemplo.

`<script src="//app-ys12.marketo.com/js/forms2/js/forms2.min.js"></script>`

    <form id="mktoForm_1179"></form>
    <script>MktoForms2.loadForm("("//app-ys12.marketo.com", "123-PQR-789", 1179);</script>

8.  Copie os valores mostrados no código de incorporação para que pode configurar o **Id do servidor**, **Munchkin Id**, e **Id de formulário** os campos do Marketo no Portal de parceiros de nuvem.

Utilize o exemplo seguinte como guia para obter os Ids de que precisa do exemplo de código de incorporação do Marketo.

- Id do servidor = **ys12**
- Id de munchkin = **123-PQR-789**
- Id de formulário = **1179**\
