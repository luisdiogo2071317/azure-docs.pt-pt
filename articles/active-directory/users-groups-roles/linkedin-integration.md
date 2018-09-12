---
title: Ativar a integração de ligações do LinkedIn no Azure Active Directory | Documentos da Microsoft
description: Explica como ativar ou desativar ligações de conta do LinkedIn para aplicações da Microsoft no Azure Active Directory
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 09/11/2018
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.openlocfilehash: 0eaa2656313ecd9b64503051265dc16285f0a1f3
ms.sourcegitcommit: 794bfae2ae34263772d1f214a5a62ac29dcec3d2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44392271"
---
# <a name="linkedin-account-connections"></a>Ligações de conta do LinkedIn

Neste artigo, pode saber como ativar ou desativar ligações de conta do LinkedIn para o seu inquilino no Centro de administração do Azure Active Directory (Azure AD).

> [!IMPORTANT]
> As ligações de conta do LinkedIn definição é implementado para inquilinos do Azure AD. Quando ele é implementado com o seu inquilino, ele é habilitado por padrão. 
> 
> Exceções:
> * A definição não está disponível para clientes que utilizam o Microsoft Cloud para administração pública dos EUA, o Microsoft Cloud Alemanha, ou o Azure e o Office 365, operado pela 21Vianet na China.
> * A definição está desativada por predefinição para os inquilinos aprovisionados na Alemanha. Tenha em atenção que a definição não está disponível para clientes que utilizam o Microsoft Cloud Alemanha.
> * A definição está desativada por predefinição para os inquilinos aprovisionados na França.

> A integração funciona somente se tiver habilitado *e* se permitir que os utilizadores consentirem que as aplicações acedam aos dados da empresa em nome deles. Para obter informações sobre a definição de consentimento, consulte [como remover o acesso de um utilizador a uma aplicação](https://docs.microsoft.com/azure/active-directory/application-access-assignment-how-to-remove-assignment).

## <a name="enable-or-disable-linkedin-account-connections-for-your-tenant-in-the-azure-portal"></a>Ativar ou desativar ligações de conta do LinkedIn para o seu inquilino no portal do Azure

Pode ativar ou desativar ligações de conta do LinkedIn para todo o seu inquilino ou para apenas os utilizadores selecionados no seu inquilino.

1. Inicie sessão para o [Centro de administração do Azure Active Directory](https://aad.portal.azure.com/) com uma conta que seja um administrador global do inquilino do Azure AD.
2. Selecione **utilizadores**.
3. Sobre o **usuários** painel, selecione **as definições de utilizador**.
4. Sob **ligações de conta do LinkedIn**:
  * Selecione **Sim** para permitir ligações de conta do LinkedIn para todos os utilizadores no seu inquilino
  * Selecione **selecionados** para ativar o LinkedIn conta ligações para apenas inquilino selecionado utilizadores
  * Selecione **não** para desativar as ligações de conta do LinkedIn para todos os utilizadores ![ligações do LinkedIn ativar conta](./media/linkedin-integration/linkedin-integration.png)
5. Guardar as definições, quando tiver terminado, selecionando **guardar**.

## <a name="enable-or-disable-linkedin-account-connections-for-your-tenant-using-group-policy"></a>Ativar ou desativar ligações de conta do LinkedIn para o seu inquilino através da política de grupo

1. Transferir o [arquivos de modelo de administração do Office 2016 (ADMX/ADML)](https://www.microsoft.com/download/details.aspx?id=49030)
2. Extrair os **ADMX** arquivos e copie-os para o armazenamento central.
3. Gerenciamento de diretiva de grupo de Open.
4. Criar um objeto de política de grupo com a definição seguinte: **configuração do usuário** > **modelos administrativos** > **Microsoft Office 2016**  >  **Diversos** > **funcionalidades do LinkedIn Mostrar em aplicativos do Office**.
5. Selecione **habilitado** ou **desativado**.
  
 Estado | Efeito
------ | ------
**Ativado** | O **funcionalidades do LinkedIn Mostrar em aplicativos do Office** nas opções do Office 2016 está habilitada. Os utilizadores na sua organização podem utilizar funcionalidades do LinkedIn nas aplicações do Office.
 **Desativado** | O **funcionalidades do LinkedIn Mostrar em aplicativos do Office** definição nas opções do Office 2016 está desativada e os utilizadores finais não é possível alterar esta definição. Os utilizadores na sua organização não é possível utilizar funcionalidades do LinkedIn em seus aplicativos do Office 2016.

Esta política de grupo afeta apenas as aplicações do Office 2016 para um computador local. Os utilizadores podem ver as funcionalidades de LinkedIn em cartões de perfil em todo o Office 365, mesmo que elas desativam LinkedIn nas suas aplicações do Office 2016.

## <a name="learn-more"></a>Saiba mais

* [Integrar o LinkedIn na sua organização](linkedin-user-consent.md)

* [Informações do LinkedIn e funcionalidades nas suas aplicações da Microsoft](https://go.microsoft.com/fwlink/?linkid=850740)

* [Centro de ajuda do LinkedIn](https://www.linkedin.com/help/linkedin)

## <a name="next-steps"></a>Passos Seguintes
Utilize a seguinte ligação para ver as ligações de conta atuais do LinkedIn definição no portal do Azure:

[Configurar ligações de conta do LinkedIn](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings) 