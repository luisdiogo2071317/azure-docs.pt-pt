---
title: Esquemas de controlo AS2 para a monitorização do B2B - Azure Logic Apps | Microsoft Docs
description: Utilize esquemas de controlo AS2 para monitorizar as mensagens de B2B de transações na sua conta de integração do Azure.
author: padmavc
manager: jeconnoc
editor: ''
services: logic-apps
documentationcenter: ''
ms.assetid: f169c411-1bd7-4554-80c1-84351247bf94
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 48e39fd20716e962c4a3e367fdff18e0b4fba32d
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35300886"
---
# <a name="start-or-enable-tracking-of-as2-messages-and-mdns-to-monitor-success-errors-and-message-properties"></a>Iniciar ou ativar o controlo de mensagens AS2 e MDNs êxito do monitor, erros e propriedades da mensagem
Pode utilizar estes esquemas de controlo AS2 na sua conta de integração do Azure para o ajudar a monitorizar as transações do empresa-empresa (B2B):

* Esquema de controlo de mensagem AS2
* Esquema de controlo AS2 MDN

## <a name="as2-message-tracking-schema"></a>Esquema de controlo de mensagem AS2
````java

    {
       "agreementProperties": {  
            "senderPartnerName": "",  
            "receiverPartnerName": "",  
            "as2To": "",  
            "as2From": "",  
            "agreementName": ""  
        },  
        "messageProperties": {
            "direction": "",
            "messageId": "",
            "dispositionType": "",
            "fileName": "",
            "isMessageFailed": "",
            "isMessageSigned": "",
            "isMessageEncrypted": "",
            "isMessageCompressed": "",
            "correlationMessageId": "",
            "incomingHeaders": {
            },
            "outgoingHeaders": {
            },
        "isNrrEnabled": "",
        "isMdnExpected": "",
        "mdnType": ""
        }
    }
````

| Propriedade | Tipo | Descrição |
| --- | --- | --- |
| senderPartnerName | Cadeia | Nome do parceiro do remetente da mensagem AS2. (Opcional) |
| receiverPartnerName | Cadeia | Nome do parceiro do recetor de mensagem AS2. (Opcional) |
| as2To | Cadeia | Nome de recetor de mensagem a AS2, de cabeçalhos da mensagem AS2. (Obrigatório) |
| as2From | Cadeia | Nome do remetente da mensagem AS2, dos cabeçalhos da mensagem AS2. (Obrigatório) |
| agreementName | Cadeia | Nome do contrato AS2 para que as mensagens são resolvidas. (Opcional) |
| direção | Cadeia | Direção do fluxo de mensagens, receber ou enviar. (Obrigatório) |
| messageId | Cadeia | ID de mensagem AS2, dos cabeçalhos da mensagem AS2 (opcional) |
| dispositionType |Cadeia | Valor de tipo de mensagem disposição notificação (MDN) disposição. (Opcional) |
| fileName | Cadeia | Nome de ficheiro, o cabeçalho da mensagem AS2. (Opcional) |
| isMessageFailed |Booleano | Indica se a mensagem AS2 falhou. (Obrigatório) |
| isMessageSigned | Booleano | Indica se a mensagem de AS2 foi assinada. (Obrigatório) |
| isMessageEncrypted | Booleano | Indica se a mensagem de AS2 foi encriptada. (Obrigatório) |
| isMessageCompressed |Booleano | Indica se a mensagem AS2 foi comprimida. (Obrigatório) |
| correlationMessageId | Cadeia | ID de mensagem AS2, correlacionar mensagens com MDNs. (Opcional) |
| incomingHeaders |Dicionário de JToken | Detalhes de cabeçalho de mensagem de entrada AS2. (Opcional) |
| outgoingHeaders |Dicionário de JToken | Detalhes de cabeçalho de mensagem de saída AS2. (Opcional) |
| isNrrEnabled | Booleano | Utilize o valor predefinido se o valor não é conhecido. (Obrigatório) |
| isMdnExpected | Booleano | Utilize o valor predefinido se o valor não é conhecido. (Obrigatório) |
| mdnType | Enum | Valores permitidos são **NotConfigured**, **sincronização**, e **Async**. (Obrigatório) |

## <a name="as2-mdn-tracking-schema"></a>Esquema de controlo AS2 MDN
````java

    {
        "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "as2To": "",
                "as2From": "",
                "agreementName": "g"
            },
            "messageProperties": {
                "direction": "",
                "messageId": "",
                "originalMessageId": "",
                "dispositionType": "",
                "isMessageFailed": "",
                "isMessageSigned": "",
                "isNrrEnabled": "",
                "statusCode": "",
                "micVerificationStatus": "",
                "correlationMessageId": "",
                "incomingHeaders": {
                },
                "outgoingHeaders": {
                }
            }
    }
````

| Propriedade | Tipo | Descrição |
| --- | --- | --- |
| senderPartnerName | Cadeia | Nome do parceiro do remetente da mensagem AS2. (Opcional) |
| receiverPartnerName | Cadeia | Nome do parceiro do recetor de mensagem AS2. (Opcional) |
| as2To | Cadeia | Nome do parceiro que recebe a mensagem AS2. (Obrigatório) |
| as2From | Cadeia | Nome do parceiro que envia a mensagem AS2. (Obrigatório) |
| agreementName | Cadeia | Nome do contrato AS2 para que as mensagens são resolvidas. (Opcional) |
| direção |Cadeia | Direção do fluxo de mensagens, receber ou enviar. (Obrigatório) |
| messageId | Cadeia | ID de mensagem AS2. (Opcional) |
| originalMessageId |Cadeia | AS2 original ID de mensagem. (Opcional) |
| dispositionType | Cadeia | Valor do tipo MDN disposição. (Opcional) |
| isMessageFailed |Booleano | Indica se a mensagem AS2 falhou. (Obrigatório) |
| isMessageSigned |Booleano | Indica se a mensagem de AS2 foi assinada. (Obrigatório) |
| isNrrEnabled | Booleano | Utilize o valor predefinido se o valor não é conhecido. (Obrigatório) |
| statusCode | Enum | Valores permitidos são **aceites**, **rejeitado**, e **AcceptedWithErrors**. (Obrigatório) |
| micVerificationStatus | Enum | Valores permitidos são **NotApplicable**, **com êxito**, e **falha**. (Obrigatório) |
| correlationMessageId | Cadeia | ID de correlação. ID de messaged original (o ID de mensagem da mensagem para o qual está configurada MDN). (Opcional) |
| incomingHeaders | Dicionário de JToken | Indica os detalhes de cabeçalho da mensagem recebida. (Opcional) |
| outgoingHeaders |Dicionário de JToken | Indica a enviar detalhes de cabeçalho da mensagem. (Opcional) |

## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre o [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md).    
* Saiba mais sobre [monitorização mensagens B2B](logic-apps-monitor-b2b-message.md).   
* Saiba mais sobre [B2B esquemas de controlo personalizado](logic-apps-track-integration-account-custom-tracking-schema.md).   
* Saiba mais sobre [X12 controlo esquemas](logic-apps-track-integration-account-x12-tracking-schema.md).   
* Saiba mais sobre [mensagens B2B na análise de registos de controlo](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).
