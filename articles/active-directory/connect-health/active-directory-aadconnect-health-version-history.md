---
title: "Histórico das Versões do Azure AD Connect Health"
description: "Este documento descreve as versões para o Azure AD Connect Health e que foi incluído nessas versões."
services: active-directory
documentationcenter: 
author: karavar
manager: mtillman
editor: curtand
ms.assetid: 8dd4e998-747b-4c52-b8d3-3900fe77d88f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: b43eb5e78b70f38226e3e8cb53d1530d348c7c20
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2018
---
# <a name="azure-ad-connect-health-version-release-history"></a>Azure AD Connect Health: Histórico do Lançamento de Versões
A equipa do Azure Active Directory atualiza regularmente o Azure AD Connect Health com novas funcionalidades e funções. Este artigo apresenta as versões e funcionalidades que tenham sido publicadas.

## <a name="december-2017"></a>Dezembro de 2017
**Atualização do agente:**

*   Agente do Azure AD Connect Health para AD DS (versão 3.0.145.0)
  1. Melhoramentos de disponibilidade do agente 
  2. Foram adicionadas comandos de resolução de problemas de agente novo
  3. Correções de erros e melhoramentos gerais
*   Agente do Azure AD Connect Health para AD FS (versão 3.0.145.0)
  1. Foram adicionadas comandos de resolução de problemas de agente novo
  2. Melhoramentos de disponibilidade do agente 
  3. Correções de erros e melhoramentos gerais
  
## <a name="october-2017"></a>Outubro de 2017
**Atualização do agente:**

 * Agente do Azure AD Connect Health para sincronização (versão 3.0.129.0) lançada com o Azure AD Connect versão 1.1.649.0
<br></br> Corrigido um problema de compatibilidade de versão entre o Azure AD Connect e o agente do Azure AD Connect Health para sincronização. Este problema afeta os clientes que estiver a efetuar a atualização no local do Azure AD Connect versão 1.1.647.0, mas atualmente tem o agente de estado de funcionamento da versão 3.0.127.0. Após a atualização, o agente de estado de funcionamento já não pode enviar dados de estado de funcionamento sobre o serviço de sincronização ligar do Azure AD para o serviço de estado de funcionamento do Azure AD. Com esta correção está instalado o agente de estado de funcionamento da versão 3.0.129.0 durante a atualização do Azure AD Connect no local. Agente de estado de funcionamento versão 3.0.129.0 não tem o problema de compatibilidade com o Azure AD Connect versão 1.1.649.0.

## <a name="july-2017"></a>Julho de 2017
**Atualização do agente:**

*   Agente do Azure AD Connect Health para AD DS (versão 3.0.68.0)
  1. Correções de erros e melhoramentos gerais
  2. Suporte de nuvem sovereign
*   Agente do Azure AD Connect Health para AD FS (versão 3.0.68.0)
  1. Correções de erros e melhoramentos gerais
  2. Suporte de nuvem sovereign
* Agente do Azure AD Connect Health para sincronização (versão 3.0.68.0) lançada com o Azure AD Connect versão 1.1.614.0
1. Suporte para a nuvem do Microsoft Azure Government e na Alemanha de nuvem da Microsoft

## <a name="april-2017"></a>Abril de 2017      
**Atualização do agente:**

*   Agente do Azure AD Connect Health para AD FS (versão 3.0.12.0)
  1. Correções de erros e melhoramentos gerais
*   Agente do Azure AD Connect Health para AD DS (versão 3.0.12.0)
  1. Melhoramentos de carregar os contadores de desempenho
  2. Correções de erros e melhoramentos gerais

## <a name="october-2016"></a>Outubro de 2016
**Atualização do agente:**

* O agente do Azure AD Connect Health para AD FS \(versão 2.6.408.0\)
1. Melhoramentos na deteção de endereços IP do cliente em pedidos de autenticação
2. Correções de erros relacionados com alertas
* Agente do Azure AD Connect Health para AD DS (versão 2.6.408.0)
1. Correções de erros relacionados com alertas.
* Agente do Azure AD Connect Health para sincronização (versão 2.6.353.0) lançada com o Azure AD Connect versão 1.1.281.0
1. Fornecer os dados necessários para os relatórios de erros de sincronização
2. Correções de erros relacionados com alertas

**Novas funcionalidades de pré-visualização:**

* Relatórios de erros de sincronização do Azure AD Connect

**Novas funcionalidades:**

* Azure AD Connect Health para AD FS - campo do endereço IP está disponível no relatório sobre os principais 50 utilizadores com o nome de utilizador/palavra-passe incorreta.

## <a name="july-2016"></a>Julho de 2016
**Novas funcionalidades de pré-visualização:**

* [Azure AD Connect Health para AD DS](active-directory-aadconnect-health-adds.md).

## <a name="january-2016"></a>Janeiro de 2016
**Atualização do agente:**

* Agente do Azure AD Connect Health para AD FS (versão 2.6.91.1512)

**Novas funcionalidades:**

* [Ferramenta de teste de conectividade para o Azure AD Connect agentes de estado de funcionamento](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service)

## <a name="november-2015"></a>Novembro de 2015
**Novas funcionalidades:**

* Suporte para [controlo de acesso baseado em funções](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control)

**Novas funcionalidades de pré-visualização:**

* [Azure AD Connect Health para sincronização](active-directory-aadconnect-health-sync.md).

**Fixos problemas:**

* Correções de erros para erros encontrados durante registos do agente.

## <a name="september-2015"></a>Setembro de 2015
**Novas funcionalidades:**

* Errado relatório de palavra-passe do nome de utilizador para o AD FS
* Suporte para configurar o Proxy de HTTP não autenticado
* Suporte para configurar o agente no Server core
* Melhoramentos para alertas para o AD FS
* Melhoramentos no agente do Azure AD Connect Health para AD FS para dados e conectividade carregar.

**Fixos problemas:**

* Correções de erros nas informações sobre a utilização de tipos de erro do AD FS.

## <a name="june-2015"></a>Junho de 2015
**Versão inicial do Azure AD Connect Health para AD FS e Proxy do AD FS.**

**Novas funcionalidades:**

* Alertas de monitorização servidores do AD FS e Proxy do AD FS com notificações por e-mail.
* Acesso fácil a topologia do AD FS e padrões de contadores de desempenho do AD FS.
* Tendência de pedidos de token com êxito nos servidores do AD FS, agrupados por aplicações, métodos de autenticação, etc de localização de rede do pedido.
* Tendências de pedido falhado nos servidores do AD FS, agrupados por aplicações, etc. tipos de erro.
* Implementação do agente mais simples utilizar credenciais de Administrador Global do AD do Azure.  

## <a name="next-steps"></a>Passos Seguintes
Saiba mais sobre [monitorizar os serviços de infraestrutura e a sincronização de identidade de no local na nuvem](active-directory-aadconnect-health.md).

