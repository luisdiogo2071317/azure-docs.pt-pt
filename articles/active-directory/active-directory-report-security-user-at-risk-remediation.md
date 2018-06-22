---
title: Utilizadores sinalizados no relatório de segurança de risco no portal do Azure Active Directory | Microsoft Docs
description: Saiba mais sobre os utilizadores sinalizados no relatório de segurança de risco no portal do Azure Active Directory
services: active-directory
author: priyamohanram
manager: mtillman
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 05/23/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 60a77a8bee54c0582cbc9c88b9d517820a2760e0
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36221985"
---
# <a name="remediate-users-flagged-for-risk-in-the-azure-active-directory-portal"></a>Remediar utilizadores sinalizados para risco no portal do Azure Active Directory

Com os relatórios de segurança no Azure Active Directory (Azure AD), pode obter informações sobre a probabilidade de contas de utilizador comprometidas no seu ambiente. Os [utilizadores sinalizados para risco](active-directory-identityprotection.md#users-flagged-for-risk) são um indicador de uma conta de utilizador que pode ter sido comprometida.

A Microsoft está empenhada em ajudar a manter os seus ambientes seguros. Como parte desse compromisso, a Microsoft monitoriza continuamente atividades que são invulgares ou que são consistentes com padrões de ataques conhecidos. 


Se foram detetadas atividades invulgares que possam indicar acesso não autorizado a algumas das suas contas de utilizador, receberá notificações que lhe permitem tomar medidas. A disponibilização das notificações não significa que os sistemas da Microsoft tenham sido comprometidos de alguma forma.
 

## <a name="access-the-users-flagged-for-risk-report"></a>Aceda ao relatório de utilizadores sinalizados para risco

Pode rever os utilizadores sinalizados para risco através do [relatório](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/UsersAtRisk) relacionado no Azure Active Directory (AD). Se não for subscritor do Azure AD, pode seguir o processo de subscrição único sem custos, em [https://aka.ms/AccessAAD](https://aka.ms/AccessAAD). Neste relatório, pode fazer diversas ações, como:

- Gerar uma palavra-passe temporária
- Exigir que o utilizador reponha a palavra-passe dele em segurança na próxima vez que iniciar sessão
- Dispense o risco de utilizador sem tomar qualquer ação de remediação.

Para obter informações, veja [Relatório de segurança de Utilizadores sinalizados para risco no portal do Azure Active Directory](active-directory-reporting-security-user-at-risk.md).

### <a name="azure-ad-subscription-for-office-365-customers"></a>Subscrição do Azure AD para os clientes do Office 365

Depois de concluído, pode utilizar as suas credenciais do Office 365 para aceder ao Centro de Administração do Azure. Depois de ativar o acesso ao Azure AD, é redirecionado para o portal do Azure AD. Ao nível da subscrição básica, a quantidade de detalhes indicados nos relatórios é limitada. Estão disponíveis mais dados e análises para os subscritores Premium do Azure.


**Para aceder aos Relatórios de utilizadores sinalizados para risco no centro de administração do Office 365:**

1.  No menu de navegação no lado esquerdo, clique em **Centro de administração**. 
2.  Clique em **Azure AD**.
3.  Inicie sessão no **Centro de administração do Azure Active Directory**.
4.  Se for apresentada uma faixa na parte superior da página que diz **Veja o portal novo**, clique na ligação.
4.  No menu de navegação no lado esquerdo, clique em **Azure Active Directory**. 
5.  No painel de navegação, em **Segurança**, clique em **Utilizadores sinalizados para risco**.

Reveja as informações apresentadas aí. Deverá repor a palavra-passe de todas as contas que forem listadas. 

## <a name="remediation-actions"></a>Ações de remediação

Execute as ações seguintes para ajudar a retificar as contas afetadas e a proteger o seu ambiente:

1.  [Valide](http://aka.ms/MFAValid) as informações corretas para a autenticação multifator e a reposição personalizada de palavras-passe. 
2.  [Ative](http://aka.ms/MFAuth) a autenticação multifator para todos os utilizadores. 
3.  Ao utilizar este [script de remediação](http://aka.ms/remediate) em todas as contas afetadas, pode realizar automaticamente os passos abaixo: 

    a. Repor a palavra-passe para proteger a conta e abortar as sessões ativas.

    b. Remover delegados de caixas de correio.

    c. Desativar as regras de reencaminhamento de correio para domínios externos.

    d. Remover a propriedade de reencaminhamento de correio global na caixa de correio.

    e. Ativar a MFA na conta do utilizador.

    f. Definir uma complexidade de palavra-passe na conta alta.

    g. Ativar a auditoria da caixa de correio.

    h. Produzir Registos de Auditoria, para análise por parte do administrador.

4. Investigar o inquilino do Office 365 e outras infraestruturas de TI, incluindo uma análise de todas as definições do inquilino, das contas de utilizador e das definições de configuração por utilizador, para possível modificação. Verificar indicadores de métodos de persistência, bem como indicadores de que um intruso poderá ter tirado partido de uma presença inicial para obter credenciais de VPN ou acesso a outros recursos da organização. 

5.  Como parte da sua investigação, considere se deve ou tem de notificar as autoridades, incluindo as autoridades judiciais.

Além disso, deve:

- Ler e implementar estas [orientações](http://aka.ms/fixaccount) relativamente à abordagem a atividades invulgares. 
- [Ativar o pipeline de auditoria](http://aka.ms/improvesecurity) para ajudar a analisar a atividade nos seus inquilinos. Depois de concluído, o arquivo de auditoria começa a ser preenchido com todos os registos de atividade. Nesta fase, também pode tirar partido da funcionalidade [Search and Investigation do Security and Compliance Center](http://aka.ms/sccsearch). 
- Utilize este [script](http://aka.ms/mailboxaudit1) para ativar a auditoria das caixas de correio em todas as suas contas de correio. 
- Reveja as permissões de delegado e as regras de reencaminhamento de todas as caixas de correio de correio. Pode utilizar este [script do PowerShell](http://aka.ms/delegateforwardrules) para realizar esta tarefa. 



## <a name="next-steps"></a>Passos Seguintes

- Para obter mais informações sobre o Azure Active Directory Identity Protection, veja [Azure Active Directory Identity Protection](active-directory-identityprotection.md).

