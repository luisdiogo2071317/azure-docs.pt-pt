---
title: Histórico de lançamento de versão de agente proteção palavra-passe do Azure AD no local
description: Histórico de alterações de comportamento e de lançamento da versão de documentos
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 10/30/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: c52c84a1311c30c19356bb8a1287b203faf476fc
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/01/2018
ms.locfileid: "50743262"
---
# <a name="preview--azure-ad-password-protection-agent-version-history"></a>Preview: Histórico de versões de agente de proteção palavra-passe do Azure AD

|     |
| --- |
| Proteção de palavra-passe do Azure AD é uma funcionalidade de pré-visualização pública do Azure Active Directory. Para obter mais informações sobre pré-visualizações, consulte [termos de utilização suplementares para pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="12100"></a>1.2.10.0

Data de lançamento: 17/8/2018

Correções:

* Registre-se-AzureADPasswordProtectionProxy e registre-se-AzureADPasswordProtectionForest agora suportam a autenticação multifator
* Registre-se-AzureADPasswordProtectionProxy requer um WS2012 ou o posterior controlador de domínio no domínio para evitar erros de encriptação.
* Serviço de agente do controlador de domínio é mais fiável sobre o pedido de uma nova política de palavra-passe do Azure na inicialização.
* Serviço do agente DC irá pedir uma nova política de palavra-passe do Azure a cada hora, se necessário, mas agora farão isso numa hora de início selecionado aleatoriamente.
* Serviço de agente do controlador de domínio já não irá causar um atraso indefinido no novo anúncio do controlador de domínio quando instalado num servidor antes da respetiva promoção como uma réplica.
* Serviço de agente do controlador de domínio agora respeitará a definição de configuração "Ativar a proteção de palavra-passe no Windows Server Active Directory"
* Ambos os instaladores de agente e o proxy do controlador de domínio agora suportará a atualização no local ao atualizar para versões futuras.

> [!WARNING]
> Atualização no local da versão 1.1.10.3 não é suportada e irá resultar num erro de instalação. Como atualizar para versão 1.2.10 ou posterior, tem primeiro completamente desinstalar o software de serviço de proxy de agente e controlador de domínio, e instalar a nova versão a partir do zero. Re-registo da proteção de palavra-passe do Azure AD service de Proxy é necessário.  Não é necessário voltar a registar a floresta.

> [!NOTE]
> Atualizações in-loco do software do agente DC exigirá uma reinicialização.

* Serviço de agente e o proxy de DC suportam agora em execução num servidor configurado para utilizar apenas os algoritmos em conformidade com FIPS.
* A melhoria do registo
* Desempenho de pequenas e robustez correções

## <a name="11103"></a>1.1.10.3

Data de lançamento: 6/15/2018

Versão de pré-visualização pública inicial

## <a name="next-steps"></a>Passos Seguintes

[Implementar proteção de palavras-passe do Azure AD](howto-password-ban-bad-on-premises-deploy.md)
