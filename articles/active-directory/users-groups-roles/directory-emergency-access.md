---
title: Gerir o acesso de emergência de contas administrativas no Azure AD | Documentos da Microsoft
description: Este artigo descreve como utilizar contas de acesso de emergência para ajudar as organizações a restringir o acesso privilegiado num ambiente do Azure Active Directory existente.
services: active-directory
author: markwahl-msft
ms.author: billmath
ms.date: 12/13/2017
ms.topic: article-type-from-white-list
ms.service: active-directory
ms.workload: identity
ms.custom: it-pro
ms.reviewer: markwahl-msft
ms.openlocfilehash: 4f3772abc1cdbd3b35b8b1f16e7a47c0f1a17783
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38595659"
---
# <a name="manage-emergency-access-administrative-accounts-in-azure-ad"></a>Gerir o acesso de emergência de contas administrativas no Azure AD 

Na maioria das atividades diárias, *administrador global* direitos não são necessários para os seus utilizadores. Os utilizadores não devem ser permanentemente atribuídos à função, porque eles podem inadvertidamente a executar uma tarefa que exige mais permissões do que eles devem ter. Quando os usuários não precisam atuar como um administrador global, que os utilizadores devem ativar a atribuição de função com o Azure Active Directory (Azure AD) Privileged Identity Management (PIM), em sua própria conta ou uma conta administrativa alternativa.

Além dos utilizadores a demorar sobre direitos de acesso administrativo por conta própria, precisa impedir que está a ser inadvertidamente impedido de aceder a administração de inquilino do Azure AD porque não podem iniciar sessão nem ativar a conta de um usuário individual existente como um administrador. Pode mitigar o impacto de inadvertida falta de acesso administrativo ao armazenar duas ou mais *contas de acesso de emergência* no seu inquilino.

Contas de acesso de emergência podem ajudar as organizações a restringir o acesso privilegiado num ambiente do Azure Active Directory existente. Essas contas são altamente privilegiadas e não são atribuídas a indivíduos específicos. Contas de acesso de emergência estão limitadas a emergência ou cenários "break glass", situações onde não não possível utilizar contas administrativas normais. As organizações têm de manter uma meta de restringir a utilização da conta de emergência para apenas esse tempo durante o qual é necessário.

Uma organização poderá ter de utilizar uma conta de acesso de emergência nas seguintes situações:

 - As contas de utilizador forem federadas e Federação está atualmente indisponível devido a uma quebra de rede de célula ou uma falha do fornecedor de identidade. Por exemplo, se o anfitrião do fornecedor de identidade no seu ambiente tornou-se para baixo, os utilizadores poderão não é possível iniciar sessão quando do Azure AD redireciona para o seu fornecedor de identidade. 
 - Os administradores estão registados através do multi-factor Authentication, e todos os seus dispositivos individuais estão disponíveis. Os utilizadores poderão não é possível concluir a multi-factor Authentication para ativar uma função. Por exemplo, uma indisponibilidade de rede de célula está a impedi-los de responder a chamadas telefónicas ou receber mensagens de texto, os mecanismos de autenticação de dois únicos que eles registado para o respetivo dispositivo. 
 - A pessoa com o acesso administrativo global mais recente tiver saído da organização. O Azure AD impede que o último *administrador global* conta seja eliminado, mas ele não impede que a conta seja eliminado ou desativado no local. De qualquer situação poderá fazer a organização não é possível recuperar a conta.

## <a name="initial-configuration"></a>Configuração inicial

Crie duas ou mais contas de acesso de emergência. Devem ser contas apenas na cloud que utilizam o \*. domínio onmicrosoft.com e que não são federados ou sincronizados a partir de um ambiente no local. 

As contas não devem ser associadas a qualquer utilizador individual na organização. As organizações precisam garantir que as credenciais para estas contas são mantidas seguras e conhecidas somente a indivíduos que têm autorização para utilizá-los. 

> [!NOTE]
> Uma palavra-passe de conta para uma conta de acesso de emergência, normalmente, é separada em duas ou três partes, escrita em partes separadas de papel e armazenada em cofres seguras, incêndios, que se encontram em localizações seguras, em separado. 
>
> Certifique-se de que as contas de acesso de emergência não estão ligadas com qualquer celulares fornecido pelo funcionário, tokens de hardware que viajam com funcionários individuais ou outras credenciais de funcionário específico. Esta precaução abrange instâncias em que um funcionário é inacessível quando a credencial é necessária. 

### <a name="initial-configuration-with-permanent-assignments"></a>Configuração inicial com atribuições permanentes

Uma opção é que usuários membros permanentes dos *administrador global* função. Esta opção poderá ser adequada para as organizações que não tem subscrições do Azure AD Premium P2.

