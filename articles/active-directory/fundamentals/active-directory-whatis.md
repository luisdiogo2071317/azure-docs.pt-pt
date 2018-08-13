---
title: O que é o Azure Active Directory (Azure AD)? | Microsoft Docs
description: Utilize o Azure Active Directory para levar as suas identidades no local existentes para a cloud ou para desenvolver aplicações integradas do Azure AD.
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.author: lizross
ms.assetid: 498820c4-9ebe-42be-bda2-ecf38cc514ca
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 04/09/2018
ms.custom: it-pro
ms.openlocfilehash: 5087f759d682382bc22b5f95f462fe0f08619cc8
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39449999"
---
# <a name="what-is-azure-active-directory"></a>O que é o Azure Active Directory?
O Azure Active Directory (Azure AD) é o serviço de diretórios multi-inquilino, baseados na cloud e de gestão de identidades da Microsoft que junta serviços-base de diretórios, gestão de acesso a aplicações e proteção de identidade numa única solução. O Azure AD também oferece uma plataforma sofisticada baseada em normas que permite aos programadores disponibilizarem controlo de acesso às suas aplicações, baseado em políticas e regras centralizadas.

![Pilha do Azure AD Connect](./media/active-directory-whatis/Azure_Active_Directory.png)

- **Para administradores de TI.** O Azure AD oferece uma solução mais segura à sua organização mediante a utilização de uma gestão de identidades mais forte e de acesso de início de sessão único (SSO) a milhares de [aplicações SaaS baseadas na cloud](../saas-apps/tutorial-list.md) e aplicações no local. Com essas aplicações, também obterá segurança para as aplicações baseada na cloud, acesso ininterrupto, colaboração melhorada e automatização do ciclo de vida das identidades dos seus colaboradores, o que ajuda a aumentar a segurança e a conformidade.

    Além disso, com apenas [quatro cliques](./../connect/active-directory-aadconnect-get-started-express.md), pode integrar o Azure AD numa instância já existente do Windows Server Active Directory, o que permite à sua organização utilizar os investimentos atuais em identidade no local para gerir o acesso a aplicações SaaS baseadas na cloud.

- **Para programadores de aplicações.** O Azure AD permite-lhe concentrar-se nas suas aplicações mediante a integração numa solução de gestão de identidades que é utilizada por milhões de organizações de todo o mundo.

- **Para clientes do Office 365, do Azure ou do Dynamics CRM Online.** Já está a utilizar o Azure AD. Cada inquilino do Office 365, do Azure e do Dynamics CRM Online é, na verdade, um inquilino do Azure AD, com o qual pode começar imediatamente a gerir o acesso dos colaboradores às suas aplicações na cloud integradas.

## <a name="how-reliable-is-azure-ad"></a>Até que ponto é o Azure AD fiável?
O design de multi-inquilino, distribuição geográfica e elevada disponibilidade do Azure AD significa que pode confiar nom mesmo para as necessidades mais críticas do seu negócio. Executado em 28 datacenters em todo o mundo com ativação pós-falha automática, sentir-se-á confortável por saber que o Azure AD é altamente fiável e que, mesmo que um datacenter fique inativo, existem cópias dos dados do seu diretório em pelo menos outros dois datacenters dispersos por outras regiões e disponíveis para acesso instantâneo.

