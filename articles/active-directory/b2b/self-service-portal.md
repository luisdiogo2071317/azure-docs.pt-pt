---
title: Portal de inscrição self-service para colaboração B2B do Azure Active Directory do | Microsoft Docs
description: A colaboração do B2B Azure Active Directory suporta as relações entre empresas, permitindo a parceiros de negócios acederem, seletivamente, às suas aplicações empresariais
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/08/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 5ee55034e84fe09484a2f7613cc2224be70fdebb
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="self-service-portal-for-azure-ad-b2b-collaboration-sign-up"></a>Portal self-service para inscrição de colaboração B2B do Azure AD

Os clientes podem fazer muito com as funcionalidades incorporadas que são expostas através de [portal do Azure](https://portal.azure.com) e o [painel de acesso de aplicação](https://myapps.microsoft.com) para os utilizadores finais. No entanto, poderá ter de personalizar o fluxo de trabalho de integração para os utilizadores de B2B para se ajustarem às necessidades da sua organização. Pode fazê-lo com [o convite API](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation).

Como uma organização convidando, poderá não saber antecedência quem são os colaboradores externos individuais que necessitam de aceder aos seus recursos. Precisa de uma forma para os utilizadores de empresas, para se inscrever-se com um conjunto de políticas que controla, como a organização convidando. Este cenário é possível através de APIs. Não existe um [projeto de exemplo no GitHub](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web) que mesmo.

Este projeto de GitHub mostra como as organizações podem utilizar as APIs para fornecer uma capacidade de inscrição baseadas em políticas, self-service para os parceiros fidedignos, com regras que determinam as aplicações que podem aceder. Os utilizadores do parceiro aceder aos recursos quando precisam. Pode tal forma segura, sem exigindo que a organização convidando para carregar manualmente. Pode facilmente implementar o projeto para uma subscrição do Azure à sua escolha.

## <a name="as-is-code"></a>Como-é código

Este código é disponibilizado como um exemplo para demonstrar a utilização do convite do Azure Active Directory B2B API. Deve ser personalizada pela sua equipa de desenvolvimento ou de um parceiro e deve ser revisto antes de implementar um cenário de produção.

## <a name="next-steps"></a>Passos Seguintes

* [O que é a colaboração B2B do Azure AD?](what-is-b2b.md)
* [Licenciamento e colaboração do Azure AD B2B](licensing-guidance.md)
* [Colaboração do Azure Active Directory B2B perguntas mais frequentes (FAQ)](faq.md)