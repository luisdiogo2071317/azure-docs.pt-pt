---
title: Azure AD SSPR no ecrã de início de sessão do Windows 10
description: Neste tutorial, vai ativar a reposição de palavra-passe no ecrã de início de sessão do Windows 10 para reduzir as chamadas de suporte técnico.
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: tutorial
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: f2f2208f325728275706eeed9ff16e8afc3b11cf
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166882"
---
# <a name="tutorial-azure-ad-password-reset-from-the-login-screen"></a>Tutorial: Reposição de palavra-passe do Azure AD no ecrã de início de sessão

Neste tutorial, vai permitir aos utilizadores repor as respetivas palavras-passe no ecrã de início de sessão do Windows 10. Com a nova Atualização de 10 de abril de 2018 do Windows, os utilizadores que tenham dispositivos **associados ao Azure AD** ou **associados ao Azure AD híbrido** podem utilizar uma ligação "Repor palavra-passe" no ecrã de início de sessão. Quando clicam nesta ligação, são encaminhados para a experiência de reposição personalizada de palavra-passe (SSPR) que já conhecem.

> [!div class="checklist"]
> * Configurar a ligação Repor palavra-passe com o Intune
> * Opcionalmente, pode configurar através do Registo do Windows
> * Compreender o que os utilizadores vão ver

## <a name="prerequisites"></a>Pré-requisitos

* Atualização de 10 de abril de 2018 do Windows ou o cliente mais recente:
   * [associado ao Azure AD](../device-management-azure-portal.md) ou 
   * [associado ao Azure AD Híbrido](../device-management-hybrid-azuread-joined-devices-setup.md)
* A reposição de palavras-passe self-service do Azure AD tem de estar ativada.

## <a name="configure-reset-password-link-using-intune"></a>Configurar a ligação Repor palavra-passe com o Intune

Implementar a alteração da configuração para ativar a reposição de palavra-passe no ecrã de início de sessão com o Intune é o método mais flexível. O Intune permite implementar a alteração de configuração num grupo específico de máquinas que definir. Este método exige a inscrição do dispositivo no Intune.

### <a name="create-a-device-configuration-policy-in-intune"></a>Criar uma política de configuração de dispositivos no Intune

