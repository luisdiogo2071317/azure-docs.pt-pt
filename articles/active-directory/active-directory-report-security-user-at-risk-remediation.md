---
title: "Utilizadores sinalizados no relatório de segurança de risco no portal do Azure Active Directory | Microsoft Docs"
description: "Saiba mais sobre os utilizadores sinalizados no relatório de segurança de risco no portal do Azure Active Directory"
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: ed6201e9edcef39b14b948b6b2f6e0b5da01ec60
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/05/2018
---
# <a name="remediate-users-flagged-for-risk-in-the-azure-active-directory-portal"></a>Remediar utilizadores sinalizados para risco no portal do Azure Active Directory

Com os relatórios de segurança no Azure Active Directory (Azure AD), pode obter informações sobre a probabilidade de contas de utilizador comprometidas no seu ambiente. Os [utilizadores sinalizados para risco](active-directory-identityprotection.md#users-flagged-for-risk) são um indicador de uma conta de utilizador que pode ter sido comprometida.

A Microsoft está empenhada em ajudar a manter os seus ambientes seguros. Como parte desse compromisso, a Microsoft monitoriza continuamente atividades que são invulgares ou que são consistentes com padrões de ataques conhecidos. 


Se foram detetadas atividades invulgares que possam indicar acesso não autorizado a algumas das suas contas de utilizador, receberá notificações que lhe permitem tomar medidas. A disponibilização das notificações não significa que os sistemas da Microsoft tenham sido comprometidos de alguma forma.
 

## <a name="azure-active-directory-report-access"></a>Acesso ao relatório do Azure Active Diectory

Pode rever os utilizadores sinalizados para risco através de um relatório do Azure Active Directory online. Se não for subscritor do Azure, pode seguir o processo de subscrição sem custos, em [http://aka.ms/AccessAAD](http://aka.ms/AccessAAD).  
Depois de concluído, pode utilizar as suas credenciais do Office 365 para aceder ao Centro de Administração do Azure. Tenha em conta que, ao nível da subscrição básica, a quantidade de detalhes fornecidos é limitada. Estão disponíveis mais dados e análises para os subscritores Premium do Azure. Para obter informações, veja [Relatório de segurança de Utilizadores sinalizados para risco no portal do Azure Active Directory](active-directory-reporting-security-user-at-risk.md).

Depois de ativar o acesso ao Azure AD, é redirecionado para o [portal do Azure AD](https://portal.azure.com). Vá diretamente para o relatório e navegue para o URL [https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/UsersAtRisk](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/UsersAtRisk).

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



## <a name="next-steps"></a>Passos seguintes

- Para obter mais informações sobre o Azure Active Directory Identity Protection, veja [Azure Active Directory Identity Protection](active-directory-identityprotection.md).

