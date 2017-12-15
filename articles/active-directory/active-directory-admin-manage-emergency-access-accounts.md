---
title: "Gerir contas de acesso de emergência a adminstrative no Azure AD | Microsoft Docs"
description: "Descreve como utilizar contas de acesso de emergência para ajudar as organizações a restringir o acesso privilegiado num ambiente existente do Azure Active Directory."
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
ms.openlocfilehash: 75ea8e445c890e7af5ab14f4fc4c53cfb1af4568
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2017
---
# <a name="managing-emergency-access-administrative-accounts-in-azure-ad"></a>Gerir as contas administrativas de acesso de emergência no Azure AD 

Para a maioria das atividades diárias, não são necessários direitos de função administrativa global.  Os utilizadores não devem ser permanentemente atribuídos à função, como o utilizador inadvertidamente pode executar uma tarefa com permissões superiores que o necessário. Quando um utilizador necessário agir como um administrador global, que devem ativam a atribuição de função utilizando o Azure AD PIM em sua própria conta ou uma conta de administrador alternativa.

Para além dos utilizadores a colocar nos direitos de acesso administrativo para si próprios, que os clientes precisam para se preparar para garantir que não recebem para uma situação em que foi ser inadvertidamente bloqueados administração do seu inquilino do Azure AD devido a sua incapacidade de iniciar sessão na ou ativar um existente individuais da conta de utilizador como administrador.  Estes podem mitigar o impacto de acidental falta de acesso administrativo através do armazenamento no respetivo inquilino dois ou mais *contas de acesso de emergência*.

Contas de acesso de emergência ajudam as organizações a restringir o acesso privilegiado num ambiente existente do Azure Active Directory. Estas contas são altamente privilegiadas e não estão atribuídas a indivíduos específicos. Contas de acesso de emergência estão limitadas a emergência para 'break glass' cenários onde não não possível utilizar as contas administrativas normais.  As organizações tem de garantir o objetivo de controlar e reduzindo a utilização de emergência da conta para apenas esse tempo para o qual é necessário.

Cenários em que uma organização poderá ter de efetuar a utilização de uma conta de acesso de emergência:

 - As contas de utilizador são federadas e a Federação está indisponível devido a uma quebra de rede ou uma falha do fornecedor de identidade.  Por exemplo, se o anfitrião do fornecedor de identidade no ambiente do cliente tornou-se para baixo, em seguida, um utilizador poderá não conseguir iniciar sessão quando o Azure AD redireciona para o seu fornecedor de identidade. 
 - Os administradores são registados através de MFA e todos os respetivos dispositivos individuais não estão disponíveis.  É possível que os utilizadores conseguem não concluída multi-factor authentication ativar uma função, se para a instância uma falha de rede de célula pode impedir os utilizadores a capacidade de resposta de chamadas telefónicas ou receber mensagens de texto e que foi o mecanismo de apenas essa que registado para o respetivo dispositivo. 
 - A última pessoa que tinha acesso administrativo global saído da organização.  Azure AD impede que a última conta de administrador global que está a ser eliminada, mas não impede a conta que está a ser eliminado ou desativada no local, originando uma situação em que a organização não é possível recuperar essa conta.

## <a name="initial-configuration"></a>Configuração inicial

Crie dois ou mais contas de acesso de emergência.  Devem ser contas apenas na nuvem utilizando o *. c o m domínio, não federada ou sincronizados a partir de um ambiente no local.  

Não deve estar associados a qualquer utilizador individuais na organização.  As organizações precisam de Certifique-se de que as credenciais para estas contas são mantidas segura e conhecidos apenas a indivíduos que têm autorização para utilizá-los. 

Nota: Normalmente, a palavra-passe de conta para uma conta de acesso de emergência é separada em partes de duas ou três e, em seguida, escrita em separado partes do documento e armazenada no seguras, fireproof cofres que se encontram em localizações separadas seguras. Certifique-se de que as contas de acesso de emergência não estão ligadas com qualquer telemóveis fornecido o empregado, tokens de hardware que levar com funcionários individuais ou outras credenciais específicas do empregado, caso esse empregado individuais está inacessível no momento a credencial é necessária. 

### <a name="initial-configuration-with-permanent-assignments"></a>Configuração inicial com atribuições permanentes

É uma opção de atribuir esses utilizadores como membros permanentes da função de Administrador Global.  Isto seria adequado para as organizações que não tem subscrições do Azure AD Premium P2.

