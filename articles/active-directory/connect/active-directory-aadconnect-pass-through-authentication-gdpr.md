---
title: Autenticação de pass-through do Active Directory privacidade do utilizador e do Azure | Microsoft Docs
description: Este artigo lida com autenticação de pass-through do Azure Active Directory (Azure AD) e GDPR compatibilidade.
services: active-directory
keywords: Authentication do Azure AD Connect pass-through, GDPR, os componentes necessários para o Azure AD, SSO, o início de sessão único
documentationcenter: ''
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2018
ms.component: hybrid
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: 3343cebb85124f19fe773822e296312abad53d96
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34591179"
---
# <a name="user-privacy-and-azure-active-directory-pass-through-authentication"></a>Autenticação de pass-through do Active Directory do Azure e privacidade do utilizador


[!INCLUDE [Privacy](../../../includes/gdpr-intro-sentence.md)]

## <a name="overview"></a>Descrição geral

A autenticação pass-through do AD do Azure cria o tipo de registo seguinte, o que pode conter dados pessoais:

- Ficheiros de registo de rastreio do Azure AD Connect.
- Ficheiros de registo de rastreio de agente de autenticação.
- Ficheiros de registo de eventos do Windows.

Melhorar a privacidade do utilizador para autenticação pass-through de duas formas:

1.  Mediante pedido, extrair dados para uma pessoa e remover dados dessa pessoa das instalações.
2.  Certifique-se de que não existem dados são mantidos para além de 48 horas.

Recomendamos vivamente a segunda opção, é mais fácil de implementar e manter. Seguem-se as instruções para cada tipo de registo:

### <a name="delete-azure-ad-connect-trace-log-files"></a>Eliminar ficheiros de registo de rastreio do Azure AD Connect

Verifique o conteúdo do **%ProgramData%\AADConnect** conteúdo de registo de pasta e elimine o rastreio (**rastreio -\*. log** ficheiros) desta pasta dentro de 48 horas de instalar ou atualizar o Azure AD Connect ou modificar a configuração de autenticação pass-through, como esta ação pode criar dados abrangidos por GDPR.

>[!IMPORTANT]
>Não elimine o **PersistedState.xml** de ficheiros nesta pasta, este ficheiro é utilizado para manter o estado da instalação anterior do Azure AD Connect e é utilizado quando é efetuada uma instalação de atualização. Este ficheiro nunca irá conter todos os dados sobre uma pessoa e nunca deve ser eliminado.

Pode rever e eliminar estes ficheiros de registo de rastreio utilizando o Explorador do Windows ou pode utilizar o seguinte script do PowerShell para executar as ações necessárias:

```
$Files = ((Get-Item -Path "$env:programdata\aadconnect\trace-*.log").VersionInfo).FileName 
 
Foreach ($file in $Files) { 
    {Remove-Item -Path $File -Force} 
}
```

Guarde o script num ficheiro com o ". PS1 "extensão. Execute este script conforme necessário.

Para saber mais sobre relacionados com os requisitos do Azure AD Connect GDPR, consulte [neste artigo](active-directory-aadconnect-gdpr.md).

### <a name="delete-authentication-agent-event-logs"></a>Eliminar registos de eventos do agente de autenticação

Também pode criar este produto **registos de eventos do Windows**. Para obter mais informações, leia [neste artigo](https://msdn.microsoft.com/library/windows/desktop/aa385780(v=vs.85).aspx).

Para ver registos relacionados com o agente de autenticação pass-through, abra o **Visualizador de eventos** aplicação no servidor e verificação em **aplicações e serviço Logs\Microsoft\AzureAdConnect\AuthenticationAgent\Admin** .

### <a name="delete-authentication-agent-trace-log-files"></a>Eliminar ficheiros de registo de rastreio do agente de autenticação

Deve verificar regularmente os conteúdos do **%ProgramData%\Microsoft\Azure AD Connect Agent\Trace de autenticação\**  e elimine o conteúdo desta pasta cada 48 horas. 

>[!IMPORTANT]
>Se o agente de autenticação está em execução, não poderá eliminar o ficheiro de registo atual na pasta. Pare o serviço antes de tentar novamente. Para evitar falhas de início de sessão de utilizador, deve já configurou a autenticação pass-through para [elevada disponibilidade](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability).

Pode rever e eliminar estes ficheiros através do Explorador do Windows ou pode utilizar o seguinte script para executar as ações necessárias:

```
$Files = ((Get-childitem -Path "$env:programdata\microsoft\azure ad connect authentication agent\trace" -Recurse).VersionInfo).FileName 
 
Foreach ($file in $files) { 
    {Remove-Item -Path $File -Force} 
}
```

Para agendar a execução deste script cada 48 horas, siga estes passos:

1.  Guarde o script num ficheiro com o ". PS1 "extensão.
2.  Abra **painel de controlo** e clique em **sistema e segurança**.
3.  Sob o **ferramentas administrativas** cabeçalho, clique em "**agenda de tarefas**".
4.  No **Programador de tarefas**, faça duplo clique em "**biblioteca de agendamento de tarefas**"e clique em"**criar tarefas básicas...** ".
5.  Introduza o nome para a nova tarefa e clique em **seguinte**.
6.  Selecione "**diária**" para o **acionador da tarefa** e clique em **seguinte**.
7.  Defina a periodicidade para dois dias e clique em **seguinte**.
8.  Selecione "**iniciaram um programa**" como a ação e clique em **seguinte**.
9.  Tipo de "**PowerShell**"na caixa para o programa/script e na caixa de etiqueta"**adicionar argumentos (opcionais)**", introduza o caminho completo para o script que criou anteriormente, em seguida, clique em **seguinte**.
10. O aparecimento do ecrã mostra um resumo da tarefa que está prestes a criar. Verifique os valores e clique em **concluir** para criar a tarefa:
 
### <a name="note-about-domain-controller-logs"></a>Tenha em atenção sobre os registos do controlador de domínio

Se um registo de auditoria estiver ativado, este produto poderá gerar registos de segurança para os controladores de domínio. Para obter mais informações sobre como configurar políticas de auditoria, leia este [artigo](https://technet.microsoft.com/library/dd277403.aspx).

## <a name="next-steps"></a>Passos Seguintes
* [Rever a política de Microsoft Privacy no Centro de confiança](https://www.microsoft.com/trustcenter)
- [**Resolver problemas** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -Saiba como resolver problemas comuns com a funcionalidade.
