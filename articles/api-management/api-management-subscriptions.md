---
title: As subscrições na gestão de API do Azure | Documentos da Microsoft
description: Saiba mais sobre o conceito de subscrições na gestão de API do Azure.
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
ms.openlocfilehash: 45a65c7a94f3e6917b2882f27c9ad7d764ef97d7
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/29/2018
ms.locfileid: "52621834"
---
# <a name="subscriptions-in-azure-api-management"></a>Subscrições na gestão de API do Azure

As assinaturas são um conceito importante na gestão de API do Azure (APIM). É a forma mais comum para os consumidores de APIS obter acesso a APIs publicadas através de uma instância APIM. Este artigo fornece uma descrição geral do conceito.

## <a name="what-is-subscriptions"></a>O que é subscrições

Ao publicar APIs por meio de APIM, é a forma mais fácil e mais comuns para proteger o acesso a essas APIs com chaves de subscrição. Em outras palavras, os desenvolvedores que precisam de consumir as APIs publicadas tem de incluir uma chave de subscrição válido em pedidos de HTTP ao efetuar chamadas para essas APIs. Caso contrário, as chamadas são rejeitadas imediatamente pelo gateway de APIM e não são reencaminhadas para os serviços de back-end.

Para obter uma chave de subscrição para aceder a APIs, é necessária uma subscrição. Subscrição é essencialmente um contentor com o nome para um par de chaves de subscrição. As assinaturas podem ser obtidas pelos desenvolvedores que precisam de consumir as APIs publicadas, com ou sem a aprovação por editores de API. Os publicadores de APIS também podem criar subscrições diretamente, em nome dos consumidores de API.

> [!TIP]
> APIM também suporta outros mecanismos para proteger o acesso a APIs incluindo [OAuth2.0](api-management-howto-protect-backend-with-aad.md), [certificados de cliente](api-management-howto-mutual-certificates-for-clients.md), e [listas de permissões de IP](https://docs.microsoft.com/azure/api-management/api-management-access-restriction-policies#RestrictCallerIPs)

## <a name="scope-of-subscriptions"></a>Âmbito de subscrições

As assinaturas podem ser associadas a vários âmbitos: produto, todas as APIs ou uma API individual.

### <a name="subscriptions-for-a-product"></a>Subscrições para um produto

Tradicionalmente, subscrições no APIM sempre foram associadas um único [produto da API](api-management-terminology.md) âmbito. Os desenvolvedores seriam encontrar a lista de produtos no Portal do programador e submeter pedidos de subscrição para os produtos que gostariam de utilizar. Depois de uma subscrição do pedido for aprovado (automaticamente ou pelos editores de APIS), o programador pode utilizar as chaves na mesma para aceder a todas as APIs no produto.

![Assinaturas do produto](./media/api-management-subscriptions/product-subscription.png)

> [!TIP]
> Em determinados cenários, os publicadores de APIS talvez queira publicar um produto da API para o público sem a necessidade de subscrições. Eles podem, desmarque a **requer subscrição** opção a **definições** página do produto no portal do Azure. Como resultado, todas as APIs sob o produto podem ser acedidas sem uma chave de API.

### <a name="subscriptions-for-all-apis-or-an-individual-api"></a>Subscrições para todas as APIs ou uma API individual

> [!NOTE]
> Atualmente, esta funcionalidade está disponível na camada de consumo de gestão de API, apenas.

Quando Introduzimos o [consumo](https://aka.ms/apimconsumptionblog) escalão de APIM, fizemos algumas alterações para simplificar a gestão de chaves. Em primeiro lugar, adicionámos duas mais âmbitos de subscrição - todas as APIs e uma única API. O âmbito das subscrições já não está limitado a um produto da API. Agora é possível criar chaves de conceder acesso a uma API (ou todas as APIs numa instância APIM), sem a necessidade de criar um produto e adicionar-lhe as APIs do primeiro. Além disso, cada instância APIM agora vem com uma assinatura imutável, todas as APIs, que torna mais fácil e mais simples de testar e depurar as APIs dentro da consola de teste.

Em segundo lugar, a APIM agora permite subscrições de "autónomo". Subscrições já não são necessários para estar associado a uma conta de programador. Isto é útil em cenários, como quando uma subscrição é partilhada por vários desenvolvedores ou equipes.

Por fim, os publicadores de APIS podem agora [criar subscrições](api-management-howto-create-subscriptions.md) diretamente no Portal do Azure:

![Subscrições flexíveis](./media/api-management-subscriptions/flexible-subscription.png)

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre a gestão de API:

+ Saiba outro [conceitos](api-management-terminology.md) na gestão de API
+ Siga nosso [tutoriais](import-and-publish.md) para saber mais sobre a gestão de API
+ Verifique nossas [página de perguntas frequentes](api-management-faq.md) para perguntas comuns