Azure AD recomenda que necessita de autenticação multifator (MFA) para todos os utilizadores individuais, incluindo administradores e todos os outros utilizadores que teriam um impacto significativo se a sua conta foi comprometida (por exemplo, financeiros officers). Isto reduz o risco de um ataque devido a uma palavra-passe comprometido. No entanto, se a sua organização não tiver dispositivos partilhados, em seguida, MFA poderá não ser possível para estas contas de acesso de emergência.  Se estiver a configurar uma política de acesso condicional para exigir [registo do MFA para cada administrador](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states) para o Azure AD e outro ligado aplicações SaaS, poderá ter de configurar exclusões de política para excluir este contas de acesso de emergência requisito.

### <a name="initial-configuration-with-approvals"></a>Configuração inicial com aprovações

Outra opção consiste em configurar esses utilizadores como elegível e aprovadores para ativar a função de Administrador Global.  Isto precisaria que a organização tenha subscrições do Azure AD Premium P2, bem como uma opção de MFA adequada para utilização partilhada entre vários trabalhadores e o ambiente de rede.  Isto acontece porque o utilizador efetuou anteriormente MFA necessita de ativação da função de Administrador Global.  Saiba mais em [como requerer a MFA no Azure AD Privileged Identity Management](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-privileged-identity-management-how-to-require-mfa).

Utilizar o MFA associada com dispositivos pessoais não são recomendadas para contas de acesso de emergência, porque numa real emergência uma pessoa que tem de ter acesso a um dispositivo registado para MFA poderão não ser aquele que é que contém o dispositivo pessoal.  Considere também a ameaças, se devido a circunstâncias imprevistas telemóvel ou outras redes estão indisponíveis durante um desastre natural emergência.  Por conseguinte, é importante certificar-se de que todos os dispositivos registados são mantidos numa localização segura, conhecida que tenha vários meios de comunicação com o Azure AD.

## <a name="ongoing-monitoring"></a>A monitorização em curso

Monitor de [do Azure AD a iniciar sessão e registos de auditoria](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-activity-sign-ins) para quaisquer inícios de sessão e auditar a atividade das contas de acesso de emergência.  Normalmente, essas contas não devem estar a iniciar sessão e não devem ser efetuar alterações, pelo que a utilização dos mesmos é provável que sejam anómala e exigir investigação de segurança.

## <a name="account-check-validation-must-occur-at-regular-intervals"></a>Validação da verificação de conta tem de ocorrer em intervalos regulares

Execute, pelo menos, os seguintes passos:
 - Todos os 90 dias
 - Quando tiver ocorrido alguma alteração recente na equipa de TI – uma tarefa alterar, distanciamento ou contratação novo
 - Quando as subscrições do Azure AD da organização foram alterados

Para garantir que os funcionários são preparados em como utilizar as contas de acesso de emergência:

1.  Certifique-se de que os funcionários de monitorização de segurança estão cientes que a atividade de verificação da conta está em curso.
2.  Confirme que as contas de utilizador de nuvem podem iniciar sessão e ativar as respetivas funções e que os utilizadores que poderão ter de executar estes passos durante emergência são preparados do processo.
3.  Certifique-se de que estes não foram registados para MFA ou SSPR ao dispositivo ou pessoais detalhes de qualquer utilizador individuais.  
4. Se as contas estão registadas para a MFA para um dispositivo, para utilização durante a ativação de função, certifique-se de que o dispositivo se encontra acessível para todos os administradores que poderão ter de utilizá-lo durante emergência.  Verifique também se o dispositivo está registado através de, pelo menos, dois mecanismos que não partilham um modo comuns de falha (por exemplo, o dispositivo conseguiu estabelecer ligação à Internet, quer através da rede sem fios de uma instalação, bem como através de uma rede de fornecedor de célula).
5.  Atualize as credenciais.

## <a name="next-steps"></a>Passos seguintes
- [Adicionar um utilizador baseado na nuvem](add-users-azure-active-directory.md) e [atribuir o novo utilizador à função de administrador global](active-directory-users-assign-role-azure-portal.md)
- [Inscrever-se no Azure Active Directory Premium](active-directory-get-started-premium.md), se ainda não o fez
- [Exigir a MFA do Azure para utilizadores individuais atribuídos como administradores](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states)
- [Configurar proteções adicionais para os administradores globais no Office 365](https://support.office.com/article/Protect-your-Office-365-global-administrator-accounts-6b4ded77-ac8d-42ed-8606-c014fd947560), se estiver a utilizar o Office 365
- [Executar uma revisão do acesso de administradores globais](active-directory-privileged-identity-management-how-to-start-security-review.md) e [transição administradores globais existentes para funções de administrador mais específicas](active-directory-assign-admin-roles-azure-portal.md)

