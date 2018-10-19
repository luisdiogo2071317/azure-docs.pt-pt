---
title: Comparar a colaboração B2B e o B2C no Azure Active Directory | Microsoft Docs
description: Qual é a diferença entre a colaboração B2B do Azure Active Directory e o Azure AD B2C?
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: overview
ms.date: 03/15/2017
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 42fbb8b08a2dc24ced436c4a6104f03ae3bca1e9
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "45982815"
---
# <a name="compare-b2b-collaboration-and-b2c-in-azure-active-directory"></a>Comparar a colaboração B2B e o B2C no Azure Active Directory

Quer a colaboração do Azure Active Directory (Azure AD) B2B como o Azure AD B2C permitem trabalhar com utilizadores externos no Microsoft Azure AD. Mas como são comparados?

O **Azure AD B2B** é para as empresas que desejam partilhar ficheiros e recursos com utilizadores externos em segurança para que possam colaborar. Um administrador do Azure configura a B2B no portal do Azure e o Microsoft Azure AD encarrega-se da federação entre a sua empresa e o seu parceiro externo. Os utilizadores iniciam sessão nos recursos partilhados através de um processo de convite e resgate simples com a conta escolar ou profissional ou qualquer conta de e-mail.
 
O **Azure AD B2C** é principalmente para as empresas e programadores que criam aplicações destinadas aos clientes. Com o Azure AD B2C, os programadores podem utilizar o Microsoft Azure AD como o sistema de identidade completo para a aplicação, ao permitir aos clientes iniciarem sessão com uma identidade já estabelecida (como o Facebook ou o Gmail).

A tabela abaixo apresenta uma comparação detalhada.


Capacidades da colaboração B2B |     Oferta autónoma do Azure AD B2C
-------- | --------
Destina-se a: organizações que pretendam conseguir autenticar os utilizadores de uma organização parceira, independentemente do fornecedor de identidade. | Destina-se a: convidar os clientes das suas aplicações móveis e da Web, quer sejam clientes individuais, institucionais ou organizacionais, para o Microsoft Azure AD.
Identidades suportadas: os funcionários ou parceiros com contas escolares ou profissionais, ou quaisquer endereços de e-mail. Em breve, irá suportar a federação direta.  | Identidades suportadas: os utilizadores com contas de aplicações locais (qualquer endereço de e-mail ou nome de utilizador) ou qualquer identidade social suportada com federação direta.
O diretório no qual os utilizadores parceiros se encontram: os utilizadores parceiros da organização externa são geridos no mesmo diretório que os funcionários, mas são especialmente anotados. Estes podem ser geridos da mesma forma que os funcionários, podem ser adicionados aos mesmos grupos e assim por diante  | O diretório no qual as entidades de utilizador se encontram: no diretório da aplicação. Gerido separadamente a partir do diretório de funcionários e parceiros da organização (se existir).
É suportado o início de sessão único (SSO) para todas as aplicações ligadas ao Microsoft Azure AD. Por exemplo, pode conceder acesso ao Office 365 ou às aplicações no local e a outras aplicações SaaS, como o Salesforce ou Workday.  |  É suportado o SSO em aplicações pertencentes aos cliente nos inquilinos do Azure AD B2C. O SSO no Office 365 ou outras aplicações SaaS da Microsoft e que não são da Microsoft não são suportados.
Ciclo de vida do parceiro: gerido pelo anfitrião/organização que convida.  | Ciclo de vida do cliente: gerido automaticamente ou gerido pela aplicação.
Política de segurança e conformidade: geridas pelo anfitrião/organização que convida (por exemplo, com [políticas de acesso condicional](https://docs.microsoft.com/azure/active-directory/b2b/conditional-access)).  | Política de segurança e conformidade: geridas pela aplicação.
Imagem corporativa: é utilizada a marca do anfitrião/organização que convida.  |    Imagem corporativa: gerida pela aplicação. Geralmente, tende a ser personalizada com base no produto, com a organização a desvanecer no fundo.
Mais informações: [Mensagem de blogue](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/01/azure-ad-b2b-new-updates-make-cross-business-collab-easy/), [Documentação](what-is-b2b.md)  | Mais informações: [Página do produto](https://azure.microsoft.com/services/active-directory-b2c/), [Documentação](https://docs.microsoft.com/azure/active-directory-b2c/)


### <a name="next-steps"></a>Passos seguintes

- [O que é a colaboração B2B do Azure AD?](what-is-b2b.md)
- [Propriedades do utilizador de colaboração B2B](user-properties.md)

