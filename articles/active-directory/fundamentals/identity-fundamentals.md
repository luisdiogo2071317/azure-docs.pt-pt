---
title: Noções básicas da gestão de identidades do Azure | Microsoft Docs
description: As identidades baseadas na cloud são agora a melhor forma de manter o controlo e saber como e quando é que os utilizadores acedem às aplicações e aos dados da empresa.
keywords: ''
author: eross-msft
manager: mtillman
ms.reviewer: jsnow
ms.author: lizross
ms.date: 08/07/2018
ms.topic: overview
ms.prod: ''
ms.service: active-directory
ms.component: fundamentals
ms.technology: ''
ms.assetid: ''
ms.custom: it-pro
ms.openlocfilehash: 797c35bad03c063203e3616740af633b71835a9c
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39640280"
---
# <a name="fundamentals-of-azure-identity-management"></a>Noções básicas da gestão de identidades do Azure

À medida que cada vez mais recursos digitais das empresas residem fora das redes empresariais, na cloud e em dispositivos, tornou-se necessário recorrer a soluções de gestão de acesso e identidades baseada na cloud eficientes. As identidades baseadas na cloud são agora a melhor forma de manter o controlo e saber como e quando é que os utilizadores acedem às aplicações e aos dados da empresa.

A Microsoft protege as identidades baseadas na cloud há mais de uma década e, agora, com o [Azure Active Directory (AD)](active-directory-whatis.md), os mesmos sistemas de proteção estão disponíveis para si. Com o Azure AD, os administradores empresariais podem facilmente assegurar a responsabilidade dos utilizadores e administradores com segurança e governação melhores do que nunca.

