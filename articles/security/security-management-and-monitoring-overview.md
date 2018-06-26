---
title: Gestão de segurança do Azure e a descrição geral da monitorização | Microsoft Docs
description: Este artigo fornece uma descrição geral das funcionalidades de segurança e os serviços fornecidos pelo Azure para ajudar na gestão e monitorização do cloud services do Azure e máquinas virtuais.
services: security
documentationcenter: na
author: TerryLanfear
manager: StevenPo
editor: TomSh
ms.assetid: 5cf2827b-6cd3-434d-9100-d7411f7ed424
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: terrylan
ms.openlocfilehash: 9e538ac39af5b6df44860a4a70b0fd1e058c060c
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36752293"
---
# <a name="azure-security-management-and-monitoring-overview"></a>Gestão de segurança do Azure e a descrição geral da monitorização
O Azure oferece mecanismos de segurança para ajudar na gestão e monitorização do cloud services do Azure e máquinas virtuais (VMs). Este artigo fornece uma descrição geral destas funcionalidades de segurança de núcleo e serviços. São fornecidas hiperligações para artigos que fornecer detalhes de cada, pelo que pode saber mais.

A segurança dos seus serviços em nuvem do Microsoft é uma parceria e uma responsabilidade partilhada entre si e a Microsoft. Microsoft é responsável pela plataforma do Azure e a segurança física dos respetivos centros de dados (ao utilizar proteções de segurança como portas de entrada de distintivo bloqueado, fences e guards). O Azure oferece fortes níveis de segurança de nuvem na camada de software que satisfaça as necessidades de segurança, privacidade e conformidade dos seus clientes.

Proprietário os dados e identidades, a responsabilidade de protegê-los, a segurança dos seus recursos no local e a segurança dos componentes de nuvem através do qual tem controlo. Microsoft dá-lhe controlos de segurança e funcionalidades para o ajudar a proteger os seus dados e aplicações. O grau de responsabilidade de segurança baseia-se no tipo de serviço em nuvem.

O gráfico seguinte resume o equilíbrio de responsabilidade entre a Microsoft e o cliente.

![Responsabilidade partilhada][1]

Para obter mais informações sobre a gestão de segurança, consulte [gestão de segurança no Azure](azure-security-management.md).

## <a name="role-based-access-control"></a>Controlo de Acesso Baseado em Funções
Controlo de acesso baseado em funções (RBAC) permite a gestão de acesso detalhada para recursos do Azure. Ao utilizar o RBAC, pode conceder pessoas apenas a quantidade de acesso que precisam para desempenhar as suas funções. RBAC também pode ajudar a garantir que quando as pessoas deixam a organização, se perderem o acesso a recursos na nuvem.

Saiba mais:

