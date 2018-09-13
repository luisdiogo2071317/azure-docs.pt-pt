---
title: Portal de inscrição self-service para colaboração do Azure Active Directory B2B | Documentos da Microsoft
description: A colaboração do B2B Azure Active Directory suporta as relações entre empresas, permitindo a parceiros de negócios acederem, seletivamente, às suas aplicações empresariais
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/08/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 94001b005a883c172cab279029b47ac1ad0c0de5
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35647296"
---
# <a name="self-service-portal-for-azure-ad-b2b-collaboration-sign-up"></a>Portal self-service para inscrição de colaboração B2B do Azure AD

Os clientes podem fazer muito com os recursos incorporados que são expostos por meio da [portal do Azure](https://portal.azure.com) e o [painel de acesso de aplicação](https://myapps.microsoft.com) para os utilizadores finais. No entanto, poderá ter de personalizar o fluxo de trabalho de integração para utilizadores B2B para se ajustar às necessidades da sua organização. Pode fazer isso com [o convite API](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation).

Como uma organização que o convidou, talvez não saiba antes do tempo que os funcionários externos individuais são que necessitam de aceder aos seus recursos. Precisa de uma forma para os utilizadores de empresas associadas, para se inscrever-se com um conjunto de políticas que controla a, como a organização que o convidou. Este cenário é possível através das APIs. Há uma [projeto de exemplo no GitHub](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web) que faz exatamente isso.

Este projeto GitHub mostra como as organizações podem utilizar as APIs para fornecer uma capacidade de inscrição baseada em políticas de autoatendimento para seus parceiros de confiança, com regras que determinam as aplicações podem aceder. Utilizadores parceiros podem obter acesso aos recursos quando são necessários. Eles podem fazer isso de forma segura, sem que necessitem da organização que o convidou para carregar manualmente. Pode facilmente implementar o projeto para uma subscrição do Azure à sua escolha.

## <a name="as-is-code"></a>Como-é o código

Esse código é disponibilizado como um exemplo para demonstrar a utilização do Azure Active Directory B2B convite API. Ele deve ser personalizado por sua equipe de desenvolvimento ou um parceiro e deve ser revisto antes de implementá-la num cenário de produção.

## <a name="next-steps"></a>Passos Seguintes

* [O que é a colaboração B2B do Azure AD?](what-is-b2b.md)
* [Licenciamento de colaboração B2B do AD do Azure](licensing-guidance.md)
* [A colaboração do Azure Active Directory B2B perguntas mais frequentes (FAQ)](faq.md)