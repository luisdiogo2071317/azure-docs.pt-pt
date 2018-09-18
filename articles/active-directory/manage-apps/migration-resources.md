---
title: Recursos para a migração de aplicações para o Azure Active Directory | Documentos da Microsoft
description: Recursos para ajudar a migrar o acesso à aplicação e a autenticação para o Azure Active Directory (Azure AD).
services: active-directory
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 09/16/2018
ms.author: barbkess
ms.reviewer: baselden
ms.openlocfilehash: 950f315ae5cbeba678aceb8e901ec9bc235a07da
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/17/2018
ms.locfileid: "45740699"
---
# <a name="resources-for-migrating-applications-to-azure-active-directory"></a>Recursos para a migração de aplicativos para o Azure Active Directory

Recursos para ajudar a migrar o acesso à aplicação e a autenticação para o Azure Active Directory (Azure AD). 

| Recurso  | Descrição  |
|:-----------|:-------------|
|[Migrar seus aplicativos para o Azure AD](https://aka.ms/migrateapps/whitepaper) | Este white paper apresenta as vantagens da migração e descreve como planear a migração em quatro fases, claramente descritos: deteção, classificação, migração e gerenciamento contínuo. Vai ser orientado ao longo como pensar sobre o processo e dividir seu projeto em partes de fácil compreensão. Ao longo do documento são links para recursos importantes que ajudarão ao longo do caminho. |
| [Plano de implementação: Migrar do AD FS para sincronização de hash de palavra-passe](https://aka.ms/ADFSTOPHSDPDownload) | Com a sincronização de hash de palavra-passe, os hashes de palavras-passe do utilizador são sincronizados do Active Directory no local para o Azure AD. Isso permite que o Azure AD para autenticar os utilizadores sem a interação com o Active Directory no local.| 
| [Plano de implementação: Migrar do AD FS para autenticação pass-through](https://aka.ms/ADFSTOPTADPDownload)|Autenticação pass-through do Azure AD ajuda os utilizadores que inicie sessão no local e aplicações baseadas na cloud com a mesma palavra-passe. Esta funcionalidade fornece os seus utilizadores com uma melhor experiência, uma vez que eles têm um menos palavra-passe para se lembrar. Também reduz os custos de suporte técnico de TI vez que os usuários têm menos probabilidade de esquecer como iniciar sessão, quando apenas precisarem de Lembre-se de uma palavra-passe. Quando as pessoas iniciam sessão com o Azure AD, esta funcionalidade valida as respetivas palavras-passe diretamente no seu Active Directory no local.|
| [Plano de implementação: Ativar o início de sessão único para uma aplicação SaaS com o Azure AD](https://aka.ms/SSODPDownload) | Única início de sessão (SSO) ajuda a que aceder a todas as aplicações e recursos de que precisa para fazer negócio, ao iniciar sessão apenas uma vez, com uma conta de utilizador único. Por exemplo, após um utilizador tem sessão iniciada, o utilizador pode mover da Microsoft Office, ao SalesForce, a caixa sem autenticação (por exemplo, escrever uma palavra-passe) uma segunda vez. 
| [Plano de implementação: alargamento das aplicações para o Azure AD com o Proxy de aplicações](https://aka.ms/AppProxyDPDownload)| Fornecer acesso a partir de computadores portáteis de funcionários e outros dispositivos para locais aplicativos sempre envolveu redes privadas virtuais (VPNs) ou desmilitarizada zonas (DMZs). Estas soluções não só são complexas e difíceis de proteger, como também são caras em termos de configuração e gestão. Proxy de aplicações do Azure AD torna mais fácil para aceder a aplicações no local. |
| [Planos de implantação](../fundamentals/active-directory-deployment-plans.md) | Encontre mais planos de implementação para a implementação de recursos, como a autenticação Multifator, acesso condicional, aprovisionamento de utilizadores, totalmente integrada SSO, reposição de palavra-passe self-service e muito mais! |


