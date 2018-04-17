---
title: O que é o Azure Active Directory (Azure AD)? | Microsoft Docs
description: Utilize o Azure Active Directory para expandir as identidades no local existentes para a nuvem ou para desenvolver aplicações do Azure AD integrado.
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.author: lizross
ms.assetid: 498820c4-9ebe-42be-bda2-ecf38cc514ca
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/09/2018
ms.custom: it-pro
ms.openlocfilehash: 003ce2edda3e2069eb7e05f58ecc2e208c818946
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="what-is-azure-active-directory"></a>O que é o Azure Active Directory?
Azure Active Directory (Azure AD) é o diretório de multi-inquilino, baseado na nuvem da Microsoft e serviço de gestão de identidade que combina serviços de diretório de principais, gestão de acesso de aplicações e proteção de identidade numa solução única. Do Azure AD também oferece uma plataforma avançada, baseada em normas que permite aos programadores fornecer controlo de acesso para as respetivas aplicações, com base na política centralizada e regras.

![Pilha do Azure AD Connect](./media/active-directory-whatis/Azure_Active_Directory.png)

- **Para administradores de TI.** O Azure AD fornece uma solução mais segura para a sua organização através da utilização de mais forte gestão de identidades e acesso início de sessão único (SSO) para milhares de [baseado na nuvem aplicações de SaaS](active-directory-saas-tutorial-list.md) e aplicações no local. Através destas aplicações, também irá obter a segurança de aplicação baseada na nuvem, acesso totalmente integrado, colaboração melhorada e automatização o ciclo de vida de identidade para os seus empregados, que ajuda a aumentar a segurança e conformidade.

    Além disso, com apenas [quatro cliques](./connect/active-directory-aadconnect-get-started-express.md), pode integrar o Azure AD com um Windows Server Active Directory existente, permitindo que a sua organização utilizar existente no local a aceder investimentos de identidade para gerir aplicações de SaaS baseado na nuvem .

- **Para os programadores de aplicações.** Azure AD permite ao que se concentre em criar as suas aplicações, permitindo-lhe integra com uma solução de gestão de identidade que é utilizada pelo milhões de organizações em todo o mundo.

- **Para clientes do Office 365, Azure ou Dynamics CRM Online.** Já está a utilizar o Azure AD. Cada inquilino do Office 365, Azure e o Dynamics CRM Online é, na verdade, um inquilino do Azure AD, permitindo-lhe iniciar imediatamente gerir o acesso dos empregados às suas aplicações em nuvem integrado.

## <a name="how-reliable-is-azure-ad"></a>Como fiável é o Azure AD?
A estrutura de multi-inquilino, geo-distribuição, de elevada disponibilidade do Azure AD significa que pode confiar na mesma para as suas necessidades de negócio mais importantes. A ficar sem centros de dados de 28 em todo o mundo com ativação pós-falha automática, terá de comfort de saber que do Azure AD é altamente fiável e que, mesmo se um centro de dados fica inativo, cópias dos seus dados do diretório estão em direto em, pelo menos, duas mais regionally dispersos dados os centros e disponíveis para acesso instantânea.