Para obter mais informações sobre os contratos de nível de serviço, veja [Contratos de Nível de Serviço](https://azure.microsoft.com/support/legal/sla/).

## <a name="choose-an-edition"></a>Escolher uma edição
Todos os serviços de negócios do Microsoft Online dependem do Azure Active Directory (Azure AD) para início de sessão e outras necessidades em termos de identidades. Se subscrever algum dos serviços de negócios do Microsoft Online (por exemplo, o Office 365 ou o Microsoft Azure), obterá o Azure AD com acesso a todas as funcionalidades Gratuitas. Com a edição Azure Active Directory Gratuito, pode gerir utilizadores e grupos, sincronizar com diretórios no local, obter início de sessão único no Azure, no Office 365 e em milhares de aplicações SaaS populares como Salesforce, Workday, Concur, DocuSign, Google Apps, Box, ServiceNow, Dropbox, entre outros. 

Para otimizar o Azure Active Directory, pode adicionar capacidades pagas com as edições Azure Active Directory Básico, Premium P1 e Premium P2. As edições pagas do Azure Active Directory são criadas por cima do diretório gratuito já existente, proporcionando capacidades de nível empresarial que vão desde self-service, monitorização melhorada, relatórios de segurança, Multi-Factor Authentication (MFA) e acesso seguro à sua força de trabalho móvel.

> [!NOTE]
> Para obter as opções de preços destas edições, veja [Preços de Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/). O Azure Active Directory Premium P1 e Premium P2 e o Azure Active Directory Básico não são suportados na China, atualmente. Para obter mais informações sobre os preços do Azure AD, pode contactar o Fórum do Azure Active Directory.
>

* **Azure Active Directory Básico** - concebida para trabalhadores de tarefas com necessidades especialmente relativas à cloud, esta edição oferece acesso a aplicações centradas na cloud e soluções de gestão de identidades self-service. Com a edição Básica do Azure Active Directory, beneficia de funcionalidades de otimização de produtividade e de redução de custos, como gestão de acesso baseado em grupos, reposição de palavras-passe self-service para aplicações na cloud e o Proxy de Aplicações do Azure Active Directory (para publicar aplicações Web no local com o Azure Active Directory), com o apoio de um SLA de nível empresarial de 99,9% de tempo de atividade.
* **Azure Active Directory Premium P1** - concebida para capacitar as organizações com necessidades de gestão de identidades e acesso mais exigentes, a edição Azure Active Directory Premium acrescenta capacidades de gestão de identidades com funcionalidades avançadas e de nível empresarial e permite que os utilizadores de soluções híbridas acedam sem interrupções a recursos no local e na cloud. Esta edição inclui tudo aquilo de que precisa para os técnicos de informações e os administradores de identidades em ambientes híbridos, desde acesso de aplicações, gestão de identidades e acesso (IAM) self-service, proteção de identidades a segurança na cloud. Suporta recursos avançados de administração e delegação, como grupos dinâmicos e gestão de grupos personalizada. Inclui o Microsoft Identity Manager (um conjunto de aplicações de gestão de identidades e acesso no local) e oferece capacidades de repetição de escrita de palavras-passe que permitem soluções como a reposição de palavras-passe para os utilizadores no local.
* **Azure Active Directory Premium P2** - concebida com proteção avançada para todos os utilizadores e administradores, esta oferta nova inclui todas as capacidades do Azure AD Premium P1, bem como o Identity Protection e o Privileged Identity Management. O Azure Active Directory Identity Protection tira partido de milhares de milhões de sinais para proporcionar acesso condicional com base em riscos às suas aplicações e a dados críticos da empresa. Também o ajudamos a gerir e a proteger as contas com privilégios com o Azure Active Directory Privileged Identity Management, para que possa detetar, restringir e monitorizar os administradores e o acesso daqueles a recursos, assim como disponibilizar acesso just-in-time quando for necessário.  

> [!NOTE]
> As edições “pay as you go” incluem diversas capacidades do Azure Active Directory:
>
> * O Active Directory B2C é a solução de gestão de identidades e acesso para as aplicações orientadas para o consumidor. Para obter mais informações, veja [Azure Active Directory B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/).
> * O Multi-Factor Authentication do Azure pode ser utilizado através de fornecedores por utilizador ou por autenticação. Para obter mais informações, veja [What is Azure Multi-Factor Authentication?](../authentication/multi-factor-authentication.md) (O que é o Multi-Factor Authentication do Azure?)
>

## <a name="how-can-i-get-started"></a>Como posso começar?

**Se for administrador de TI:**

* [Experimente-o!](https://azure.microsoft.com/trial/get-started-active-directory/) - pode inscrever-se numa avaliação gratuita de 30 dias hoje e implementar a sua primeira solução na cloud em menos de cinco minutos através desta ligação

* Leia [Introdução ao Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium) para ver sugestões e truques sobre como configurar um inquilino do Azure AD rapidamente

**Se for programador:**
 
* veja o [Guia para Programadores](../develop/azure-ad-developers-guide.md) do Azure Active Directory

* [Inicie uma avaliação gratuita](https://azure.microsoft.com/trial/get-started-active-directory/) – inscreva-se numa avaliação gratuita de 30 dias hoje e comece a integrar as suas aplicações no Azure AD

## <a name="next-steps"></a>Passos seguintes
[Learn more about the fundamentals of Azure identity and access management](https://docs.microsoft.com/azure/active-directory/identity-fundamentals) (Saiba mais sobre as noções básicas da gestão de identidades e acesso do Azure)
