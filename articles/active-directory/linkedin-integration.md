---
title: "Ativar ou desativar a integração do LinkedIn para aplicações do Office no Azure Active Directory | Microsoft Docs"
description: "Explica como ativar ou desativar a integração do LinkedIn para aplicações da Microsoft no Azure Active Directory"
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 01/30/2018
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.openlocfilehash: 5ebc44d0ef6200baeacf4f1f8c4371e2d1eed9db
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="linkedin-integration-for-office-applications"></a>Integração do LinkedIn para aplicações do Office
Este artigo mostra como restringir os utilizadores a quem LinkedIn integração é fornecida no Azure Active Directory (Azure AD). Integração de LinkedIn está ativada por predefinição, quando é adicionado ao seu inquilino, o que permite aos utilizadores aceder a dados públicos de LinkedIn dentro de algumas das respetivas aplicações da Microsoft. Cada utilizador independentemente pode optar por ligar a respetiva conta escolar ou profissional para a respetiva conta LinkedIn.

> [!IMPORTANT]
> Integração de LinkedIn não está a ser implementada para todos os inquilinos do Azure AD ao mesmo tempo. Depois de ser distribuída para o seu inquilino do Azure, LinkedIn integração está ativada por predefinição. Integração de LinkedIn não está disponível para aceda local, sovereign e inquilinos government. 

## <a name="linkedin-integration-from-the-user-perspective"></a>Integração do LinkedIn da perspetiva do utilizador
Quando os utilizadores na sua organização ligam a respetiva conta LinkedIn com o respetivo trabalho conta escolar ou profissional, [permitem LinkedIn fornecer dados](https://www.linkedin.com/help/linkedin/answer/84077) a ser utilizado em aplicações da Microsoft e serviços que fornece a sua organização. [Os utilizadores, podem desligar contas](https://www.linkedin.com/help/linkedin/answer/85097), que remova a permissão para LinkedIn partilhar dados com a Microsoft. Integração de LinkedIn utiliza informações de perfil do LinkedIn publicamente disponíveis. [Os utilizadores podem controlar a forma como é apresentado os seus próprios perfil do LinkedIn](https://www.linkedin.com/help/linkedin/answer/83) utilizando as definições de privacidade LinkedIn, incluindo o facto do respetivo perfil pode ser visualizado no aplicações da Microsoft.

## <a name="privacy-considerations"></a>Considerações de privacidade
Ativar a integração do LinkedIn no Azure AD permite que aplicações da Microsoft e serviços para aceder a algumas das informações de LinkedIn dos seus utilizadores. Leia o [declaração de privacidade do Microsoft](https://privacy.microsoft.com/privacystatement/) para saber mais sobre as considerações de privacidade ao ativar a integração do LinkedIn no Azure AD. 

## <a name="manage-linkedin-integration"></a>Gerir a integração do LinkedIn
Integração de LinkedIn para empresas está ativada por predefinição no Azure AD. Ativar a integração do LinkedIn permite que todos os utilizadores na sua organização utilizar funcionalidades de LinkedIn dentro de serviços Microsoft, como visualizar LinkedIn perfis no Outlook. Desativar a integração do LinkedIn remove LinkedIn funcionalidades de serviços e aplicações da Microsoft e deixa de partilha de dados entre LinkedIn e a sua organização através de serviços Microsoft.

### <a name="enable-or-disable-linkedin-integration-for-your-organization-in-the-azure-portal"></a>Ativar ou desativar a integração do LinkedIn da sua organização no portal do Azure

1. Iniciar sessão para o [Centro de administração do Azure Active Directory](https://aad.portal.azure.com/) com uma conta que seja um administrador global para o inquilino do Azure AD.
2. Selecione **utilizadores e grupos**.
3. No **utilizadores e grupos** painel, selecione **as definições de utilizador**.
4. Em **LinkedIn integração**, selecione **Sim** ou **não** para ativar ou desativar a integração do LinkedIn.
   ![Ativar a integração do LinkedIn](./media/linkedin-integration/LinkedIn-integration.PNG)

### <a name="enable-or-disable-linkedin-integration-for-your-organizations-office-2016-apps-using-group-policy"></a>Ativar ou desativar a integração do LinkedIn para aplicações do Office 2016 da sua organização através da política de grupo

1. Transferir o [ficheiros de modelo de administração do Office 2016 (ADMX/ADML)](https://www.microsoft.com/download/details.aspx?id=49030)
2. Extrair o **ADMX** ficheiros e copie-os para o **repositório central**.
3. Gestão de políticas de grupo de abertura.
4. Criar um objeto de política de grupo com a seguinte definição: **configuração do utilizador** > **modelos administrativos** > **Microsoft Office 2016**  >  **Diversas** > **permitir a integração do LinkedIn**.
5. Selecione **ativada** ou **desativado**.
  * Quando a política é **ativado**, a **Mostrar LinkedIn funcionalidades nas aplicações do Office** definição localizada na caixa de diálogo Opções do Office 2016 está ativada. Isto também significa que os utilizadores na sua organização podem utilizar funcionalidades de LinkedIn nas aplicações do Office.
  * Quando a política é **desativada**, a **Mostrar LinkedIn funcionalidades nas aplicações do Office** encontrada nas opções do 2016 do Office caixa de diálogo está definida para o estado desativado e os utilizadores finais não é possível alterar esta definição. Os utilizadores na sua organização não é possível utilizar funcionalidades LinkedIn nas respetivas aplicações do Office 2016. 

Esta política de grupo afeta apenas as aplicações do Office 2016 para um computador local. Os utilizadores podem ver LinkedIn funcionalidades em cartões de perfil ao longo do Office 365, mesmo que elas desativam LinkedIn nas suas aplicações do Office 2016. 

### <a name="learn-more"></a>Saiba mais 
* [Informações de LinkedIn e funcionalidades nas suas aplicações da Microsoft](https://go.microsoft.com/fwlink/?linkid=850740)

* [Centro de ajuda do LinkedIn](https://www.linkedin.com/help/linkedin)

## <a name="next-steps"></a>Passos Seguintes
Utilize a seguinte hiperligação para ver a definição de integração LinkedIn atual no portal do Azure:

[Configurar a integração do LinkedIn](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings) 