---
title: Criar subscrições na gestão de API do Azure | Documentos da Microsoft
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
ms.openlocfilehash: 1393e548c46c23f6b50c1b18a274febb74914ae8
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2019
ms.locfileid: "54054514"
---
# <a name="create-subscriptions-in-azure-api-management"></a>Criar subscrições na gestão de API do Azure

Quando publicar APIs através da gestão de API do Azure, é fácil e comum para proteger o acesso a essas APIs com chaves de subscrição. Aplicações de cliente que precisam de consumir as APIs publicadas tem de incluir uma chave de subscrição válido em pedidos de HTTP quando eles fazem chamadas para essas APIs. Para obter uma chave de subscrição para aceder a APIs, é necessária uma subscrição. Para obter mais informações sobre as subscrições, veja [subscrições na gestão de API do Azure](api-management-subscriptions.md).

Este artigo explica os passos para criar subscrições no portal do Azure.

## <a name="prerequisites"></a>Pré-requisitos

Efetuar os passos descritos neste artigo, os pré-requisitos são os seguintes:

+ [Criar uma instância de gestão de API](get-started-create-service-instance.md).
+ Compreender [subscrições na gestão de API](api-management-subscriptions.md).

## <a name="create-a-new-subscription"></a>Criar uma nova subscrição

1. Selecione **subscrições** no menu à esquerda.
2. Selecione **adicionar subscrição**.
3. Forneça um nome da subscrição e selecione o âmbito.
4. Selecione **Guardar**.

![Subscrições flexíveis](./media/api-management-subscriptions/flexible-subscription.png)

Depois de criar a subscrição, são fornecidas duas chaves de API para aceder as APIs. Uma chave é primária e uma é secundária. 

## <a name="next-steps"></a>Passos Seguintes
Obter mais informações sobre a gestão de API:

+ Saiba outro [conceitos](api-management-terminology.md) na gestão de API.
+ Siga nosso [tutoriais](import-and-publish.md) para saber mais sobre a gestão de API.
+ Verifique nossas [página de perguntas frequentes](api-management-faq.md) para perguntas comuns.