---
title: Estados do utilizador multi-factor Authentication do Microsoft Azure
description: Saiba mais sobre os Estados do utilizador no multi-factor Authentication.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 06/26/2017
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.openlocfilehash: 6945966d4a701ea6e2684b7da766c8b6c9f9a283
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39049053"
---
# <a name="how-to-require-two-step-verification-for-a-user-or-group"></a>Como requerer verificação de dois passos para um utilizador ou grupo

Pode efetuar uma de duas abordagens para exigir a verificação de dois passos. A primeira opção é permitir que cada utilizador para o Azure multi-factor Authentication (MFA). Quando os utilizadores estiverem ativados individualmente, eles executar a verificação de cada vez que iniciar sessão (com algumas exceções, por exemplo, quando iniciarem sessão de IP fidedigno endereços ou quando o _memorizadas dispositivos_ funcionalidade estiver ativada). A segunda opção é definir uma política de acesso condicional que exige a verificação de dois passos sob determinadas condições.

>[!TIP] 
>Escolha um dos seguintes métodos para exigir verificação de dois passos, não ambos. Ativar um utilizador para o Azure multi-factor Authentication substitui quaisquer políticas de acesso condicional.

## <a name="which-option-is-right-for-you"></a>Qual é a opção certa para si?

**Ativar a multi-factor Authentication do Azure com a alteração de Estados do utilizador** é a abordagem tradicional para exigir a verificação de dois passos. Funciona para ambos os Azure MFA na cloud e o servidor MFA do Azure. Todos os utilizadores que lhe permitem executam verificação de dois passos sempre que iniciam sessão. Permitindo que um usuário substitui quaisquer políticas de acesso condicional que podem afetar esse utilizador. 

