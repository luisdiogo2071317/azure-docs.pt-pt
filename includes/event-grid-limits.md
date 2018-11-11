---
title: incluir ficheiro
description: incluir ficheiro
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 04/30/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 2425d9455606f5eabba4b89cfa238b7a928be30e
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285808"
---
####

Os limites seguintes aplicam-se a tópicos de sistema do Event Grid e tópicos personalizados, *não* domínios de evento.

| Recurso | Limite |
| --- | --- |
| Tópicos personalizados por subscrição do Azure | 100 |
| Subscrições de eventos por tópico | 500 |
| Publicar a taxa para um tópico personalizado (entrada) | 5000 eventos por segundo por tópico |

####

Os limites seguintes aplicam-se apenas aos domínios de evento.

| Recurso | Limite |
| --- | --- |
| Tópicos por domínio de eventos | 1000 durante a pré-visualização pública |
| Subscrições de eventos por tópico dentro de um domínio | 50 durante a pré-visualização pública |
| Âmbito de domínio subscrições de eventos | 50 durante a pré-visualização pública |
| Publicar a taxa para um domínio de evento (entrada) | 5000 eventos por segundo durante a pré-visualização pública |