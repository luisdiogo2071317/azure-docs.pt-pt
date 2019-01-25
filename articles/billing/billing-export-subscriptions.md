---
title: Exportar as suas informações de nível superior de subscrição do Azure | Documentos da Microsoft
description: Descreve como pode ver todos os IDs associados à conta de subscrição do Azure.
keywords: ''
services: billing
documentationcenter: ''
author: adpick
manager: adpick
editor: ''
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: banders
ms.openlocfilehash: a2716793a47eae56a8e6eef8416ff446b6f7176f
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2019
ms.locfileid: "54902572"
---
# <a name="export-and-view-your-top-level-subscription-information"></a>Exportar e veja as suas informações de subscrição de nível superior
Se precisar de ver o conjunto de IDs associados com as suas credenciais de utilizador, de subscrição [transferir um ficheiro. JSON com informações da sua subscrição a partir do Centro de contas do Azure](http://account.azure.com/subscriptions/download).

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

O ficheiro. JSON transferido fornece as seguintes informações:
- Correio Eletrónico: O endereço de e-mail associado à sua conta.
- PUID: O identificador exclusivo associado à sua conta de cobrança.
- SubscriptionIds: Uma lista de subscrições que pertencem à sua conta, enumerada pelo ID da subscrição.

### <a name="subscriptionsjson-sample"></a>exemplo de subscriptions.JSON

```json
{
  "Email":"admin@contoso.com",
  "Puid":"00052xxxxxxxxxxx",
  "SubscriptionIds":[
    "38124d4d-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "7c8308f1-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "39a25f2b-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "52ec2489-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "e42384b2-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "90757cdc-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
  ]
}
```