Para reduzir o risco de um ataque resultantes de uma palavra-passe comprometida, do Azure AD recomenda que exigem o multi-factor Authentication para todos os utilizadores individuais. Este grupo deve incluir os administradores e todas as demais (por exemplo, responsáveis financeiros) cuja conta comprometida seria ter um impacto significativo. 

No entanto, se a sua organização não tiver dispositivos partilhados, multi-factor Authentication pode não ser possível para estas contas de acesso de emergência. Se estiver a configurar uma política de acesso condicional para exigir [registo de multi-factor Authentication para todos os administradores](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states) para o Azure AD e outro ligado software como aplicações de serviço (SaaS), poderá ter de configurar a política exclusões para excluir contas de acesso de emergência deste requisito.

### <a name="initial-configuration-with-approvals"></a>Configuração inicial com aprovações

Outra opção consiste em configurar os seus utilizadores como elegíveis e aprovadores para ativar a *administrador global* função. Esta opção requer que sua organização ter subscrições do Azure AD Premium P2. Ele também exigiria uma opção de multi-factor Authentication que é adequada para utilização partilhada entre vários indivíduos e o ambiente de rede. Estes requisitos são porque a ativação dos *administrador global* a função requer que os utilizadores anteriormente tiver executado o multi-factor Authentication. Para obter mais informações, consulte [como requerer o multi-factor Authentication no Azure AD Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-how-to-require-mfa).

Recomendamos que não utilize o multi-factor Authentication que está associada a dispositivos pessoais para contas de acesso de emergência. Numa emergência real, a pessoa que precisa de aceder a um dispositivo registado de multi-factor Authentication pode não ser que tenha o dispositivo pessoal. 

Além disso, considere o cenário de ameaças. Por exemplo, uma circunstância imprevista, como um desastre natural emergência pode ocorrer durante o qual um telefone celular ou a outras redes podem não estar disponíveis. É importante certificar-se de que todos os dispositivos registados são mantidos num local conhecido e seguro que tenha vários meios de comunicação com o Azure AD.

## <a name="ongoing-monitoring"></a>Monitorização contínua

Monitor de [registos de início de sessão no Azure AD e auditoria](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins) para quaisquer inícios de sessão e auditar a atividade a partir de contas de acesso de emergência. Normalmente, essas contas não devem estar a iniciar sessão e não devem estar fazendo alterações, então, usá-los é provável que sejam anómalas e exigir uma investigação de segurança.

## <a name="account-check-validation-must-occur-at-regular-intervals"></a>Validação de verificação de conta tem de ocorrer em intervalos regulares

Para validar a conta, execute os seguintes passos no mínimo:
- Todos os 90 dias.
- Quando tiver ocorrido uma alteração recente na equipe de TI, como uma alteração de tarefa, uma partidas ou uma nova contratação.
- Quando as subscrições do Azure AD na organização tem sido alterado.

Para treinar os membros da Equipe para utilizar as contas de acesso de emergência, faça o seguinte:

* Certifique-se de que a equipe de monitorização de segurança estejam cientes de que a atividade de verificação de conta está em curso.
* Confirme que as contas de utilizador da cloud podem iniciar sessão e ativar as suas funções e que os utilizadores que poderão ter de executar estes passos durante uma emergência estão treinados sobre o processo.
* Certifique-se de que eles não se registou multi-factor Authentication ou a reposição de palavra-passe self-service (SSPR) para o dispositivo ou detalhes pessoais de qualquer utilizador individual. 
* Se as contas registadas para o multi-factor Authentication para um dispositivo, para utilização durante a ativação de função, certifique-se de que o dispositivo não está acessível para todos os administradores que podem precisar usá-lo durante uma emergência. Certifique-se também de que o dispositivo está registado por meio, pelo menos, dois mecanismos que não partilham um modo comum de falha. Por exemplo, o dispositivo pode comunicar com a internet através da rede sem fio de um recurso e uma rede de fornecedor de célula.
* Atualize as credenciais de conta.

## <a name="next-steps"></a>Passos Seguintes
- [Adicionar um utilizador com base na cloud](../fundamentals/add-users-azure-active-directory.md) e [atribuir o novo utilizador à função de administrador global](../fundamentals/active-directory-users-assign-role-azure-portal.md).
- [Inscrever-se no Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md), se ainda não tiver já.
- [Exigir multi-factor Authentication para utilizadores individuais atribuídos como administradores](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states).
- [Configurar proteções adicionais para os administradores globais do Office 365](https://support.office.com/article/Protect-your-Office-365-global-administrator-accounts-6b4ded77-ac8d-42ed-8606-c014fd947560), se estiver a utilizar o Office 365.
- [Realizar uma revisão de acesso de administradores globais](../privileged-identity-management/pim-how-to-start-security-review.md) e [transição os administradores globais existentes para funções de administrador mais específicas](directory-assign-admin-roles.md).


