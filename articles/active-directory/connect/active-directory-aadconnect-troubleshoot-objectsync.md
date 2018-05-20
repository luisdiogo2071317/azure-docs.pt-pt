---
title: 'O Azure AD Connect: Resolver problemas de sincronização de objetos | Microsoft Docs'
description: Este tópico fornece os passos sobre como resolver problemas com a sincronização de objeto através da tarefa resolução de problemas.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: curtand
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: billmath
ms.openlocfilehash: 9945ad30cc7d8882d8b99f6b4278f2063ab4b7f7
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="troubleshoot-object-synchronization-with-azure-ad-connect-sync"></a>Resolver problemas de sincronização de objetos com a sincronização do Azure AD Connect
Este documento fornece os passos sobre como resolver problemas com a sincronização de objeto através da tarefa resolução de problemas.

## <a name="troubleshooting-task"></a>Tarefas de resolução de problemas
Para o Azure Active Directory (AAD) ligar implementação com a versão 1.1.749.0 ou superior, utilize a tarefa de resolução de problemas no Assistente para resolver problemas de sincronização do objeto. Para versões anteriores,. resolver manualmente, conforme descrito [aqui](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md).

### <a name="run-the-troubleshooting-task-in-the-wizard"></a>Executar a tarefa de resolução de problemas no Assistente
Para executar a tarefa de resolução de problemas do assistente, execute os seguintes passos:

1.  Abra uma nova sessão do Windows PowerShell no servidor do Azure AD Connect com a executar como opção de administrador.
2.  Executar `Set-ExecutionPolicy RemoteSigned` ou `Set-ExecutionPolicy Unrestricted`.
3.  Inicie o Assistente do Azure AD Connect.
4.  Navegue para a página de tarefas adicionais, selecione de resolução de problemas e clique em seguinte.
5.  Na página de resolução de problemas, clique em Iniciar para iniciar o menu de resolução de problemas no PowerShell.
6.  No menu principal, selecione a resolver problemas de sincronização de objetos.
![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch11.png)

### <a name="troubleshooting-input-parameters"></a>Os parâmetros de entrada de resolução de problemas
Os seguintes parâmetros de entrada necessários para a tarefa de resolução de problemas:
1.  **Nome único do objeto** – este é o nome único do objeto que necessita de resolução de problemas
2.  **Nome do conector do AD** – este é o nome de floresta do AD, onde reside o objeto acima.
3.  Credenciais de administrador global de inquilino do Azure AD ![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch1.png)

### <a name="understand-the-results-of-the-troubleshooting-task"></a>Compreender os resultados da tarefa resolução de problemas
A tarefa de resolução de problemas efetua as seguintes verificações:

1.  Erro de correspondência de UPN de detetar se o objeto está sincronizado com o Azure Active Directory
2.  Verifique se o objeto é filtrado devido a filtragem de domínio
3.  Verifique se o objeto é filtrado devido à filtragem de UO
4.  Verifique se a sincronização de objetos é bloqueada devido a uma caixa de correio ligada
5. Verifique se o objeto é o grupo de distribuição dinâmica que não seja deveria ser sincronizados

As restantes desta secção descreve os resultados específicos que são devolvidos pela tarefa. Em cada caso, a tarefa fornece uma análise seguida ações recomendadas para resolver o problema.

## <a name="detect-upn-mismatch-if-object-is-synced-to-azure-active-directory"></a>Erro de correspondência de UPN de detetar se o objeto está sincronizado com o Azure Active Directory
### <a name="upn-suffix-is-not-verified-with-azure-ad-tenant"></a>Sufixo UPN não é verificado com o inquilino do Azure AD
Quando UserPrincipalName (UPN) / sufixo do ID de início de sessão alternativo não é verificado com o inquilino do Azure AD, em seguida, do Azure Active Directory substitui os sufixos UPN com o nome de domínio predefinida "onmicrosoft.com".

![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch2.png)

### <a name="changing-upn-suffix-from-one-federated-domain-to-another-federated-domain"></a>Alterar o sufixo UPN de um domínio federado para outro domínio federado
Azure Active Directory não permite a sincronização de UserPrincipalName (UPN) / Federado de alteração de sufixo do ID de início de sessão alternativo de um domínio federado para outro domínio. Isto aplica-se a domínios, que são verificados com o inquilino do Azure AD e tem o tipo de autenticação que Federated.

![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch3.png) 

### <a name="azure-ad-tenant-dirsync-feature-synchronizeupnformanagedusers-is-disabled"></a>Azure AD inquilino DirSync 'SynchronizeUpnForManagedUsers' está desativada
Quando a funcionalidade de DirSync de inquilino do Azure AD 'SynchronizeUpnForManagedUsers' está desativada, Azure Active Directory não permite atualizações de sincronização para o ID de início de sessão de UserPrincipalName/alternativo para contas de utilizador licenciado com autenticação gerida.

![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch4.png)

## <a name="object-is-filtered-due-to-domain-filtering"></a>Objeto é filtrado devido a filtragem de domínio
### <a name="domain-is-not-configured-to-sync"></a>Domínio não está configurado para sincronização
Objeto está fora do âmbito devido ao domínio não configurado. No exemplo abaixo, o objeto está dessincronizado âmbito como o domínio a que pertence está filtrado de sincronização.

![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch5.png)

### <a name="domain-is-configured-to-sync-but-is-missing-run-profilesrun-steps"></a>Domínio está configurado para sincronizar mas falta passos de perfis de execução/execução
Objeto está fora do âmbito como o domínio está em falta executar passos de perfis/executar. No exemplo abaixo, o objeto está dessincronizado do âmbito que o domínio a que pertence está em falta passos de execução para a importação completa perfil de execução.
![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch6.png)

## <a name="object-is-filtered-due-to-ou-filtering"></a>Objeto é filtrado devido à filtragem de UO
O objecto está dessincronizado âmbito devido à configuração de filtragem de UO. No exemplo abaixo, o objeto pertence a UO = NoSync, DC = bvtadwbackdc, DC = com.  Essa UO não está incluído no âmbito da sincronização.</br>

![UO](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch7.png)

## <a name="linked-mailbox-issue"></a>Problema de caixa de correio ligado
Uma caixa de correio ligada deve para ser associado com uma conta de principal externa localizada noutra floresta conta fidedigna. Se não houver nenhuma conta deste género externa principal, em seguida, o Azure AD Connect não irá sincronizar o utilizador conta corresponde a caixa de correio ligada na floresta do Exchange para o inquilino do Azure AD.</br>
![Caixa de correio ligada](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch12.png)

## <a name="dynamic-distribution-group-issue"></a>Problema de grupo dinâmico de distribuição
Devido a várias diferenças entre no local do Active Directory e do Azure Active Directory, Azure AD Connect não sincronizar a grupos de distribuição dinâmica para o inquilino do Azure AD.

![Grupo de distribuição dinâmico](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch13.png)

## <a name="html-report"></a>Relatório de HTML
Além de analisar o objeto, a tarefa de resolução de problemas também gera um relatório HTML que contém tudo sobre o objeto. Este relatório HTML pode ser partilhado com a equipa de suporte para o fazer adicionais de resolução de problemas, se necessário.

![](media\active-directory-aadconnect-troubleshoot-objectsynch\objsynch8.png)

## <a name="next-steps"></a>Passos Seguintes
Saiba mais sobre como [Integrar as identidades no local ao Azure Active Directory](active-directory-aadconnect.md).
