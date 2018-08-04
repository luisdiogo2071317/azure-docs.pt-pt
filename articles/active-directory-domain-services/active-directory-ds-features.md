---
title: 'Dos serviços de domínio do Active Directory do Azure: Funcionalidades | Documentos da Microsoft'
description: Funcionalidades dos serviços de domínio do Active Directory do Azure
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 8d1c3eb3-1022-4add-a919-c98cc6584af1
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: maheshu
ms.openlocfilehash: a64401792fd034fde98fe1330340b1ffaa7dfcc1
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39505417"
---
# <a name="azure-ad-domain-services"></a>Azure AD Domain Services
## <a name="features"></a>Funcionalidades
As seguintes funcionalidades estão disponíveis em domínios de geridos do Azure AD Domain Services.

* **Experiência de implementação simples:** pode ativar o Azure AD Domain Services no diretório do Azure AD com apenas alguns cliques. O domínio gerido inclui contas de utilizador apenas na cloud e contas de utilizador sincronizadas a partir de um diretório no local.
* **Suporte para associação a um domínio:** pode facilmente os computadores de associação a um domínio na rede virtual do Azure, o domínio gerido está disponível no. A experiência de associação a um domínio no cliente do Windows e sistemas operativos de servidor funciona perfeitamente em relação a domínios atendidos pelo Azure AD Domain Services. Também pode utilizar a associação a um domínio automatizada de ferramentas em relação a esses domínios.
* **Instância de um domínio por diretório do Azure AD:** pode criar um único domínio do Active Directory para cada diretório do Azure AD.
* **Criar domínios com nomes personalizados:** pode criar domínios com nomes personalizados (por exemplo, ' contoso100.com') utilizando o Azure AD Domain Services. Pode usar qualquer um dos nomes de domínio verificado ou não verificado. Opcionalmente, também pode criar um domínio com o sufixo de domínio internos (ou seja, "*. onmicrosoft.com") oferecidos pelo seu diretório do Azure AD.
* **Integrado com o Azure AD:** não é necessário configurar ou gerir a replicação para o Azure AD Domain Services. Contas de utilizador, associações a grupos e credenciais de utilizador (palavras-passe) do diretório do Azure AD estão automaticamente disponíveis no Azure AD Domain Services. Novos utilizadores, grupos ou as alterações nos atributos de inquilino do Azure AD ou o seu diretório no local são sincronizadas automaticamente com o Azure AD Domain Services.
* **Autenticação NTLM e Kerberos:** com suporte para autenticação NTLM e Kerberos, pode implementar aplicações que dependem Windows-Integrated autenticação.
* **Utilizar as credenciais empresariais/senhas:** funcionam com o Azure AD Domain Services de inquilino de palavras-passe para os utilizadores no seu Azure AD. Os utilizadores podem utilizar as credenciais da empresa para máquinas de associação a um domínio, iniciar sessão interativamente ou ao longo do ambiente de trabalho remoto e autenticar o domínio gerido.
* **Suporte de leitura de enlace de LDAP & LDAP:** pode utilizar as aplicações que dependem une LDAP para autenticar os utilizadores nos domínios atendidos pelo Azure AD Domain Services. Além disso, os aplicativos que usam as operações de leitura de LDAP para atributos de utilizador/computador de consulta do diretório também podem trabalhar em relação a serviços de domínio do Azure AD.
* **LDAP seguro (LDAPS):** pode ativar o acesso ao diretório através de secure LDAP (LDAPS). Acesso de Secure LDAP está disponível na rede virtual por predefinição. No entanto, opcionalmente, também pode ativar o acesso de secure LDAP pela internet.
* **Política de grupo:** pode utilizar um único GPO incorporado cada para os utilizadores e computadores contentores para impor a conformidade com necessário políticas de segurança para contas de usuários e computadores associados a um domínio. Também pode criar seus próprios GPOs personalizados e atribuí-los para unidades organizacionais personalizadas para [gerenciar diretiva de grupo](active-directory-ds-admin-guide-administer-group-policy.md).
* **Gerir o DNS:** membros do grupo "Administradores do AAD DC" podem gerir o DNS para o seu domínio gerido com ferramentas de administração do DNS familiares, como o snap-in MMC de administração do DNS.
* **Criar personalizado unidades organizacionais (UOs):** membros do grupo "Administradores do AAD DC" podem criar UOs personalizadas no domínio gerido. Esses usuários são concedidos privilégios administrativos completos sobre UOs personalizadas, para que eles podem adicionar ou remover contas de serviço, computadores, grupos etc. dentro desses UOs personalizadas.
* **Disponível em muitas regiões globais do Azure:** consulte a [serviços do Azure por região](https://azure.microsoft.com/regions/#services/) página saber as regiões do Azure em que o Azure AD Domain Services está disponível.
* **Elevada disponibilidade:** dos serviços de domínio do Azure AD oferece elevada disponibilidade para o seu domínio. Esta funcionalidade oferece a garantia de tempo de atividade de serviço mais elevado e resiliência a falhas. Estado de funcionamento incorporado monitorização ofertas automatizado remediação de falhas por gerar novas instâncias para substituir instâncias com falha e a fornecer serviço contínuo para o seu domínio.
* **Proteção de bloqueio de conta AD:** as contas de utilizadores estão bloqueadas durante 30 minutos se as palavras-passe inválidas cinco são utilizadas em dois minutos. As contas são automaticamente desbloqueadas após 30 minutos.
* **Utilize ferramentas de gestão familiares:** pode utilizar ferramentas de gestão do Windows Server Active Directory familiares como o Centro de administração do Active Directory ou o Active Directory do PowerShell para administrar os domínios geridos.