* [Blogue de equipa do Active Directory em RBAC](https://cloudblogs.microsoft.com/enterprisemobility/?product=azure-active-directory)
* [Controlo de acesso baseado em funções do Azure](../role-based-access-control/role-assignments-portal.md)

## <a name="antimalware"></a>Antimalware
Com o Azure, pode utilizar o software antimalware dos fornecedores de principais de segurança, tais como Microsoft, da Symantec, Micro de tendência, McAfee e Kaspersky. Este software ajuda a proteger as máquinas virtuais a partir de ficheiros maliciosos, adware e outras ameaças.

Antimalware da Microsoft para máquinas virtuais e serviços Cloud do Azure oferece a capacidade de instalar um agente de antimalware para funções de PaaS e máquinas virtuais. Com base no System Center Endpoint Protection, esta funcionalidade coloca comprovado no local tecnologia de segurança para a nuvem.

Que oferecemos também a integração profunda do tendência [segurança profunda](http://www.trendmicro.com/us/enterprise/cloud-solutions/deep-security/) e [SecureCloud](http://www.trendmicro.com/us/enterprise/cloud-solutions/secure-cloud/) produtos na plataforma do Azure. Segurança avançada é uma solução de antivírus e SecureCloud é uma solução de encriptação. Segurança avançada é implementada dentro de VMs através de um modelo de extensão. Utilizando a IU do portal do Azure e o PowerShell, pode optar por utilizar segurança avançada dentro de novas VMs que estão a ser aceleradas ou VMs existentes que já estão implementadas.

Symantec Endpoint Protection (SEP) também é suportada no Azure. Através da integração do portal, pode especificar que pretende utilizar SEP numa VM. SEP pode ser instalado numa VM nova através do portal do Azure ou pode ser instalado numa VM existente através do PowerShell.

Saiba mais:

* [Implementar Soluções Antimalware em Máquinas Virtuais do Azure](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
* [Antimalware da Microsoft para máquinas virtuais e serviços em nuvem do Azure](azure-security-antimalware.md)
* [Como instalar e configurar a segurança avançada do tendência Micro como um serviço numa VM do Windows](../virtual-machines/windows/classic/install-trend.md)
* [Como instalar e configurar o Symantec Endpoint Protection numa VM do Windows](../virtual-machines/windows/classic/install-symantec.md)
* [Novas opções de Antimalware para proteger máquinas virtuais do Azure](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication
Multi-factor Authentication do Azure é um método de autenticação que requer a utilização de mais do que um método de verificação. Adiciona uma segunda camada crítica de segurança para inícios de sessão de utilizador e de transações. 

Multi-factor Authentication ajuda a salvaguardar o acesso a dados e aplicações, cumprindo o pedido do utilizador para um processo de início de sessão simple. Fornece autenticação forte através de uma variedade de opções de verificação (chamada telefónica, mensagem de texto ou notificação ou da verificação de código da aplicação móvel) e tokens OATH de terceiros.

Saiba mais:

* [Multi-Factor Authentication](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
* [O que é o Multi-Factor Authentication do Azure?](../active-directory/authentication/multi-factor-authentication.md)
* [Como funciona o Azure multi-factor Authentication](../active-directory/authentication/concept-mfa-howitworks.md)

## <a name="expressroute"></a>ExpressRoute
Pode utilizar o Azure ExpressRoute para expandir as suas redes no local para o Microsoft Cloud através de uma ligação privada dedicada que é facilitada por um fornecedor de conectividade. Com o ExpressRoute, pode estabelecer ligações a serviços cloud Microsoft como o Azure, Office 365 e o CRM Online. A conectividade pode ser a partir de:

- Uma rede de qualquer a qualquer (VPN de IP).
- Uma rede de Ethernet ponto a ponto.
- Uma ligação cruzada virtual através de um fornecedor de conectividade a uma instalação de colocalização. 

As ligações do ExpressRoute não passam para a internet pública. Pode oferecem mais fiabilidade, velocidades mais rápidas, latências inferiores e uma maior segurança ligações típicas através da internet.

Saiba mais:

* [Descrição geral técnica do ExpressRoute](../expressroute/expressroute-introduction.md)

## <a name="virtual-network-gateways"></a>Gateways de rede virtual
Gateways de VPN, também denominados gateways de rede virtual do Azure, são utilizados para enviar tráfego de rede entre redes virtuais e localizações no local. Também são utilizadas para enviar tráfego entre várias redes virtuais no Azure (a rede de rede). Gateways de VPN fornecem uma conectividade entre instalações segura entre o Azure e a infraestrutura.

Saiba mais:

* [Acerca dos gateways de VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md)
* [Descrição geral de segurança de rede do Azure](security-network-overview.md)

## <a name="privileged-identity-management"></a>Privileged Identity Management
Por vezes, os utilizadores precisam realizar operações privilegiadas de recursos do Azure ou outras aplicações SaaS. Isto, muitas vezes, significa que as organizações conceder-lhes acesso privilegiado permanente no Azure Active Directory (Azure AD). 

Este é um risco de segurança crescente para recursos alojados na nuvem, porque as organizações não é possível monitorizar suficientemente que esses utilizadores estão a fazer com o respetivo acesso privilegiado. Além disso, se uma conta de utilizador com acesso privilegiado for comprometida, que uma violação pode afetar a segurança de nuvem global da organização. O Azure AD Privileged Identity Management ajuda a resolver este risco, diminuindo o tempo de exposição de privilégios e aumentar a visibilidade em utilização.  

Privileged Identity Management apresenta o conceito de um administrador temporário para uma função ou "just in time" acesso de administrador. Este tipo de admin é um utilizador que precisa para concluir um processo de ativação para que a função atribuída. O processo de ativação é alterado a atribuição do utilizador para uma função no Azure AD do período inativo para o Active Directory, durante um período de tempo especificado.

Saiba mais:

* [Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-configure.md)
* [Introdução ao Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-getting-started.md)

## <a name="identity-protection"></a>Identity Protection
Azure AD Identity Protection fornece uma vista consolidada de início de sessão atividades suspeitas e potenciais vulnerabilidades para ajudar a proteger a sua empresa. Proteção de identidade Deteta atividades suspeitas para utilizadores e de identidades de privilégio (administrador), com base no sinais como:

- Ataques de força bruta.
- Credenciais obtidas ilicitamente.
- Inícios de sessão de localizações desconhecidas e de dispositivos infetados.

Ao fornecer notificações e a remediação recomendada, Identity Protection ajuda a mitigar os riscos em tempo real. Calcula a gravidade do risco de utilizador. Pode configurar as políticas baseadas em risco para o ajudar automaticamente a aplicação de salvaguarda contra ameaças futuras de acesso.

Saiba mais:

* [Proteção de identidade do Azure Active Directory](../active-directory/active-directory-identityprotection.md)
* [Canal 9: Do Azure AD e mostrar de identidade: identidade pré-visualização de proteção](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="security-center"></a>Centro de Segurança
Centro de segurança do Azure ajuda-o a evitar, detetar e responder a ameaças. Oferece do Centro de segurança maior visibilidade e controlo sobre a segurança dos seus recursos Azure. Fornece gestão de políticas e monitorização de segurança integrada nas suas subscrições do Azure. Ajuda a detetar ameaças que caso contrário podem passar despercebidas e funciona com um ecossistema abrangente de soluções de segurança.

Centro de segurança ajudam a otimizar e monitorizar a segurança dos seus recursos do Azure por:

* Ativar definir políticas para os seus recursos de subscrição do Azure de acordo com:
  * Tem de segurança da sua empresa.
  * O tipo de aplicações ou sensibilidade dos dados em cada subscrição.
* Monitorizar o estado das máquinas virtuais do Azure, das redes e aplicações.
* Fornecer uma lista de alertas de segurança prioritários, incluindo alertas de soluções de parceiros integradas. Também fornece as informações necessárias para investigar rapidamente um ataque e recomendações sobre como resolvê-lo.

Saiba mais:

* [Introdução ao Centro de Segurança do Azure](../security-center/security-center-intro.md)

<!--Image references-->
[1]: ./media/security-management-and-monitoring-overview/shared-responsibility.png
