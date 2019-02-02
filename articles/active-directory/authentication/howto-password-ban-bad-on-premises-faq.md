---
title: Proteção de passe do Azure AD no local FAQ
description: Proteção de passe do Azure AD no local FAQ
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.openlocfilehash: 62bda3a1c9cb9d53578c2d471b9e63d1f0873234
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/02/2019
ms.locfileid: "55663303"
---
# <a name="preview-azure-ad-password-protection-on-premises---frequently-asked-questions"></a>Pré-visualização: Azure proteção de palavra-passe do AD no local - perguntas mais frequentes

|     |
| --- |
| Proteção de palavra-passe do Azure AD é uma funcionalidade de pré-visualização pública do Azure Active Directory. Para obter mais informações sobre pré-visualizações, consulte [termos de utilização suplementares para pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="general-questions"></a>Perguntas gerais

**P: Quando é que a proteção de palavra-passe do Azure AD será contactado disponibilidade geral (GA)?**

Disponibilidade geral está prevista para Q1 CY2019 (antes do final de Março de 2019). Obrigado a todos os que forneceu comentários sobre a funcionalidade de data – Agradecemos-lo!

**P: É no local suportada em nuvens não pública de proteção de palavra-passe do Azure AD?**

Não, a proteção de palavra-passe do Azure AD no local só é suportada na cloud pública.

**P: Como posso aplicar os benefícios da proteção de palavra-passe do Azure AD para um subconjunto dos meus utilizadores no local?**

Não suportado. Depois de implementada e ativada, a proteção de palavra-passe do Azure AD não distinguir - todos os utilizadores recebem benefícios de segurança igual.

**P: É possível para instalar a proteção de palavra-passe do Azure AD lado a lado com outros produtos baseados em palavra-passe-filtro?**

Sim. Suporte para várias dlls de filtro de palavra-passe registado é uma funcionalidade de Windows core e não específico para a proteção de palavra-passe do Azure AD. Todas as dlls de filtro de palavras-passe registado tem de concordar antes de uma palavra-passe é aceite.

**P: Por que motivo o DFSR é necessário para a replicação de sysvol?**

O FRS (a tecnologia antecessora da DFSR) tem vários problemas conhecidos e é totalmente suportado em versões mais recentes do Windows Server Active Directory. Zero testes de proteção de palavra-passe do Azure AD serão efetuada em domínios configurados do FRS.

Para obter mais informações, consulte os artigos seguintes:

[O caso para replicação de sysvol de migrar para o DFSR](https://blogs.technet.microsoft.com/askds/2010/04/22/the-case-for-migrating-sysvol-to-dfsr)

[O End é Nigh para FRS](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs)

**P: Por que motivo é necessário um reinício para instalar ou atualizar o software do agente DC?**

Este requisito é causado por núcleo de comportamento do Windows.

**P: Existe alguma forma de configurar um agente de controlador de domínio para utilizar um servidor proxy específico?**

Não.

## <a name="next-steps"></a>Passos Seguintes

Se tiver uma questão de proteção de palavra-passe do Azure AD no local que não está respondida aqui, submeta um item de comentário abaixo - obrigado!

[Implementar proteção de palavras-passe do Azure AD](howto-password-ban-bad-on-premises-deploy.md)