1. Inicie sessão no [portal do Azure](https://portal.azure.com) e clique em **Intune**.
2. Aceda a **Configuração de dispositivos** > **Perfis** > **Criar Perfil** para criar um perfil de configuração de dispositivos novo
   * Dê um nome relevante ao perfil
   * Opcionalmente, indique uma descrição relevante do perfil
   * Plataforma **Windows 10 e posterior**
   * Tipo de perfil **Personalizado**

3. Configure as **Definições**
   * **Adicione** a Definição de OMA-URI seguinte para ativar a ligação Repor palavra-passe
      * Indique um nome relevante para explicar o que a definição faz
      * Opcionalmente, indique uma descrição relevante da definição
      * **OMA-URI** definido como `./Vendor/MSFT/Policy/Config/Authentication/AllowAadPasswordReset`
      * **Tipo de dados** definido como **Inteiro**
      * **Valor** definido como **1**
      * Clique em **OK**
   * Clique em **OK**
4. Clique em **Criar**.

### <a name="assign-a-device-configuration-policy-in-intune"></a>Atribuir uma política de configuração de dispositivos no Intune

#### <a name="create-a-group-to-apply-device-configuration-policy-to"></a>Criar um grupo ao qual aplicar a política de configuração de dispositivos

1. Inicie sessão no [portal do Azure](https://portal.azure.com) e clique em **Azure Active Directory**.
2. Navegue até **Utilizadores e grupos** > **Todos os grupos** > **Novo grupo**
3. Indique um nome para o grupo e, em **tipo de associação**, escolha **Atribuído**
   * Em **Membros**, escolha os dispositivos Windows 10 associados ao Azure AD aos quais pretende aplicar a política
   * Clique em **Selecionar**
4. Clique em **Criar**.

Estão disponíveis mais informações sobre a criação de grupos no artigo [Manage access to resources with Azure Active Directory groups](../fundamentals/active-directory-manage-groups.md) (Gerir o acesso a recursos com grupos do Azure Active Directory).

#### <a name="assign-device-configuration-policy-to-device-group"></a>Atribuir a política de configuração de dispositivos ao grupo de dispositivos

1. Inicie sessão no [portal do Azure](https://portal.azure.com) e clique em **Intune**.
2. Aceda a **Configuração de dispositivos** > **Perfis** > clique no perfil criado anteriormente para localizar o perfil de configuração de dispositivos que criámos acima.
3. Atribuir o perfil a um grupo de dispositivos 
   * Clique em **Atribuições** > em **Incluir** > **Selecionar grupos a incluir**
   * Selecione o grupo criado anteriormente e clique em **Selecionar**
   * Clique em **Guardar**

   ![Atribuição][Assignment]

Criou e atribuiu agora uma política de configuração de dispositivos para ativar a ligação Repor palavra-passe no ecrã de início de sessão com o Intune.

## <a name="configure-reset-password-link-using-the-registry"></a>Configurar a ligação Repor palavra-passe com o registo

1. Utilizar credenciais administrativas para iniciar sessão no PC Windows
2. Execute **regedit** como administrador
3. Defina a chave de registo seguinte
   * `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\AzureADAccount`
      * `"AllowPasswordReset"=dword:00000001`

## <a name="what-do-users-see"></a>O que os utilizadores veem

Agora que a política está configurada e atribuída, que alterações há para o utilizador? Como é que podem saber que é permitido repor a palavra-passe no ecrã de início de sessão?

![LoginScreen][LoginScreen]

Quando os utilizadores tentam iniciar sessão, veem agora a ligação Repor palavra-passe, a qual abre a experiência de reposição personalizada de palavra-passe no ecrã de início de sessão. Com esta funcionalidade, os utilizadores podem repor as palavras-passe sem terem de utilizar outro dispositivo para aceder a um browser.

Os seus utilizadores podem obter orientações sobre como utilizar esta funcionalidade em [Reset your work or school password](../user-help/active-directory-passwords-update-your-own-password.md#reset-password-at-sign-in) (Repor a palavra-passe da conta escolar ou profissional)

## <a name="common-issues"></a>Problemas comuns

Ao testar esta funcionalidade com o Hyper-V, a ligação "Repor palavra-passe" não aparece.

* Aceda à VM com a qual está a testar, clique em **Ver** e desmarque **Sessão avançada**.

Quando testar esta funcionalidade com o Ambiente de Trabalho Remoto, a ligação "Repor palavra-passe" não aparece.

* A reposição de palavras-passe não é suportada em Ambientes de Trabalho Remotos atualmente.

Se o ecrã de bloqueio do Windows for desativado com uma chave de registo ou uma política de grupo, **Repor palavra-passe** não estará disponível.

O registo de auditoria do Azure AD irá incluir informações sobre o endereço IP e ClientType onde ocorreu a reposição de palavra-passe.

![Ecrã de exemplo de início de sessão com reposição de palavra-passe no registo de auditoria do Azure AD](media/tutorial-sspr-windows/windows-sspr-azure-ad-audit-log.png)

## <a name="clean-up-resources"></a>Limpar recursos

Se decidir que já não quer utilizar a funcionalidade que configurou como parte deste tutorial, elimine o perfil de configuração de dispositivos do Intune que criou ou a chave de registo.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, permitiu aos utilizadores repor as respetivas palavras-passe no ecrã de início de sessão do Windows 10. Avance para o próximo tutorial para ver como o Azure Identity Protection pode ser integrado na reposição personalizada de palavra-passe e nas experiências de Multi-Factor Authentication.

> [!div class="nextstepaction"]
> [Avaliar o risco de início de sessão](tutorial-risk-based-sspr-mfa.md)

[Assignment]: ./media/tutorial-sspr-windows/profile-assignment.png "Atribuir a política de configuração de dispositivos do Intune a um grupo de dispositivos Windows 10"
[LoginScreen]: ./media/tutorial-sspr-windows/logon-reset-password.png "Ligação Repor palavra-passe no ecrã de início de sessão do Windows 10"