Para mais informações sobre contratos de nível de serviço, consulte [contratos de nível de serviço](https://azure.microsoft.com/support/legal/sla/).

## <a name="choose-an-edition"></a>Escolher uma edição
Todos os serviços empresariais do Microsoft Online baseiam-se no Azure Active Directory (Azure AD) para início de sessão e outra identidade necessidades. Se subscrever qualquer um dos serviços de empresas do Microsoft Online (por exemplo, o Office 365 ou o Microsoft Azure), obtenha do Azure AD com acesso a todas as funcionalidades livres. Com a edição gratuito do Azure Active Directory, pode gerir utilizadores e grupos, sincronize com diretórios no local, obter o início de sessão único através do Azure, Office 365 e milhares de aplicações SaaS populares, como o Salesforce Workday, Concur, DocuSign, Google As aplicações, caixa, ServiceNow, Dropbox e muito mais. 

Para melhorar o Azure Active Directory, pode adicionar capacidades pagas utilizando as edições básico do Azure Active Directory, Premium P1 e Premium P2. Azure Active Directory paga edições assentes livre diretório existente, fornecer enterprise capacidades de classe expansão avançada self-service, monitorização, segurança de relatórios, multi-factor Authentication (MFA) e acesso seguro para o força de trabalho móvel.

> [!NOTE]
> Para as opções de preços destas edições, consulte [preços do Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/). Azure Active Directory Premium P1, Premium P2 e básico do Azure Active Directory não são atualmente suportadas na China. Para obter mais informações sobre preços do Azure AD, pode contactar o Fórum do Azure Active Directory.
>

* **Azure Active Directory básico** -concebido para trabalhadores de tarefas com as necessidades de nuvem primeiro, nesta edição fornece acesso e Self-Service a identidade da aplicação de nuvem centrada soluções de gestão. Com a edição Básica do Azure Active Directory, beneficia de funcionalidades de otimização de produtividade e de redução de custos, como gestão de acesso baseado em grupos, reposição de palavras-passe self-service para aplicações na cloud e o Proxy de Aplicações do Azure Active Directory (para publicar aplicações Web no local com o Azure Active Directory), com o apoio de um SLA de nível empresarial de 99,9% de tempo de atividade.
* **Azure Active Directory Premium P1** - concebida para organizações com pedir o seu trabalho mais de capacitar tem de gestão de identidades e acessos, edição do Azure Active Directory Premium adiciona funcionalidades de gestão de identidade de nível empresarial avançada em termos de funcionalidade e permite aos utilizadores híbrida totalmente integrada aceder no local e as capacidades de nuvem. Esta edição inclui tudo o que precisa para o técnico de informação e administradores de identidade em ambientes híbridos através de acesso de aplicação, de identidade self-service e gestão de acesso (IAM), proteção de identidade e segurança na nuvem. Suporta recursos de administração e a delegação avançados, como grupos dinâmicos e de gestão de grupos self-service. Inclui o Microsoft Identity Manager (um local identidades e acessos management suite) e fornece capacidades de repetição de escrita ativar soluções como self-service reposição palavra-passe para os seus utilizadores no local de nuvem.
* **Azure Active Directory Premium P2** -concebidas com proteção avançada para todos os seus utilizadores e administradores, esta nova oferta inclui todas as capacidades no Azure AD Premium P1, bem como a proteção de identidade e Privileged Identity Management. Azure Active Directory Identity Protection tira partido billions dos sinais para fornecer acesso condicional baseado em risco aos seus dados críticos da empresa e aplicações. Iremos também ajuda a gerir e proteger contas privilegiadas com o Azure Active Directory Privileged Identity Management, pelo que pode detetar, restringir e monitorize os administradores e o respetivo acesso aos recursos e fornecem acesso just-in-time quando necessário.  

> [!NOTE]
> Um número de capacidades do Azure Active Directory está disponível nas várias edições de "pay as you go":
>
> * Active Directory B2C é a solução de gestão de identidades e acessos para as suas aplicações direcionadas para o consumidor. Para obter mais informações, consulte [Azure Active Directory B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/)
> * Multi-factor Authentication do Azure pode ser utilizada através por utilizador ou por fornecedores de autenticação. Para obter mais informações, consulte [que é o Azure multi-factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)
>

## <a name="how-can-i-get-started"></a>Como pode começar a utilizar?

**Se for um administrador de TI:**

* [Experimente!](https://azure.microsoft.com/trial/get-started-active-directory/) -Pode inscrever-se para uma avaliação gratuita de 30 dias hoje e implementar a sua primeira solução em nuvem em cinco minutos, utilizando a seguinte hiperligação

* Leitura [introdução ao Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium) para truques sobre a obtenção de um Azure AD e sugestões de inquilino e em execução rápida

**Se for um programador:**
 
* Veja o [guia para programadores](active-directory-developers-guide.md) ao Azure Active Directory

* [Iniciar uma avaliação](https://azure.microsoft.com/trial/get-started-active-directory/) – inscrever para uma avaliação gratuita de 30 dias hoje e começar a integrar as suas aplicações com o Azure AD

## <a name="next-steps"></a>Passos Seguintes
[Saiba mais sobre as noções básicas sobre a gestão de identidades e acessos do Azure](https://docs.microsoft.com/azure/active-directory/identity-fundamentals)