**Ativar a multi-factor Authentication do Azure com uma política de acesso condicional** é uma abordagem mais flexível para exigir a verificação de dois passos. Ele só funciona para o Azure MFA na cloud, no entanto, e _acesso condicional_ é um [paga os recursos do Azure Active Directory](https://www.microsoft.com/cloud-platform/azure-active-directory-features). Pode criar políticas de acesso condicional que se aplicam a grupos, bem como os utilizadores individuais. Grupos de alto risco podem receber mais restrições que grupos de baixo risco ou verificação de dois passos pode ser necessário apenas para aplicações na cloud de alto risco e foi ignorada para aqueles de baixo risco. 

Ambas as opções de pedem aos utilizadores para se registrar para o Azure multi-factor Authentication a primeira vez que iniciar sessão depois de ativassem os requisitos. Ambas as opções também funcionam com o configuráveis [definições de multi-factor Authentication](howto-mfa-mfasettings.md).

## <a name="enable-azure-mfa-by-changing-user-status"></a>Ativar a MFA do Azure ao alterar o estado do utilizador

Contas de utilizador no multi-factor Authentication do Azure tem os seguintes três Estados distintos:

| Estado | Descrição | Aplicações não baseadas no browser afetadas | Aplicações de browser afetadas | Autenticação moderna afetada |
|:---:|:---:|:---:|:--:|:--:|
| Desativado |O estado predefinido para um novo utilizador não estiver inscrito no MFA do Azure. |Não |Não |Não |
| Ativado |O utilizador inscreveu na MFA do Azure, mas não se registou. Eles recebem um pedido de registo da próxima vez que iniciarem sessão. |Não.  Eles continuar a funcionar até que o processo de registo é concluído. | Sim. Depois da sessão expira, o registo de MFA do Azure é necessário.| Sim. Depois do token de acesso expira, o registo de MFA do Azure é necessário. |
| Imposto |O utilizador foi inscrito e concluiu o processo de registo do MFA do Azure. |Sim.  As aplicações necessitam de palavras-passe de aplicação. |Sim. A MFA do Azure é necessária no início de sessão. | Sim. A MFA do Azure é necessária no início de sessão. |

Um Estado do usuário reflete se um administrador inscreveu-los na MFA do Azure e, se eles concluir o processo de registo.

Todos os utilizadores começam *desativado*. Quando inscreve os utilizadores na MFA do Azure, o estado muda para *ativado*. Quando os utilizadores ativados iniciar sessão e concluir o processo de registo, o estado muda para *imposto*.  

### <a name="view-the-status-for-a-user"></a>Ver o estado de um utilizador

Utilize os seguintes passos para aceder à página onde pode ver e gerir perfis de usuário:

1. Inicie sessão no [portal do Azure](https://portal.azure.com) como administrador.
2. Aceda a **do Azure Active Directory** > **utilizadores e grupos** > **todos os utilizadores**.
3. Selecione **multi-factor Authentication**.
   ![Selecione o multi-factor Authentication](./media/howto-mfa-userstates/selectmfa.png)
4. É aberta uma nova página que mostra os Estados do utilizador.
   ![Estado de utilizador do multi-factor authentication - captura de ecrã](./media/howto-mfa-userstates/userstate1.png)

### <a name="change-the-status-for-a-user"></a>Alterar o estado de um utilizador

1. Utilizar os passos anteriores para obter a multi-factor Authentication **utilizadores** página.
2. Localize o utilizador que pretende ativar a MFA do Azure. Poderá ter de alterar a vista na parte superior. 
   ![Localizar utilizador - captura de ecrã](./media/howto-mfa-userstates/enable1.png)
3. Selecione a caixa junto do respetivo nome.
4. No lado direito, sob **passos rápidos**, escolha **ativar** ou **desativar**.
   ![Ativar o utilizador selecionado - captura de ecrã](./media/howto-mfa-userstates/user1.png)

   >[!TIP]
   >*Ativada* os usuários estão automaticamente no modo *imposto* quando eles se registrar para o MFA do Azure. Fazer manualmente não alterar o estado do utilizador *imposto*. 

5. Confirme a sua seleção na janela de pop-up que é aberta. 

Depois de ativar os utilizadores, notificá-los por e-mail. Informe-os de que estes serão solicitados para se registar na próxima vez que iniciarem sessão. Além disso, se sua organização utilizar aplicações não baseadas no browser que não suportam a autenticação moderna, terá de criar palavras-passe de aplicação. Também pode incluir uma ligação para o [Guia do utilizador final de MFA do Azure](../user-help/multi-factor-authentication-end-user.md) para ajudá-los a começar a utilizar.

### <a name="use-powershell"></a>Utilizar o PowerShell
Alterar o estado do utilizador utilizando [do Azure AD PowerShell](/powershell/azure/overview), altere `$st.State`. Existem três Estados possíveis:

* Ativado
* Imposto
* Desativado  

Não é necessário mover os utilizadores diretamente para o *imposto* estado. Se o fizer, aplicações não baseadas no browser deixam de funcionar porque o utilizador não passou pelo registo do Azure MFA nem obteve uma [palavra-passe de aplicação](howto-mfa-mfasettings.md#app-passwords). 

Com o PowerShell é uma boa opção quando precisa ativar utilizadores em massa. Crie um script do PowerShell que executa um loop através de uma lista de utilizadores e ativa-as:

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

O script seguinte é um exemplo:

    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }

## <a name="enable-azure-mfa-with-a-conditional-access-policy"></a>Ativar a MFA do Azure com uma política de acesso condicional

_Acesso condicional_ é uma funcionalidade paga do Azure Active Directory, com muitas opções de configuração. Estes passos irão guiá uma forma de criar uma política. Para obter mais informações, leia sobre [acesso condicional no Azure Active Directory](../active-directory-conditional-access-azure-portal.md).

1. Inicie sessão no [portal do Azure](https://portal.azure.com) como administrador.
2. Aceda a **do Azure Active Directory** > **acesso condicional**.
3. Selecione **nova política**.
4. Sob **atribuições**, selecione **utilizadores e grupos**. Utilize o **inclusão** e **excluir** separadores para especificar os utilizadores e grupos a política gere.
5. Sob **atribuições**, selecione **aplicações na Cloud**. Optar por incluir **todas as aplicações na cloud**.
6. Sob **controlos de acesso**, selecione **concessão**. Escolher **exigir autenticação multifator**.
7. Ative **ativar política** ao **no**e, em seguida, selecione **guardar**.

As outras opções na política de acesso condicional dão-lhe a capacidade de especificar exatamente quando a verificação de dois passos é necessária. Por exemplo, pode fazer uma política como esta: quando tentam prestadores de serviço aceder a nossa aplicação de aprovisionamento de redes não fidedignas em dispositivos que não estão associados a um domínio, exigir verificação de dois passos. 

## <a name="next-steps"></a>Passos Seguintes

- Obtenha dicas sobre o [melhores práticas para acesso condicional](../active-directory-conditional-access-best-practices.md).

- Gerir as definições de multi-factor Authentication [os seus utilizadores e os respetivos dispositivos](howto-mfa-userdevicesettings.md).
