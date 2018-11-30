---
title: Como criar subscrições na gestão de API do Azure | Documentos da Microsoft
description: Saiba como criar subscrições na gestão de API do Azure.
services: api-management
documentationcenter: ''
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2018
ms.author: apimpm
ms.openlocfilehash: 3f4e113125ec9644aac974e47996afe290e57cee
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/29/2018
ms.locfileid: "52621823"
---
# <a name="how-to-create-subscriptions-in-azure-api-management"></a>Como criar subscrições na gestão de API do Azure

Ao publicar APIs de gestão de API através do Azure (APIM), é a forma mais fácil e mais comuns para proteger o acesso a essas APIs com chaves de subscrição. Em outras palavras, as aplicações cliente que precisam de consumir as APIs publicadas tem de incluir uma chave de subscrição válido em pedidos de HTTP ao efetuar chamadas para essas APIs. Para obter uma chave de subscrição para aceder a APIs, é necessária uma subscrição. Para obter mais informações sobre as subscrições, veja [subscrições na gestão de API do Azure](api-management-subscriptions.md)

Este artigo explica os passos para criar subscrições no portal do Azure.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir os passos neste artigo, terá de:

+ [Criar uma instância do APIM](get-started-create-service-instance.md)
+ Compreender [subscrições no APIM](api-management-subscriptions.md)

## <a name="create-a-new-subscription"></a>Criar uma nova subscrição

1. Clique em **subscrições** no menu à esquerda
2. Clique em **adicionar subscrição**
3. Forneça um nome da subscrição e selecione o âmbito
4. Clicar em **Guardar**

![Subscrições flexíveis](./media/api-management-subscriptions/flexible-subscription.png)

Depois de criar a subscrição, um par de chaves de API (primários e secundários) são aprovisionados para aceder as APIs.

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre a gestão de API:

+ Saiba outro [conceitos](api-management-terminology.md) na gestão de API
+ Siga nosso [tutoriais](import-and-publish.md) para saber mais sobre a gestão de API
+ Verifique nossas [página de perguntas frequentes](api-management-faq.md) para perguntas comuns