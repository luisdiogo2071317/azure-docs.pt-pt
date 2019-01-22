---
title: Orientação de licenciamento a colaboração B2B do Active Directory do Azure | Documentos da Microsoft
description: O Azure Active Directory B2B não necessita de colaboração pago licenças do Azure AD, mas pode também obter funcionalidades pagas para utilizadores convidados B2B
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 10/04/2018
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: mal
ms.openlocfilehash: d89fb86209af2074794d5a5abcdcebadec904799
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2019
ms.locfileid: "54434279"
---
# <a name="azure-active-directory-b2b-collaboration-licensing-guidance"></a>Orientação de licenciamento de colaboração do Azure Active Directory B2B

Com a colaboração de empresa-empresa (B2B) do Azure Active Directory (Azure AD), pode convidar utilizadores externos (ou "utilizadores") para utilizar seu paga serviços do Azure AD. Para cada pagas licença do Azure AD que são atribuídas a um utilizador, pode convidar até cinco utilizadores convidados sob a permissão de utilizador externo.

Um utilizador convidado é alguém que não é um membro da sua organização ou qualquer uma das afiliadas da sua organização. Os utilizadores convidados são definidos pela sua relação com a sua organização, não pelas credenciais que utilizam para iniciar sessão. Na verdade, um utilizador convidado pode iniciar sessão com uma identidade de externa ou com as credenciais pertencentes à sua organização.

Seguem-se *não* utilizadores convidados:
- Seus funcionários, prestadores de serviços no local ou agentes locais
- Os funcionários, prestadores de serviços no local ou agentes locais das suas empresas afiliadas

Licenciamento de utilizadores de convidados B2B é automaticamente calculada e reportada com base no rácio de 1:5. Atualmente, não é possível atribuir B2B licenças de utilizador convidado diretamente para os utilizadores convidados.

Existem algumas situações em que um utilizador convidado não é comunicado com a permissão de utilizador externo de 1:5. Se um utilizador convidado já tem um paga licença do Azure AD na organização do utilizador, o utilizador não consome uma das suas licenças de utilizador de convidados B2B. Além disso, o convidado, os utilizadores podem utilizar gratuito funcionalidades do Azure AD sem necessidade de licenciamento adicionais. Os utilizadores convidados têm acesso gratuito a funcionalidades do Azure AD, mesmo se não tiver nenhum pago licenças do Azure AD. 

## <a name="examples-calculating-guest-user-licenses"></a>Exemplos: Cálculo de licenças de utilizador convidado
Depois de determinar o número de utilizadores convidados precisa acessar seu paga serviços do Azure AD, verifique se tem suficiente paga do Azure AD licenças para abranger os utilizadores convidados na proporção de 1:5 necessária. Eis alguns exemplos:

- Pretende convidar 100 utilizadores para as suas aplicações do Azure AD ou serviços, e que pretende atribuir a gestão de acesso e aprovisionamento para todos os utilizadores convidados. Também queira exigir a MFA e o acesso condicional para 50 desses usuários de convidado. Para cobrir essa combinação, precisará 10 licenças do Azure AD básico e 10 licenças do Azure AD Premium P1. Se planeia utilizar funcionalidades de proteção de identidade com os seus utilizadores convidados, terá de licenças do Azure AD Premium P2 na mesma proporção de 1:5 para abranger todos os utilizadores convidados.
- Deseja convidar 60 utilizadores que exigem a MFA, pelo que tem de ter, pelo menos, 12 licenças do Azure AD Premium P1. Tem 10 funcionários com licenças do Azure AD Premium P1, que permitiria que o máximo de 50 utilizadores convidados em 1:5 rácio de licenciamento. Terá de comprar licenças de Premium P1 adicionais duas para cobrir a 10 utilizadores adicionais.

## <a name="using-the-b2b-collaboration-api-to-invite-users-from-your-affiliates"></a>Usando a API de colaboração do B2B para convidar os utilizadores de suas afiliadas

Por definição, um utilizador de convidados B2B é um utilizador externo convidar para utilizar seu pagos do Azure AD aplicações e serviços. Dos funcionários, prestadores de serviços no local ou agente no local de sua empresa ou uma das suas empresas afiliadas não se qualificam para colaboração do B2B, mesmo se B2B funcionalidades são utilizadas. Eis alguns exemplos: 
- Que pretende utilizar credenciais externas (por exemplo, uma identidade de redes sociais) para convidar um utilizador que seja um funcionário da sua organização. Este cenário não é compatível com os requisitos de licenciamento e não é permitido. Credenciais externas não Cometa um funcionário um utilizador externo.  
- Que pretende utilizar APIs de B2B para convidar um utilizador a partir de uma das afiliadas da sua organização. Embora este cenário utiliza as APIs de B2B para convidar o utilizador, não é considerada a colaboração B2B. Ele não está em conformidade com os requisitos de licenciamento porque um utilizador da sua afiliada não for um utilizador externo. 

Em ambos os cenários, a melhor solução é usar a API de B2B convidar utilizadores como membros (invitedUserType = membro) e atribuir-lhes cada uma licença do Azure AD. 

## <a name="next-steps"></a>Passos Seguintes

Consulte os seguintes recursos sobre a colaboração B2B do Azure AD:

* [O que é a colaboração B2B do Azure AD?](what-is-b2b.md)
* [Perguntas mais frequentes (FAQ) sobre a colaboração B2B do Azure Active Directory](faq.md)
