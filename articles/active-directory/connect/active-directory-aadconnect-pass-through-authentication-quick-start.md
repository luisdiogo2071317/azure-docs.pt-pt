---
title: Autenticação de pass-through do Azure AD - guia de introdução | Documentos da Microsoft
description: Este artigo descreve como começar com a autenticação pass-through do Azure Active Directory (Azure AD).
services: active-directory
keywords: Autenticação do Azure AD Connect pass-through, Active Directory, de instalação necessários componentes para o Azure AD, SSO, Single Sign-on
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: fc98f15303f23937d58131de971d5c60017c9034
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/09/2018
ms.locfileid: "37917715"
---
# <a name="azure-active-directory-pass-through-authentication-quick-start"></a>Do Azure Active Directory autenticação pass-through: Início rápido

## <a name="deploy-azure-ad-pass-through-authentication"></a>Implementar a autenticação do Azure AD pass-through

Autenticação de pass-through do Azure Active Directory (Azure AD) permite aos utilizadores iniciar sessão no local e aplicações baseadas na cloud utilizando as mesmas palavras-passe. Autenticação pass-through assina os utilizadores validando as palavras-passe diretamente no Active Directory no local.

>[!IMPORTANT]
>Se usar esse recurso por meio de uma versão de pré-visualização, certifique-se de que Atualize as versões de pré-visualização dos agentes de autenticação ao utilizar as instruções fornecidas no [autenticação de pass-through do Azure Active Directory: pré-visualização de atualização Agentes de autenticação](./active-directory-aadconnect-pass-through-authentication-upgrade-preview-authentication-agents.md).

Siga estas instruções para implementar a autenticação pass-through:

## <a name="step-1-check-the-prerequisites"></a>Passo 1: Verificar os pré-requisitos

Certifique-se de que os seguintes pré-requisitos são cumpridos.

### <a name="in-the-azure-active-directory-admin-center"></a>No Centro de administração do Azure Active Directory

1. Crie uma conta de administrador global apenas na cloud no seu inquilino do Azure AD. Dessa forma, pode gerir a configuração do seu inquilino devem seus serviços no local falhar ou ficar indisponíveis. Saiba mais sobre [adicionar uma conta de administrador global apenas na cloud](../active-directory-users-create-azure-portal.md). Concluir este passo é fundamental assegurar que não fica bloqueado fora do seu inquilino.
2. Adicionar um ou mais [nomes de domínio personalizados](../active-directory-domains-add-azure-portal.md) ao seu inquilino do Azure AD. Os utilizadores podem a iniciar sessão com um destes nomes de domínio.

### <a name="in-your-on-premises-environment"></a>No seu ambiente no local

