---
title: "Gerir as contas administrativas de acesso de emergência no Azure AD | Microsoft Docs"
description: "Este artigo descreve como utilizar contas de acesso de emergência para ajudar as organizações a restringir o acesso privilegiado num ambiente existente do Azure Active Directory."
services: active-directory
keywords: "Não adicionar ou editar as palavras-chave sem consultadoria seu perito de SEO."
author: markwahl-msft
ms.author: billmath
ms.date: 12/13/2017
ms.topic: article-type-from-white-list
ms.service: active-directory
ms.workload: identity
ms.custom: it-pro
ms.reviewer: markwahl-msft
ms.openlocfilehash: 1545fb9a89794a74efbb855c4480040973c3308e
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="manage-emergency-access-administrative-accounts-in-azure-ad"></a>Gerir as contas administrativas de acesso de emergência no Azure AD 

Para a maioria das atividades diárias, *administrador global* não são necessários direitos pelos seus utilizadores. Os utilizadores não devem ser permanentemente atribuídos à função, porque estes inadvertidamente poderão efetuar uma tarefa que necessita de permissões superiores que devem ter. Quando os utilizadores não precisam de atuar como um administrador global, que devem ativam a atribuição de função utilizando o Azure Active Directory (Azure AD) Privileged Identity Management (PIM), na sua própria conta ou uma conta de administrador alternativa.

Para além dos utilizadores demorar nos direitos de acesso administrativo para si próprios, tem de impedir que está a ser inadvertidamente bloqueado a administração de inquilino do Azure AD porque pode não iniciar sessão nem ativar uma existente individuais da conta de utilizador como um administrador. Pode mitigar o impacto de inadvertida falta de acesso administrativo ao armazenar dois ou mais *contas de acesso de emergência* no seu inquilino.

Contas de acesso de emergência podem ajudar as organizações a restringir o acesso privilegiado num ambiente existente do Azure Active Directory. Estas contas são altamente privilegiadas, e não forem atribuídos a indivíduos específicos. Contas de acesso de emergência estão limitadas à emergência ou 'break glass' cenários, situações onde não não possível utilizar as contas administrativas normais. As organizações têm de manter um objetivo de restringir a utilização de emergência da conta para apenas esse tempo durante os quais é necessário.

Uma organização poderá ter de utilizar uma conta de acesso de emergência nas seguintes situações:

 - As contas de utilizador são federadas e a Federação está indisponível devido a uma quebra de rede de célula ou uma falha do fornecedor de identidade. Por exemplo, se o anfitrião do fornecedor de identidade no seu ambiente tornou-se para baixo, os utilizadores poderão não conseguir iniciar sessão quando o Azure AD redireciona para o seu fornecedor de identidade. 
 - Os administradores são registados através do Azure multi-factor Authentication e os respetivos dispositivos individuais não se encontram disponíveis. Os utilizadores poderão não consegue concluir o multi-factor Authentication para ativar uma função. Por exemplo, uma falha de rede da célula está a impedi-los de atender as chamadas telefónicas ou receber mensagens de texto, as mecanismos de autenticação apenas dois registados para o respetivo dispositivo. 
 - A pessoa com acesso administrativo global mais recente ter saído da organização. Azure AD impede que o último *administrador global* conta que está a ser eliminado, mas não impede a conta que está a ser eliminado ou desativada no local. A situação pode tornar a organização não é possível recuperar a conta.

## <a name="initial-configuration"></a>Configuração inicial

Crie dois ou mais contas de acesso de emergência. Devem ser contas apenas na nuvem que utilizam o \*. domínio onmicrosoft.com e que não são federado ou sincronizados a partir de um ambiente no local. 

As contas não devem estar associadas a qualquer utilizador individuais na organização. As organizações precisam de garantir que as credenciais para estas contas são mantidas segura e conhecidos apenas a indivíduos que têm autorização para utilizá-los. 

> [!NOTE]
> Uma palavra-passe de conta para uma conta de acesso de emergência, normalmente, é separada em partes de duas ou três, escrita em separado peças de documento e armazenada na seguras, fireproof cofres que se encontram em localizações seguras, separadas. 
>
> Certifique-se de que as contas de acesso de emergência não estão ligadas com qualquer telemóveis fornecido o empregado, tokens de hardware que levar com funcionários individuais ou outras credenciais específicas do empregado. Esta precaução abrange situações em que um empregado individuais está inacessível quando a credencial é necessária. 

### <a name="initial-configuration-with-permanent-assignments"></a>Configuração inicial com atribuições permanentes

É uma opção para que os utilizadores membros permanentes do *administrador global* função. Esta opção seria adequada para organizações que não tem subscrições do Azure AD Premium P2.

