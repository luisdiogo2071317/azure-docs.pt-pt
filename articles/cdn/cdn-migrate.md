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
ms.openlocfilehash: 615f35c602f9a086bdc5c32b04a97efa368411a3
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36308130"
---
# <a name="migrate-an-azure-cdn-profile-from-standard-verizon-to-premium-verizon"></a>Migrar um perfil de CDN do Azure da Verizon padrão para Premium da Verizon

Azure rede de entrega de conteúdos (CDN) oferece quatro diferentes produtos, cada um dos quais oferece funcionalidades diferentes: 
- **CDN do Azure Standard da Microsoft**
- **CDN do Azure Standard da Akamai**
- **CDN do Azure Standard da Verizon**
- **CDN do Azure Premium da Verizon**.

Se estiver a utilizar um **CDN do Azure Standard da Verizon** perfil para gerir os pontos finais da CDN, terá a opção de atualizá-lo para um **CDN do Azure Premium da Verizon** perfil e preservar a todos os dados.

Para atualizar um **CDN do Azure Standard da Verizon** perfil, contacto [Microsoft Support](https://azure.microsoft.com/support/options/).

> [!IMPORTANT]
> Assim que tiver atualizado para uma **CDN do Azure Premium da Verizon** perfil, não é possível mais tarde convertê-lo novamente para um **CDN do Azure Standard da Verizon** perfil.
> 

## <a name="profile-comparison"></a>Comparação de perfil
**O Azure CDN Premium da Verizon** perfis tem as seguintes diferenças a chaves de **CDN do Azure Standard da Verizon** perfis de:
- Para determinados CDN do Azure funcionalidades como [compressão](cdn-improve-performance.md), regras, a colocação em cache e [georreplicação filtragem](cdn-restrict-access-by-country.md), não é possível utilizar a interface da CDN do Azure, tem de utilizar o portal da Verizon através de **gerir**botão.
- API: Ao contrário Standard da Verizon, não é possível utilizar a API para controlar as funcionalidades que são acedidas a partir do portal do Premium da Verizon. No entanto, pode utilizar a API para controlar outras funcionalidades comuns, tais como criar/eliminar um ponto final, remoção/carregamento ativos em cache e ativação/desativação de um domínio personalizado.
- Preços: Premium da Verizon tem uma estrutura de preço diferente para as transferências de dados que Standard da Verizon. Para obter mais informações, consulte [preços de rede de entrega de conteúdos](https://azure.microsoft.com/pricing/details/cdn/).

**O Azure CDN Premium da Verizon** perfis têm as seguintes funcionalidades adicionais:
- [Autenticação de token](cdn-token-auth.md): permite aos utilizadores obter e utilizam um token de obtenção segurados recursos.
- [Motor de regras](cdn-rules-engine.md): permite-lhe personalizar a forma como os pedidos de HTTP são processados.
- Ferramentas de análise avançada:
   - [Análise de HTTP de detalhado](cdn-advanced-http-reports.md)
   - [Análise de desempenho do contorno](cdn-edge-performance.md)
   - [Análise em tempo real](cdn-real-time-alerts.md)


## <a name="next-steps"></a>Passos Seguintes
Para ver uma comparação detalhada das funcionalidades de produto da CDN do Azure, consulte [funcionalidades do produto Azure CDN](Compare Azure CDN product features).

Para saber mais sobre o motor de regras, consulte o artigo [regras da CDN do Azure motor referência](cdn-rules-engine-reference.md).

