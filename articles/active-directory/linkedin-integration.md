---
title: Ativar LinkedIn ligações para aplicações da Microsoft e serviços no Azure Active Directory | Microsoft Docs
description: Explica como ativar ou desativar LinkedIn conta ligações para aplicações da Microsoft no Azure Active Directory
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 03/22/2018
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.openlocfilehash: 33e3305288edc3990ed88b39c819293a8adc2dfe
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="linkedin-account-connections-for-microsoft-apps-and-services"></a>Ligações de conta LinkedIn para serviços e aplicações da Microsoft
Neste artigo, pode saber como gerir LinkedIn ligações de conta para o seu inquilino no Centro de administração do Azure Active Directory (Azure AD). 

> [!IMPORTANT]
> Funcionalidade de ligações do LinkedIn conta está atualmente a ser implementada para inquilinos do Azure AD. Quando é revertida para o seu inquilino, está ativada por predefinição. Não está disponível para clientes de governamentais dos Estados Unidos e organizações com o Exchange Online caixas de correio alojadas da Austrália, Canadá, China, França, Datacenters, Índia, Sul Coreia, Reino Unido, Japão e África do Sul. Suporte para estas localizações de caixa de correio está disponível em breve.  Para uma vista atualizada das informações de implementação, consulte o [plano do Office 365](https://products.office.com/business/office-365-roadmap?filters=%26freeformsearch=linkedin#abc) página.

## <a name="how-linkedin-account-connections-appear-to-the-user"></a>Como o LinkedIn ligações de conta são apresentados ao utilizador
Ligações de conta LinkedIn permitem aos utilizadores ver informações de perfil do LinkedIn públicas dentro de algumas das respetivas aplicações da Microsoft. Os utilizadores no seu inquilino podem optar por ligar o seu trabalho LinkedIn e Microsoft escolar ou profissional contas para ver informações de perfil do LinkedIn adicionais. Para obter mais informações, consulte [LinkedIn informações e funcionalidades nos serviços e aplicações da Microsoft](https://go.microsoft.com/fwlink/?linkid=850740).

Quando os utilizadores na sua organização ligar o seu trabalho LinkedIn e a Microsoft ou escola contas, têm duas opções: 
* Conceda permissão para partilhar dados entre ambas as contas. Isto significa que fornecem permissões para a respetiva conta LinkedIn para partilhar os dados com o respetivo trabalho da Microsoft ou conta profissional, bem como os Microsoft conta escolar ou profissional para partilhar os dados com a respetiva conta LinkedIn. Dados que são partilhados com LinkedIn deixa os serviços online. 
* Conceder permissão para partilhar dados apenas a partir dos respetivos LinkedIn conta com o respetivo trabalho da Microsoft e a conta profissional

Para obter mais informações sobre os dados que são partilhados entre LinkedIn dos utilizadores e a Microsoft de trabalho ou escola contas, consulte [LinkedIn nas aplicações da Microsoft na sua empresa ou escola](https://www.linkedin.com/help/linkedin/answer/84077). 
* [Os utilizadores, podem desligar contas](https://www.linkedin.com/help/linkedin/answer/85097) e remover dados permissões de partilha em qualquer altura. 
* [Os utilizadores podem controlar a forma como é apresentado os seus próprios perfil do LinkedIn](https://www.linkedin.com/help/linkedin/answer/83), incluindo se o respetivo perfil pode ser visualizado no aplicações da Microsoft.

## <a name="privacy-considerations"></a>Considerações de privacidade
Ativar LinkedIn ligações conta permite que aplicações da Microsoft e serviços para aceder a algumas das informações de LinkedIn dos seus utilizadores. Leia o [declaração de privacidade do Microsoft](https://privacy.microsoft.com/privacystatement/) para saber mais sobre as considerações de privacidade quando a ativação de ligações de conta do LinkedIn no Azure AD. 

## <a name="manage-linkedin-account-connections"></a>Gerir ligações de conta LinkedIn
Funcionalidade de ligações do LinkedIn conta está ativada por predefinição para o seu inquilino de todo. Pode optar por desativar LinkedIn ligações de conta para o seu inquilino de todo ou ativar as ligações de conta do LinkedIn para utilizadores selecionados no seu inquilino. 

### <a name="enable-or-disable-linkedin-account-connection-for-your-tenant-in-the-azure-portal"></a>Ativar ou desativar a ligação de conta LinkedIn para o seu inquilino no portal do Azure

1. Iniciar sessão para o [Centro de administração do Azure Active Directory](https://aad.portal.azure.com/) com uma conta que seja um administrador global para o inquilino do Azure AD.
2. Selecione **utilizadores**.
3. No **utilizadores** painel, selecione **as definições de utilizador**.
4. Em **ligações de conta LinkedIn**:
  * Selecione **Sim** para ativar as ligações de conta do LinkedIn para todos os utilizadores no seu inquilino
  * Selecione **selecionados** para ativar o LinkedIn conta ligações para apenas inquilino selecionado utilizadores
  * Selecione **não** desativar LinkedIn ligações de conta para todos os utilizadores ![LinkedIn ativar ligações de conta](./media/linkedin-integration/LinkedIn-integration.png)
5. Guardar as definições, quando tiver terminado, selecionando **guardar**.

### <a name="enable-or-disable-linkedin-account-connections-for-your-organizations-office-2016-apps-using-group-policy"></a>Ativar ou desativar LinkedIn conta ligações para aplicações do Office 2016 da sua organização através da política de grupo

1. Transferir o [ficheiros de modelo de administração do Office 2016 (ADMX/ADML)](https://www.microsoft.com/download/details.aspx?id=49030)
2. Extrair o **ADMX** ficheiros e copie-os para o arquivo central.
3. Gestão de políticas de grupo de abertura.
4. Criar um objeto de política de grupo com a seguinte definição: **configuração do utilizador** > **modelos administrativos** > **Microsoft Office 2016**  >  **Diversos** > **Mostrar LinkedIn funcionalidades nas aplicações do Office**.
5. Selecione **ativada** ou **desativado**.
  * Quando a política é **ativado**, a **Mostrar LinkedIn funcionalidades nas aplicações do Office** definição localizada na caixa de diálogo Opções do Office 2016 está ativada. Isto também significa que os utilizadores na sua organização podem utilizar funcionalidades de LinkedIn nas aplicações do Office.
  * Quando a política é **desativada**, a **Mostrar LinkedIn funcionalidades nas aplicações do Office** encontrada nas opções do 2016 do Office caixa de diálogo está definida para o estado desativado e os utilizadores finais não é possível alterar esta definição. Os utilizadores na sua organização não é possível utilizar funcionalidades LinkedIn nas respetivas aplicações do Office 2016. 

Esta política de grupo afeta apenas as aplicações do Office 2016 para um computador local. Os utilizadores podem ver LinkedIn funcionalidades em cartões de perfil ao longo do Office 365, mesmo que elas desativam LinkedIn nas suas aplicações do Office 2016. 

### <a name="learn-more"></a>Saiba mais 
* [Informações de LinkedIn e funcionalidades nas suas aplicações da Microsoft](https://go.microsoft.com/fwlink/?linkid=850740)

* [Centro de ajuda do LinkedIn](https://www.linkedin.com/help/linkedin)

## <a name="next-steps"></a>Passos Seguintes
Utilize a seguinte hiperligação para ver as suas ligações de conta LinkedIn atuais no portal do Azure:

[Configurar as ligações de conta LinkedIn](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings) 