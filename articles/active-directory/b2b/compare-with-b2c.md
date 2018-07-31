---
title: Comparar colaboração B2B e B2C no Azure Active Directory | Documentos da Microsoft
description: O que é a diferença entre o Azure AD B2C e de colaboração do Azure Active Directory B2B?
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 03/15/2017
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: b59dba541394c105370cfd3af0768a8477ddb4e6
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/30/2018
ms.locfileid: "39348521"
---
# <a name="compare-b2b-collaboration-and-b2c-in-azure-active-directory"></a>Comparar colaboração B2B e B2C no Azure Active Directory

Colaboração B2B do Azure Active Directory (Azure AD) e do Azure AD B2C permitem que trabalhe com utilizadores externos no Azure AD. Mas como são comparadas?


Capacidades de colaboração do B2B |     Oferta de autónoma do Azure AD B2C
-------- | --------
Se destina a: as organizações que pretendem conseguir autenticar os utilizadores de uma organização de parceiro, independentemente do fornecedor de identidade. | Se destina a: convidar os clientes do seu dispositivo móvel e aplicações web, se pessoas, os clientes institucionais ou organizacionais para o Azure AD.
Suportada de identidades: os funcionários com o trabalho ou escola contas, parceiros com contas escolares ou profissionais ou qualquer endereço de e-mail. Em breve para suportar o Federação direta.  | Suportada de identidades: os utilizadores com contas de aplicação local (qualquer e-mail endereço ou nome de utilizador) ou qualquer suportada a identidade de redes sociais com a Federação direta.
O diretório que os usuários de parceiros estão na: utilizadores parceiros da organização externo são geridos no mesmo diretório que os funcionários, mas especialmente anotados. Eles podem ser geridos da mesma forma que os funcionários podem ser adicionados aos mesmos grupos e assim por diante  | O diretório as entidades de utilizador do cliente estão em: no diretório da aplicação. Gerido separadamente a partir do diretório da organização de funcionários e parceiros (se houver.
Início de sessão único (SSO) para todas as aplicações do Azure AD-ligado é suportada. Por exemplo, pode fornecer acesso ao Office 365 ou aplicações no local e para outras aplicações SaaS, como o Salesforce ou Workday.  |  SSO para aplicações nos inquilinos do Azure AD B2C de pertencentes ao cliente é suportada. SSO para o Office 365 ou para outras aplicações SaaS não Microsoft e a Microsoft não é suportada.
Ciclo de vida de parceiro: geridas pelo anfitrião/convidar organização.  | Ciclo de vida do cliente: Self-Service ou gerenciado pela aplicação.
Política de segurança e conformidade: geridas pelo anfitrião/convidar organização (por exemplo, com [políticas de acesso condicional](https://docs.microsoft.com/azure/active-directory/b2b/conditional-access)).  | Política de segurança e conformidade: geridos pela aplicação.
Imagem corporativa: É utilizada a marca de anfitrião/convidando da organização.  |    Imagem corporativa: Geridas pela aplicação. Geralmente, tende a ser o produto de marca, com o esmaecimento de organização no plano de fundo.
Obter mais informações: [mensagem de blogue](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/01/azure-ad-b2b-new-updates-make-cross-business-collab-easy/), [documentação](what-is-b2b.md)  | Obter mais informações: [página do produto](https://azure.microsoft.com/services/active-directory-b2c/), [documentação](https://docs.microsoft.com/azure/active-directory-b2c/)


### <a name="next-steps"></a>Passos Seguintes

- [O que é a colaboração B2B do Azure AD?](what-is-b2b.md)
- [Propriedades de utilizador de colaboração B2B](user-properties.md)

