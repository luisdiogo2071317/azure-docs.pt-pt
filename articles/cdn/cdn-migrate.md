---
title: Migrar um perfil de CDN do Azure da Verizon padrão para Premium da Verizon | Microsoft Docs
description: Saiba mais sobre os detalhes da migração de um perfil do Standard da Verizon para Premium da Verizon.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2018
ms.author: v-deasim
ms.custom: ''
ms.openlocfilehash: 074dbb094e7ae2cd1f1719016928bd7348da3949
ms.sourcegitcommit: 0408c7d1b6dd7ffd376a2241936167cc95cfe10f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/26/2018
ms.locfileid: "36958060"
---
# <a name="migrate-an-azure-cdn-profile-from-standard-verizon-to-premium-verizon"></a>Migrar um perfil de CDN do Azure da Verizon padrão para Premium da Verizon

Quando cria um perfil de rede de entrega conteúdo do Azure (CDN) para gerir os seus pontos finais, a CDN do Azure oferece quatro diferentes produtos para escolher. Para obter informações sobre os diferentes produtos e as respetivas funcionalidades disponíveis, consulte [funcionalidades do produto comparar o Azure CDN](cdn-features.md).

Se tiver criado uma **CDN do Azure Standard da Verizon** perfil e estiver a utilizar para gerir os pontos finais da CDN, tem a opção de atualizá-lo para um **CDN do Azure Premium da Verizon** perfil. Quando atualizar, os pontos finais da CDN e todos os dados serão mantidos. 

> [!IMPORTANT]
> Assim que tiver atualizado para uma **CDN do Azure Premium da Verizon** perfil, não é possível mais tarde convertê-lo novamente para um **CDN do Azure Standard da Verizon** perfil.
> 

Para atualizar um **CDN do Azure Standard da Verizon** perfil, contacto [Microsoft Support](https://azure.microsoft.com/support/options/).

## <a name="profile-comparison"></a>Comparação de perfil
**O Azure CDN Premium da Verizon** perfis tem as seguintes diferenças a chaves de **CDN do Azure Standard da Verizon** perfis de:
- Para determinados CDN do Azure funcionalidades como [compressão](cdn-improve-performance.md), [regras a colocação em cache](cdn-caching-rules.md), e [georreplicação filtragem](cdn-restrict-access-by-country.md), não é possível utilizar a interface da CDN do Azure, tem de utilizar o portal da Verizon através do **Gerir** botão.
- API: Ao contrário com Standard da Verizon, não é possível utilizar a API para controlar as funcionalidades que são acedidas a partir do portal do Premium da Verizon. No entanto, pode utilizar a API para controlar outras funcionalidades comuns, tais como criar/eliminar um ponto final, remoção/carregamento ativos em cache e ativação/desativação de um domínio personalizado.
- Preços: Premium da Verizon tem uma estrutura de preço diferente para as transferências de dados que Standard da Verizon. Para obter mais informações, consulte [preços de rede de entrega de conteúdos](https://azure.microsoft.com/pricing/details/cdn/).

**O Azure CDN Premium da Verizon** perfis têm as seguintes funcionalidades adicionais:
- [Autenticação de token](cdn-token-auth.md): permite aos utilizadores obter e utilizam um token de obtenção segurados recursos.
- [Motor de regras](cdn-rules-engine.md): permite-lhe personalizar a forma como os pedidos de HTTP são processados.
- Ferramentas de análise avançada:
   - [Análise de HTTP de detalhado](cdn-advanced-http-reports.md)
   - [Análise de desempenho do contorno](cdn-edge-performance.md)
   - [Análise em tempo real](cdn-real-time-alerts.md)


## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre o motor de regras, consulte o artigo [regras da CDN do Azure motor referência](cdn-rules-engine-reference.md).

