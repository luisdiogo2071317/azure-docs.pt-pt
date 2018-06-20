---
title: 'Dos serviços de domínio do Azure Active Directory: Funcionalidades | Microsoft Docs'
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
ms.topic: article
ms.date: 05/30/2018
ms.author: maheshu
ms.openlocfilehash: 5dd3cde69c6aa36c3d9cb3060dc6deb59ff74a5a
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36214972"
---
# <a name="azure-ad-domain-services"></a>Azure AD Domain Services
## <a name="features"></a>Funcionalidades
As seguintes funcionalidades estão disponíveis nos domínios geridos de serviços de domínio do Azure AD.

* **Experiência de implementação simples:** pode ativar os serviços de domínio do Azure AD para o diretório do Azure AD com apenas alguns cliques. O domínio gerido inclui as contas de utilizador só de nuvem e de contas de utilizador sincronizadas a partir de um diretório no local.
* **Suporte para a associação de domínio:** pode facilmente computadores de associação de domínio na rede virtual do Azure seu domínio gerido está disponível. A experiência de associação de domínio no cliente Windows e funciona de sistemas operativos de servidor forma totalmente integrada em relação a domínios de manutenção pelos serviços de domínio do Azure AD. Também pode utilizar a associação a um domínio automatizada ferramentas contra desses domínios.
* **Uma instância de um domínio por diretório do Azure AD:** pode criar um único domínio de Active Directory para cada diretório do Azure AD.
* **Criar domínios com nomes personalizados:** pode criar domínios com nomes personalizados (por exemplo, ' contoso100.com') utilizando os serviços de domínio do Azure AD. Pode utilizar qualquer um dos nomes de domínio verificado ou não verificados. Opcionalmente, também pode criar um domínio com o sufixo de domínio incorporada (ou seja, ' *. c o m ") oferecidas pelo seu diretório do Azure AD.
* **Integrado com o Azure AD:** não terá de configurar ou gerir a replicação nos serviços de domínio do Azure AD. Contas de utilizador, as associações de grupo e as credenciais de utilizador (palavras-passe) do diretório do Azure AD estão automaticamente disponíveis nos serviços de domínio do Azure AD. Os novos utilizadores, grupos ou as alterações aos atributos de inquilino do Azure AD ou o seu diretório no local são automaticamente sincronizadas nos serviços de domínio do Azure AD.
* **A autenticação NTLM e Kerberos:** com suporte para a autenticação NTLM e Kerberos, pode implementar as aplicações que dependem Windows-Integrated autenticação.
* **Utilizar a sua empresa credenciais/palavras-passe:** funcionam com os serviços de domínio do Azure AD de inquilino de palavras-passe para os utilizadores no seu Azure AD. Os utilizadores podem utilizar as respetivas credenciais empresariais para máquinas de associação de domínio, iniciar sessão interativamente ou através de ambiente de trabalho remoto e autenticar face ao domínio gerido.
* **Suporte de leitura de enlace de LDAP & LDAP:** pode utilizar as aplicações que dependem LDAP está vinculada a autenticar os utilizadores em domínios de manutenção pelos serviços de domínio do Azure AD. Além disso, as aplicações que utilizam as operações de leitura de LDAP nos atributos de utilizador/computador de consulta do diretório também podem trabalhar nos serviços de domínio do Azure AD.
* **LDAP seguro (LDAPS):** pode ativar o acesso ao diretório ao longo do segura LDAP (LDAPS). Proteger o acesso LDAP está disponível na rede virtual por predefinição. No entanto, também, opcionalmente, pode ativar o acesso LDAP seguro através da internet.
* **Política de grupo:** pode utilizar um único incorporado GPO cada para os utilizadores e computadores contentores para impor a compatibilidade com necessário políticas de segurança para contas de utilizador e de computadores associados a um domínio. Também pode criar os seus próprios GPOs personalizados e atribuí-las a unidades organizacionais personalizadas para [gerir a política de grupo](active-directory-ds-admin-guide-administer-group-policy.md).
* **Faça a gestão de DNS:** membros do grupo 'AAD DC administradores' podem gerir o DNS para o seu domínio gerido utilizando ferramentas de administração de DNS familiares, tais como o snap-in MMC de DNS a administração.
* **Criar personalizado unidades organizacionais (UOs):** membros do grupo 'AAD DC administradores' podem criar UOs personalizados no domínio gerido. Estes utilizadores são concedidos privilégios administrativos completos sobre UOs personalizados, pelo que podem adicionar/remover as contas de serviço, computadores, grupos, etc. dentro destes UOs personalizados.
* **Disponível em várias regiões globais do Azure:** consulte o [serviços do Azure por região](https://azure.microsoft.com/regions/#services/) página saber regiões do Azure em que os serviços de domínio do AD do Azure está disponível.
* **Elevada disponibilidade:** dos serviços de domínio do Azure AD oferece elevada disponibilidade para o seu domínio. Esta funcionalidade oferece a garantia de maior disponibilidade de serviço e maior resiliência às falhas. Estado de funcionamento incorporado monitorização ofertas automatizada remediação de falhas por girar cópias de segurança novas instâncias para substituir instâncias falhadas e fornecer serviço contínuo para o seu domínio.
* **Proteção de bloqueio de conta do AD:** as contas de utilizadores estão bloqueadas durante 30 minutos se as palavras-passe inválidas cinco são utilizadas em dois minutos. As contas são desbloqueadas automaticamente após 30 minutos.
* **Utilizar ferramentas de gestão familiar:** pode utilizar ferramentas de gestão do Windows Server Active Directory familiares, como o Centro de administração do Active Directory ou do Active Directory PowerShell para administrar domínios geridos.