1. Identifique um servidor com o Windows Server 2012 R2 ou posterior para executar o Azure AD Connect. Adicione o servidor à mesma floresta do Active Directory que os utilizadores cujas palavras-passe tem de validar.
2. Instalar o [a versão mais recente do Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) no servidor identificado no passo anterior. Se já tiver executado o Azure AD Connect, certifique-se de que a versão é 1.1.644.0 ou posterior.

    >[!NOTE]
    >O Azure AD Connect as versões 1.1.557.0, 1.1.558.0, 1.1.561.0 e 1.1.614.0 têm um problema relacionado com a sincronização de hash de palavra-passe. Se _não_ tenciona utilizar a sincronização de hash de palavra-passe em conjunto com a autenticação pass-through, leia o [notas de versão do Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#116470).

3. Identifique um servidor adicional (com o Windows Server 2012 R2 ou posterior) onde pode executar o agente de autenticação autónomo. A versão do agente de autenticação tem de ser 1.5.193.0 ou posterior. Este servidor adicional é necessária para garantir a disponibilidade elevada de pedidos para iniciar sessão. Adicione o servidor à mesma floresta do Active Directory que os utilizadores cujas palavras-passe tem de validar.
4. Se existir uma firewall entre os servidores e do Azure AD, configure os seguintes itens:
   - Certifique-se de que os agentes de autenticação podem efetuar *saída* pedidos para o Azure AD sobre as seguintes portas:
   
    | Número da porta | Como são utilizadas |
    | --- | --- |
    | **80** | Transfere as listas de revogação de certificados (CRL) ao validar o certificado SSL |
    | **443** | Processa todas as comunicações de saída com o serviço |
   
    Se a firewall impuser regras de acordo com os utilizadores de origem, abra estas portas para o tráfego dos serviços do Windows que são executados como um serviço de rede.
   - Se sua firewall ou proxy permite que listas de permissões de DNS, ligações de lista de permissões para  **\*. msappproxy.net** e  **\*. servicebus.windows.net**. Se não, permitir o acesso para o [intervalos IP do datacenter do Azure](https://www.microsoft.com/download/details.aspx?id=41653), que é atualizado semanalmente.
   - Os agentes de autenticação precisam acessar **login.windows.net** e **login.microsoftonline.com** para o registo inicial. Abra a firewall para essas URLs também.
   - Para validação do certificado, os seguintes URLs de desbloqueio: **mscrl.microsoft.com:80**, **crl.microsoft.com:80**, **ocsp.msocsp.com:80**, e  **www.microsoft.com:80**. Estes URLs são utilizadas para validação de certificados com outros produtos da Microsoft. Poderá já ter estes URLs desbloqueados.

## <a name="step-2-enable-exchange-activesync-support-optional"></a>Passo 2: Ativar o suporte do Exchange ActiveSync (opcional)

Siga estas instruções para ativar o suporte do Exchange ActiveSync:

1. Uso [Exchange PowerShell](https://technet.microsoft.com/library/mt587043(v=exchg.150).aspx) para executar o seguinte comando:
```
Get-OrganizationConfig | fl per*
```

2. Verificar o valor da `PerTenantSwitchToESTSEnabled` definição. Se o valor for **true**, seu inquilino está corretamente configurado. Isso geralmente é o caso para a maioria dos clientes. Se o valor for **false**, execute o seguinte comando:
```
Set-OrganizationConfig -PerTenantSwitchToESTSEnabled:$true
```

3. Certifique-se de que o valor do `PerTenantSwitchToESTSEnabled` definição está agora definida como **true**. Aguarde uma hora antes de passar para o passo seguinte.

Se tiver quaisquer problemas durante este passo, consulte a [guia de resolução de problemas](active-directory-aadconnect-troubleshoot-pass-through-authentication.md#exchange-activesync-configuration-issues).

## <a name="step-3-enable-the-feature"></a>Passo 3: Ativar a funcionalidade

Ativar a autenticação pass-through através de [do Azure AD Connect](active-directory-aadconnect.md).

>[!IMPORTANT]
>Pode ativar a autenticação pass-through no servidor primário ou de teste do Azure AD Connect. Deve habilitá-la do servidor primário.

Se estiver a instalar o Azure AD Connect pela primeira vez, escolha o [caminho de instalação personalizada](active-directory-aadconnect-get-started-custom.md). Na **sessão do utilizador** página, selecione **autenticação pass-through** como o **método de início de sessão**. A conclusão com êxito, um agente de autenticação pass-through está instalado no mesmo servidor do Azure AD Connect. Além disso, a funcionalidade de autenticação pass-through está ativada no seu inquilino.

![O Azure AD Connect: Utilizador início de sessão](./media/active-directory-aadconnect-sso/sso3.png)

Se já tiver instalado o Azure AD Connect utilizando as [instalação rápida](active-directory-aadconnect-get-started-express.md) ou o [instalação personalizada](active-directory-aadconnect-get-started-custom.md) caminho, selecione o **alterar utilizador inicie sessão** tarefas no Azure AD Ligar e, em seguida, selecione **seguinte**. Em seguida, selecione **autenticação pass-through** como o método de início de sessão. A conclusão com êxito, um agente de autenticação pass-through é instalado no mesmo servidor do Azure AD Connect e a funcionalidade está ativada no seu inquilino.

![O Azure AD Connect: Alterar a sessão do utilizador](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

>[!IMPORTANT]
>Autenticação pass-through é um recurso de nível de inquilino. Ativá-la afeta o início de sessão para os utilizadores em todas _todos os_ os domínios geridos no seu inquilino. Se estiver a mudar de serviços de Federação do Active Directory (AD FS) para autenticação pass-through, deve esperar, pelo menos, 12 horas antes de encerrar a sua infraestrutura do AD FS. Este tempo de espera é garantir que os utilizadores podem manter início de sessão no Exchange ActiveSync durante a transição.

## <a name="step-4-test-the-feature"></a>Passo 4: Testar a funcionalidade

Siga estas instruções para verificar que ativou a autenticação pass-through corretamente:

1. Inicie sessão para o [Centro de administração do Azure Active Directory](https://aad.portal.azure.com) com as credenciais de administrador global do seu inquilino.
2. Selecione **do Azure Active Directory** no painel esquerdo.
3. Selecione **do Azure AD Connect**.
4. Certifique-se de que o **autenticação pass-through** funcionalidade é apresentado como **ativado**.
5. Selecione **autenticação pass-through**. O **autenticação pass-through** painel lista os servidores onde estão instalados os agentes de autenticação.

![Centro de administração do Azure Active Directory: painel do Azure AD Connect](./media/active-directory-aadconnect-pass-through-authentication/pta7.png)

![Centro de administração do Azure Active Directory: o painel de autenticação pass-through](./media/active-directory-aadconnect-pass-through-authentication/pta8.png)

Nesta fase, os utilizadores de todos os domínios geridos no seu inquilino podem iniciar sessão utilizando a autenticação pass-through. No entanto, os utilizadores de domínios federados continuam iniciar sessão com o AD FS ou outro fornecedor de federação que configurou anteriormente. Se converter um domínio de federado gerenciados, todos os utilizadores de que o domínio iniciar automaticamente a iniciar sessão utilizando a autenticação pass-through. A funcionalidade de autenticação pass-through não afeta os utilizadores apenas na cloud.

## <a name="step-5-ensure-high-availability"></a>Passo 5: Certifique-se de elevada disponibilidade

Se planeia implementar autenticação pass-through num ambiente de produção, deve instalar, pelo menos, autónomo mais um agente de autenticação. Instalar estes agentes de autenticação no servidor (es) _outros_ que um AD Connect do Azure em execução. Esta configuração fornece elevada disponibilidade para pedidos de início de sessão do utilizador.

Siga estas instruções para transferir o software do agente de autenticação:

1. Para transferir a versão mais recente do agente de autenticação (versão 1.5.193.0 ou posterior), iniciar sessão para o [Centro de administração do Azure Active Directory](https://aad.portal.azure.com) com credenciais de administrador global do seu inquilino.
2. Selecione **do Azure Active Directory** no painel esquerdo.
3. Selecione **do Azure AD Connect**, selecione **autenticação pass-through**e, em seguida, selecione **transferir o agente**.
4. Selecione o **aceitar os termos e transferir** botão.

![Centro de administração do Azure Active Directory: botão de transferir o agente de autenticação](./media/active-directory-aadconnect-pass-through-authentication/pta9.png)

![Centro de administração do Azure Active Directory: o painel de transferir o agente](./media/active-directory-aadconnect-pass-through-authentication/pta10.png)

>[!NOTE]
>Pode transferir o software do agente de autenticação também diretamente [aqui](https://aka.ms/getauthagent). Rever e aceitar o agente de autenticação [termos de serviço](https://aka.ms/authagenteula) _antes de_ instalá-lo.

Existem duas formas de implementar um agente de autenticação de autónomo:

Em primeiro lugar, pode fazê-lo interativamente apenas executar o agente de autenticação transferido executável e fornecendo credenciais de administrador global do seu inquilino, quando lhe for pedido.

Em segundo lugar, pode criar e executar um script de implementação automática. Isto é útil quando pretender implementar vários agentes de autenticação de uma só vez ou instalar agentes de autenticação nos servidores de Windows que não têm a interface de utilizador ativado ou que não pode acessar com o ambiente de trabalho remoto. Aqui estão as instruções sobre como usar essa abordagem:

1. Execute o seguinte comando para instalar um agente de autenticação: `AADConnectAuthAgentSetup.exe REGISTERCONNECTOR="false" /q`.
2. Pode registrar o agente de autenticação com o serviço com o Windows PowerShell. Criar um objeto de credenciais de PowerShell `$cred` que contém um nome de utilizador de administrador global e a palavra-passe para o seu inquilino. Execute o seguinte comando, substituindo *\<nome de utilizador\>* e  *\<palavra-passe\>*:
   
        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword
3. Aceda a **ligar o agente de autenticação C:\Program Files\Microsoft Azure AD** e execute o script seguinte com o `$cred` objeto que criou:
   
        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft Azure AD Connect Authentication Agent\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred -Feature PassthroughAuthentication

## <a name="next-steps"></a>Passos Seguintes
- [Bloqueio do smart](../authentication/howto-password-smart-lockout.md): Saiba como configurar a capacidade de bloqueio inteligente no seu inquilino para proteger contas de utilizador.
- [Limitações atuais](active-directory-aadconnect-pass-through-authentication-current-limitations.md): Saiba quais cenários são suportados com a autenticação pass-through e quais não são.
- [Análise técnica aprofundada](active-directory-aadconnect-pass-through-authentication-how-it-works.md): compreender como funciona a funcionalidade de autenticação pass-through.
- [Perguntas mais frequentes sobre](active-directory-aadconnect-pass-through-authentication-faq.md): encontre respostas para perguntas mais frequentes.
- [Resolver problemas de](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): Saiba como resolver problemas comuns com a funcionalidade de autenticação pass-through.
- [Detalhada da segurança](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md): Obtenha informações técnicas sobre a funcionalidade de autenticação pass-through.
- [O Azure AD Seamless SSO](active-directory-aadconnect-sso.md): Saiba mais sobre esta funcionalidade complementar.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): Utilize o Fórum do Azure Active Directory para pedidos de novas funcionalidades de ficheiros.