O Azure AD Premium é uma solução de gestão de acesso e identidades baseada na cloud com capacidades de proteção avançadas e que permite uma identidade segura para todas as aplicações e ativa a proteção de identidade (otimizada pelo [grafo de Segurança Inteligente da Microsoft](https://www.microsoft.com/en-us/security/intelligence) e o Privileged Identity Management. Sendo muito mais do que apenas outra ferramenta de monitorização ou criação de relatórios, o Azure AD Premium pode proteger as identidades dos seus utilizadores em tempo real e permite-lhe criar políticas de acesso adaptáveis e baseadas no risco para proteger os dados da sua organização.

Assista este breve vídeo e veja uma descrição geral da gestão e da proteção de identidades do Azure AD.
>[!VIDEO https://www.youtube.com/embed/9LGIJ2-FKIM]

A Microsoft não só lhe fornece uma identidade com a qual pode aceder a qualquer sítio, mas também um conjunto de ferramentas para automatizar, ajudar a proteger e a gerir as TI na sua organização. Mesmo após o advento da informática na cloud, continua a haver procura para a gestão e controlo de tarefas de TI, como chamadas para o suporte técnico para repor palavras-passe, gestão de grupos de utilizadores e pedidos de aplicações. Para complicar ainda mais as coisas, os colaboradores começaram a trazer os dispositivos pessoais para o trabalho e a utilizar aplicações SaaS prontamente disponíveis, o que faz com que manter o controlo sobre as aplicações deles nos vários datacenters da empresa e em plataforma de cloud pública seja um verdadeiro desafio.

[!INCLUDE [identity](../../../includes/azure-ad-licenses.md)]

## <a name="connect-on-premises-active-directory-with-azure-ad-and-office-365"></a>Ligar o Active Directory no local ao Azure AD e ao Office 365
As organizações que tenham feito investimentos avultados no Active Directory no local podem transferi-los para a cloud, mediante a integração dos diretórios no local no Azure AD, criando uma [gestão de identidades híbrida](https://aka.ms/aadframework). Desta forma, os seus utilizadores tornam-se mais produtivos, pois recebem uma identidade comum para aceder aos recursos, independentemente da localização. Depois, os utilizadores e as organizações podem utilizar o início de sessão único (SSO) para aceder a recursos no local e a serviços cloud, como o Office 365.

O [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) é a única ferramenta de que precisa para concluir a integração. Proporciona capacidades que suportam as suas necessidades de sincronização de identidades e substitui as versões mais antigas das ferramentas de integração, como o DirSync e o Azure AD Sync. Com o Azure AD Connect, a gestão e sincronização de identidades entre o local e o Azure AD é feita através de:

- Sincronização – este componente é responsável pela criação de utilizadores, grupos e outros objetos. Também é responsável por verificar se as informações de identidade dos seus utilizadores e grupos no local têm correspondência na nuvem. Também é possível ativar a repetição de escrita de palavras-passe para manter os diretórios no local em sincronização quando um utilizador atualiza a palavra-passe no Azure AD.
- Autenticação – tendo o Azure AD como o seu novo plano de controlo, a autenticação é a base do acesso na cloud. Escolher o método de autenticação certo é uma decisão fundamental para a configuração de uma solução de identidade híbrida do Azure AD. Reveja [este guia](https://aka.ms/auth-options) para escolher entre a autenticação na cloud (Sincronização de Hash de Palavras-passe) / Autenticação Pass-through) ou a autenticação federada (AD FS) para a sua organização.
- Monitorização do Estado de Funcionamento – [o Azure AD Connect Health](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health) pode proporcionar uma monitorização robusta e uma localização central no portal do Azure para visualizar esta atividade.

## <a name="increase-productivity-and-reduce-helpdesk-costs-with-self-service-and-single-sign-on-experiences"></a>Aumentar a produtividade e reduzir os custos de suporte técnico com as experiências de self-service e início de sessão único

Os colaboradores tornam-se mais produtivos se tiverem de memorizar apenas uma combinação de nome de utilizador e palavra-passe e beneficiarem de uma experiência consistente em todos os dispositivos. Também poupam tempo se puderem realizar tarefas self-service, como [repor palavras-passe esquecidas](https://docs.microsoft.com/azure/active-directory/active-directory-passwords), ou pedir acesso a uma aplicação sem terem de esperar por assistência do suporte técnico.

O Azure AD [expande o Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) para a cloud, permitindo aos utilizadores utilizar as respetivas contas principais da organização nos dispositivos deles associados ao domínio, no acesso aos recursos da empresa e em todas as aplicações Web e SaaS de que precisam para trabalhar. Para além de não terem de memorizar várias combinações de nomes de utilizador e palavras-passe, o acesso às aplicações dos utilizadores também pode ser aprovisionado automaticamente (ou desaprovisionado) com base nas adesões deles a grupos e de acordo com o estatuto de colaborador na empresa. E também pode controlar esse acesso para aplicações da galeria ou para as suas próprias aplicações no local que tenha desenvolvido e publicado através do [Proxy de Aplicações do AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started).

## <a name="manage-and-control-access-to-corporate-resources"></a>Gerir e controlar o acesso a recursos empresariais
As soluções de gestão de acesso e identidades da Microsoft ajudam as equipas de TI a proteger o acesso a aplicações e a recursos nos vários datacenters empresariais e na cloud, permitindo mais níveis de validação, como a [autenticação multifator](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next) e as [políticas de acesso condicional](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). A monitorização de atividades suspeitas através de relatórios de segurança avançada, de auditorias e de alertas ajuda a mitigar potenciais problemas de segurança.

As políticas de acesso condicional oferecem-lhe a si, o administrador da empresa, a capacidade de criar regras de acesso baseado em políticas para qualquer aplicação ligada ao Azure AD (aplicações SaaS, aplicações personalizadas em execução na cloud ou aplicações Web no local). O Azure AD avalia essas políticas em tempo real e aplica-as sempre que um utilizador tentar aceder a uma aplicação. Com as políticas de proteção de identidades do Azure, pode tomar medidas automaticamente caso sejam detetadas atividades suspeitas. Essas ações podem incluir o bloqueio do acesso a utilizadores em risco, a aplicação da autenticação multifator e a reposição das palavras-passe dos utilizadores, se houver suspeitas de que as credenciais estejam comprometidas.


## <a name="azure-active-directory-privileged-identity-management"></a>Gestão de Identidades Privilegiadas do Azure Active Directory

O [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started), incluído na oferta Azure Active Directory Premium P2 offering, permite-lhe detetar, restringir e monitorizar contas administrativas e o acesso das mesmas a recursos no Azure Active Directory e noutros serviços online da Microsoft. Também ajuda a administrar o acesso administrativo a pedido para o período de tempo exato que for necessário.

O Privileged Identity Management pode aplicar direitos de administrador a pedido, para que os administradores possam pedir a elevação com autenticação multifator ativada temporária dos respetivos privilégios para períodos de tempo pré-configurados antes de as contas regressarem ao estado de utilizador normal.

## <a name="benefits-of-azure-identity"></a>Benefícios da Identidade do Azure

Com a gestão de identidades do Azure, pode:

-   Criar e gerir uma única identidade para cada utilizador em toda a sua empresa, mantendo os utilizadores, grupos e dispositivos sincronizados com o [Azure Active Directory Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

-   Disponibilizar o acesso de início de sessão único às aplicações, incluindo a milhares de aplicações SaaS pré-integradas, ou fornecer acesso remoto seguro a aplicações SaaS no local com o [Proxy de Aplicações do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started).

-   Ativar a segurança do acesso a aplicações mediante a aplicação da [Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next) baseada em regras, tanto nas aplicações no local, como na cloud.

-   Melhorar a produtividade dos utilizadores com a [reposição personalizada de palavra-passe](https://docs.microsoft.com/azure/active-directory/active-directory-passwords) e com os pedidos de acesso a grupos e aplicações através do [portal MyApps](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-user-help).

-   Tirar partido da [elevada disponibilidade e fiabilidade](https://docs.microsoft.com/azure/architecture/resiliency/high-availability-azure-applications) de uma solução de gestão de acesso e identidades baseada na cloud de nível empresarial e global.

## <a name="next-steps"></a>Passos seguintes
[Saiba mais sobre as soluções de identidade do Azure](https://docs.microsoft.com/azure/active-directory/understand-azure-identity-solutions)
