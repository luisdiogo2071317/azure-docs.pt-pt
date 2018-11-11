---
title: Histórico das Versões do Azure AD Connect Health
description: Este documento descreve as versões para o Azure AD Connect Health e o que está incluído nessas versões.
services: active-directory
documentationcenter: ''
author: zhiweiwangmsft
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
ms.openlocfilehash: 8ca04fef8a17c43c9d1f761e36d8da7f1e07d4d5
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51283585"
---
# <a name="azure-ad-connect-health-version-release-history"></a>Azure AD Connect Health: Histórico do Lançamento de Versões
A equipa do Azure Active Directory atualiza regularmente o Azure AD Connect Health com novos recursos e funcionalidades. Este artigo lista as versões e funcionalidades que foram lançadas.
O Azure AD Connect Health para sincronização está integrado com a instalação do Azure AD Connect. Leia mais sobre [histórico de lançamento do Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history)


## <a name="august-2018"></a>Agosto de 2018 
*  Agente do Azure AD Connect Health para sincronização (versão 3.1.7.0), lançado com o Azure AD Connect versão 1.1.880.0    
   1. Correção para [libera o problema de CPU elevado do agente com o .NET Framework KB de monitorização](https://support.microsoft.com/help/4346822/high-cpu-issue-in-azure-active-directory-connect-health-for-sync)

## <a name="june-2018"></a>junho de 2018 
**Novas funcionalidades de pré-visualização:** 
* O Azure AD Connect Health para sincronização - diagnosticar e resolver erros de sincronização de atributo duplicado do portal 

**Atualização do agente:** 
*  Agente do Azure AD Connect Health para AD DS (versão 3.1.7.0)    
   1. Correção para [libera o problema de CPU elevado do agente com o .NET Framework KB de monitorização](https://support.microsoft.com/help/4346822/high-cpu-issue-in-azure-active-directory-connect-health-for-sync)
   
*   Agente do Azure AD Connect Health para AD FS (versão 3.1.7.0)  
   1. Correção para [libera o problema de CPU elevado do agente com o .NET Framework KB de monitorização](https://support.microsoft.com/help/4346822/high-cpu-issue-in-azure-active-directory-connect-health-for-sync)
   2. Correções de resultados de teste no servidor secundário do AD FS Server 2016
   
*   Agente do Azure AD Connect Health para AD FS (versão 3.1.2.0)  
   1. Correção para o gerenciamento de memória de agente e alertas relacionados especificamente para a versão 3.0.244.0


## <a name="may-2018"></a>Maio de 2018
**Atualização do agente:**
*   Agente do Azure AD Connect Health para AD DS (versão 3.0.244.0)
 1. Melhoria de privacidade do agente  
 2. Correções de bugs e melhoramentos gerais

*   Agente do Azure AD Connect Health para AD FS (versão 3.0.244.0)
 1. Serviço de diagnóstico de agente e melhorias de módulo do PowerShell relacionadas
 2. Melhoria de privacidade do agente  
 3. Correções de bugs e melhoramentos gerais

* Agente do Azure AD Connect Health para sincronização (versão 3.0.164.0), lançado com o Azure AD Connect versão 1.1.819.0 
 1. Melhoria de privacidade do agente  
 2. Correções de bugs e melhoramentos gerais


## <a name="march-2018"></a>Março de 2018
**Novas funcionalidades de pré-visualização:**
* O Azure AD Connect Health para AD FS - relatório de IP em risco e o alerta.

**Atualização do agente:**

*   Agente do Azure AD Connect Health para AD DS (versão 3.0.176.0)
  1. Aprimoramentos de disponibilidade do agente 
  2. Correções de bugs e melhoramentos gerais
*   Agente do Azure AD Connect Health para AD FS (versão 3.0.176.0)
  1. Aprimoramentos de disponibilidade do agente 
  2. Correções de bugs e melhoramentos gerais
* Agente do Azure AD Connect Health para sincronização (versão 3.0.129.0), lançado com o Azure AD Connect versão 1.1.750.0  
  1. Aprimoramentos de disponibilidade do agente 
  2. Correções de bugs e melhoramentos gerais

## <a name="december-2017"></a>Dezembro de 2017
**Atualização do agente:**

*   Agente do Azure AD Connect Health para AD DS (versão 3.0.145.0)
  1. Aprimoramentos de disponibilidade do agente 
  2. Foram adicionados comandos de resolução de problemas de agente novo
  3. Correções de bugs e melhoramentos gerais
*   Agente do Azure AD Connect Health para AD FS (versão 3.0.145.0)
  1. Foram adicionados comandos de resolução de problemas de agente novo
  2. Aprimoramentos de disponibilidade do agente 
  3. Correções de bugs e melhoramentos gerais
  
## <a name="october-2017"></a>Outubro de 2017
**Atualização do agente:**

 * Agente do Azure AD Connect Health para sincronização (versão 3.0.129.0), lançado com o Azure AD Connect versão 1.1.649.0
<br></br> Foi corrigido um problema de compatibilidade de versão entre o Azure AD Connect e o agente do Azure AD Connect Health para sincronização. Este problema afeta os clientes que estejam a executar o Azure AD Connect no local atualizar para versão 1.1.647.0, mas atualmente tem o agente de estado de funcionamento da versão 3.0.127.0. Após a atualização, o agente de estado de funcionamento não podem mais enviar dados de estado de funcionamento sobre o serviço do Azure AD Connect sincronização ao serviço de estado de funcionamento do Azure AD. Com essa correção, o agente de estado de funcionamento da versão 3.0.129.0 é instalado durante a atualização do Azure AD Connect no local. Agente de estado de funcionamento versão 3.0.129.0 não tem problema de compatibilidade com o Azure AD Connect versão 1.1.649.0.

## <a name="july-2017"></a>Julho de 2017
**Atualização do agente:**

*   Agente do Azure AD Connect Health para AD DS (versão 3.0.68.0)
  1. Correções de bugs e melhoramentos gerais
  2. Suporte de Clouds soberanas
*   Agente do Azure AD Connect Health para AD FS (versão 3.0.68.0)
  1. Correções de bugs e melhoramentos gerais
  2. Suporte de Clouds soberanas
* Agente do Azure AD Connect Health para sincronização (versão 3.0.68.0), lançado com o Azure AD Connect versão 1.1.614.0
  1. Suporte para a Cloud do Microsoft Azure Government e do Microsoft Cloud Alemanha

## <a name="april-2017"></a>Abril de 2017      
**Atualização do agente:**

*   Agente do Azure AD Connect Health para AD FS (versão 3.0.12.0)
  1. Correções de bugs e melhoramentos gerais
*   Agente do Azure AD Connect Health para AD DS (versão 3.0.12.0)
  1. Melhorias de carregar de contadores de desempenho
  2. Correções de bugs e melhoramentos gerais

## <a name="october-2016"></a>Outubro de 2016
**Atualização do agente:**

* Agente do Azure AD Connect Health para AD FS (versão 2.6.408.0)
1. Melhorias na detecção de endereços IP do cliente em pedidos de autenticação
2. Correções de erros relacionados com alertas
* Agente do Azure AD Connect Health para AD DS (versão 2.6.408.0)
1. Correções de erros relacionados com alertas.
* Agente do Azure AD Connect Health para sincronização (versão 2.6.353.0), lançado com o Azure AD Connect versão 1.1.281.0
1. Fornecer os dados necessários para os relatórios de erros de sincronização
2. Correções de erros relacionados com alertas

**Novas funcionalidades de pré-visualização:**

* Relatórios de erros de sincronização para o Azure AD Connect

**Novos recursos:**

* Azure AD Connect Health para AD FS - o campo de endereço IP está disponível no relatório sobre os principais 50 utilizadores com o nome de utilizador/palavra-passe incorreta.

## <a name="july-2016"></a>Julho de 2016
**Novas funcionalidades de pré-visualização:**

* [Azure AD Connect Health para AD DS](how-to-connect-health-adds.md).

## <a name="january-2016"></a>Janeiro de 2016
**Atualização do agente:**

* Agente do Azure AD Connect Health para AD FS (versão 2.6.91.1512)

**Novos recursos:**

* [Ferramenta de teste de conectividade para o Azure AD ligar os agentes de estado de funcionamento](how-to-connect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service)

## <a name="november-2015"></a>Novembro de 2015
**Novos recursos:**

* Suporte para [controlo de acesso baseado em funções](how-to-connect-health-operations.md#manage-access-with-role-based-access-control)

**Novas funcionalidades de pré-visualização:**

* [O Azure AD Connect Health para sincronização](how-to-connect-health-sync.md).

**Problemas fixos:**

* Correções de erros para erros vistos durante os registos de agente.

## <a name="september-2015"></a>Setembro de 2015
**Novos recursos:**

* Relatório de palavra-passe de nome de utilizador de errado para o AD FS
* Suporte para configurar o Proxy de HTTP não autenticados
* Suporte para configurar o agente no Server core
* Melhorias aos alertas para o AD FS
* Carregar a melhorias no agente do Azure AD Connect Health para AD FS para conectividade e dados.

**Problemas fixos:**

* Correções de bugs em informações de utilização para tipos de erros do AD FS.

## <a name="june-2015"></a>Junho de 2015
**Versão inicial do Azure AD Connect Health para AD FS e Proxy do AD FS.**

**Novos recursos:**

* Alertas de monitorização de servidores do AD FS e Proxy do AD FS com notificações por e-mail.
* Obter acesso fácil a topologia do AD FS e padrões nos contadores de desempenho do AD FS.
* Tendência em pedidos de token com êxito nos servidores do AD FS, agrupados por aplicativos, métodos de autenticação, pedir etc de localização de rede.
* Tendências de pedido falhado em servidores do AD FS, agrupados por aplicativos, etc de tipos de erro.
* Implementação mais simples do agente com as credenciais de Administrador Global do Azure AD.  

## <a name="next-steps"></a>Passos Seguintes
Saiba mais sobre [monitorizar os serviços de infraestrutura e a sincronização de identidade de no local na cloud](whatis-hybrid-identity-health.md).