Para reduzir o risco de um ataque resultante de uma palavra-passe comprometida, o Azure AD recomenda que necessitam de multi-factor Authentication para todos os utilizadores individuais. Deve incluir este grupo de administradores e todas as outras (por exemplo, financeiros officers) cuja conta comprometida teria um impacto significativo. 

No entanto, se a sua organização não tiver dispositivos partilhados, multi-factor Authentication pode não ser possível para estas contas de acesso de emergência. Se estiver a configurar uma política de acesso condicional para exigir [registo de multi-factor Authentication para cada administrador](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states) para o Azure AD e outro ligado software como um serviço (SaaS) de aplicações, poderá ter de configurar a política exclusões para excluir contas de acesso de emergência este requisito.

### <a name="initial-configuration-with-approvals"></a>Configuração inicial com aprovações

Outra opção consiste em configurar os seus utilizadores como elegíveis e aprovadores para ativar o *administrador global* função. Esta opção seria necessários à sua organização tem subscrições do Azure AD Premium P2. É também iria exigir uma opção de multi-factor Authentication que é adequada para utilização partilhada entre vários trabalhadores e o ambiente de rede. Estes requisitos são porque a ativação do *administrador global* função requer que os utilizadores efetuou anteriormente multi-factor Authentication. Para obter mais informações, consulte [como requerer a autenticação Multifator no Azure AD Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-how-to-require-mfa).

Não é recomendada a utilizar o multi-factor Authentication que está associada a dispositivos pessoais para contas de acesso de emergência. Em emergência real, a pessoa que precisa de aceder a um dispositivo registado o multi-factor Authentication não poderá ser um que tenha o dispositivo pessoal. 

Considere também o cenário das ameaças. Por exemplo, uma circunstância imprevista como catástrofe natural emergência pode ocorrer durante o qual um telemóvel ou outras redes poderão estar indisponíveis. É importante certificar-se de que todos os dispositivos registados são mantidos numa localização segura, conhecida que tenha vários meios de comunicação com o Azure AD.

## <a name="ongoing-monitoring"></a>A monitorização em curso

Monitor de [do Azure AD início de sessão e auditoria nos registos](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins) para quaisquer inícios de sessão e auditar a atividade das contas de acesso de emergência. Normalmente, essas contas não devem estar a iniciar sessão e não devem ser efetuar alterações, pelo que a utilização dos mesmos é provável que sejam anómala e exigir investigação de segurança.

## <a name="account-check-validation-must-occur-at-regular-intervals"></a>Validação de verificação de conta tem de ocorrer em intervalos regulares

Para validar a conta, execute os seguintes passos no mínimo:
- Todos os 90 dias.
- Quando tiver ocorrido alguma alteração recente na equipa de TI, como uma alteração de tarefa, um distanciamento ou uma novo contratação.
- Quando foram alteradas as subscrições do Azure AD na organização.

Para preparar os membros da equipa para utilizar contas de acesso de emergência, efetue o seguinte:

* Certifique-se de que os empregados da monitorização de segurança estão cientes de que a atividade de verificação de conta está em curso.
* Confirme que as contas de utilizador de nuvem podem iniciar sessão e ativar as respetivas funções e que os utilizadores que poderão ter de efetuar estes passos durante emergência são preparados do processo.
* Certifique-se de não registado multi-factor Authentication ou a reposição de palavra-passe self-service (SSPR) ao dispositivo ou pessoais detalhes de qualquer utilizador individuais. 
* Se as contas estão registadas para o multi-factor Authentication num dispositivo, para utilização durante a ativação de função, certifique-se de que o dispositivo está acessível para todos os administradores que poderão ter de utilizá-lo durante emergência. Certifique-se também de que o dispositivo é registado através de, pelo menos, dois mecanismos que não partilham um modo comuns de falha. Por exemplo, o dispositivo pode comunicar à internet através da rede sem fios de uma instalação e uma rede de fornecedor de célula.
* Atualize as credenciais da conta.

## <a name="next-steps"></a>Passos Seguintes
- [Adicionar um utilizador baseado na nuvem](add-users-azure-active-directory.md) e [atribuir o novo utilizador à função de administrador global](active-directory-users-assign-role-azure-portal.md).
- [Inscrever-se no Azure Active Directory Premium](active-directory-get-started-premium.md), se se ainda não inscreveu já.
- [Requer multi-factor Authentication do Azure para utilizadores individuais atribuídos como administradores](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states).
- [Configurar proteções adicionais para os administradores globais no Office 365](https://support.office.com/article/Protect-your-Office-365-global-administrator-accounts-6b4ded77-ac8d-42ed-8606-c014fd947560), se estiver a utilizar o Office 365.
- [Executar uma revisão do acesso de administradores globais](active-directory-privileged-identity-management-how-to-start-security-review.md) e [transição administradores globais existentes para funções de administrador mais específicas](active-directory-assign-admin-roles-azure-portal.md).


