---
title: Exportar as informações de nível superior de subscrição do Azure | Microsoft Docs
description: Descreve como pode ver a subscrição do Azure todos os IDs associados a sua conta.
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
ms.date: 10/9/2017
ms.author: adpick
ms.openlocfilehash: b995b0c7154faab66a44bef1a7d74eeb8404e5c1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655456"
---
# <a name="export-and-view-your-top-level-subscription-information"></a>Exportar e veja as suas informações de subscrição de nível superior
Se precisar de ver o conjunto de subscrição IDs associados com as suas credenciais de utilizador, [transferir um ficheiro. JSON com informações da sua subscrição a partir do Centro de contas do Azure](http://account.azure.com/subscriptions/download).

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

O ficheiro transferido. JSON fornece as seguintes informações:
- E-mail: O endereço de e-mail associado à sua conta.
- PUID: O identificador exclusivo associado à sua conta de faturação.
- SubscriptionIds: Uma lista de subscrições que pertencem à sua conta, enumerada por ID de subscrição.

### <a name="subscriptionsjson-sample"></a>exemplo de subscriptions.JSON
~~~~
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
~~~~
