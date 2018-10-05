---
title: Ponto final de HTTPS | Documentos da Microsoft
description: Configure a gestão de oportunidades potenciais para Https.
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
ms.date: 09/17/2018
ms.author: pbutlerm
ms.openlocfilehash: fd13a7281c7e8702fd199364261ebcd458db0555
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48810243"
---
# <a name="configure-lead-management-using-an-https-endpoint"></a>Configurar a gestão de oportunidades potenciais com um ponto final HTTPS

Pode utilizar um ponto final HTTPS para lidar com o Azure Marketplace e AppSource oportunidades potenciais que podem ser gravados num sistema CRM. Este artigo descreve como configurar a gestão de oportunidades potenciais com o serviço de automatização do Microsoft Flow.


## <a name="create-a-flow-using-microsoft-flow"></a>Criar um fluxo com o Microsoft Flow

1.  Abra o [fluxo](https://flow.microsoft.com/) página Web. Selecione **iniciar sessão** ou selecione **inscrever-se gratuitamente** para criar uma conta gratuita do fluxo.

2.  Iniciar sessão e selecione **os meus fluxos** na barra de menus.

    ![Meus fluxos](./media/cloud-partner-portal-lead-management-instructions-https/image001.png)

3.  Selecione **criar do zero**.

    ![Criar do zero](./media/cloud-partner-portal-lead-management-instructions-https/image003.png)


4.  Selecione o **solicitação/resposta** conector e, em seguida, procure o acionador de pedido. 

    ![Criar do zero](./media/cloud-partner-portal-lead-management-instructions-https/image005.png)

5. Selecione o **pedir** acionador.
    ![Acionador de pedido](./media/cloud-partner-portal-lead-management-instructions-https/image007.png)


6.  Copiar o **exemplo de JSON** no final deste artigo para o **pedir esquema JSON do corpo**.

7.  Adicione um novo passo e escolha o sistema CRM à sua escolha com a ação para criar um novo registo. Captura de ecrã seguinte mostra **Dynamics 365 - criar um novo registo** como exemplo.

    ![Criar uma nova palavra-passe](./media/cloud-partner-portal-lead-management-instructions-https/image009.png)

8.  Fornecer as entradas de ligação para o conector e selecione o **leva** entidade.

    ![Selecionar oportunidades potenciais](./media/cloud-partner-portal-lead-management-instructions-https/image011.png)

9.  Fluxos mostra um formulário para fornecer informações de oportunidades potenciais. Pode mapear os itens no pedido de entrada ao optar por adicionar conteúdo dinâmico.

    ![Adicionar conteúdo dinâmico](./media/cloud-partner-portal-lead-management-instructions-https/image013.png)

10.  Mapear os campos que pretende e, em seguida, selecione **guardar** para guardar o fluxo.

11. Um URL do HTTP POST é criado no pedido. Copie este URL e utilize-o como o ponto final HTTPS.

    ![URL de HTTP Post](./media/cloud-partner-portal-lead-management-instructions-https/image015.png)

## <a name="configure-your-offer-to-send-leads-to-the-https-endpoint"></a>Configure a sua oferta para enviar as oportunidades potenciais para o ponto final HTTPS

Quando configurar as informações de gestão de oportunidades potenciais para a sua oferta, selecione **ponto final de HTTPS** para o destino de levar e cole o URL do HTTP POST que copiou no passo anterior.  

![Adicionar conteúdo dinâmico](./media/cloud-partner-portal-lead-management-instructions-https/image017.png)

Quando são gerados oportunidades potenciais, a Microsoft irá enviar oportunidades potenciais para o fluxo, que são roteadas para o sistema CRM que configurou.


## <a name="json-example"></a>Exemplo de JSON

``` json
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "definitions": {},
  "id": "http://example.com/example.json",
  "properties": {
    "ActionCode": {
      "id": "/properties/ActionCode",
      "type": "string"
    },
    "OfferTitle": {
      "id": "/properties/OfferTitle",
      "type": "string"
    },
    "LeadSource": {
      "id": "/properties/LeadSource",
      "type": "string"
    },
    "UserDetails": {
      "id": "/properties/UserDetails",
      "properties": {
        "Company": {
          "id": "/properties/UserDetails/properties/Company",
          "type": "string"
        },
        "Country": {
          "id": "/properties/UserDetails/properties/Country",
          "type": "string"
        },
        "Email": {
          "id": "/properties/UserDetails/properties/Email",
          "type": "string"
        },
        "FirstName": {
          "id": "/properties/UserDetails/properties/FirstName",
          "type": "string"
        },
        "LastName": {
          "id": "/properties/UserDetails/properties/LastName",
          "type": "string"
        },
        "Phone": {
          "id": "/properties/UserDetails/properties/Phone",
          "type": "string"
        },
        "Title": {
          "id": "/properties/UserDetails/properties/Title",
          "type": "string"
        }
      },
      "type": "object"
    }
  },
  "type": "object"
}
```
