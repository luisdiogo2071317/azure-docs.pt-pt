---
title: Ativar as ligações do LinkedIn para aplicações da Microsoft e serviços no Azure Active Directory | Documentos da Microsoft
description: Explica como ativar ou desativar ligações de conta do LinkedIn para aplicações da Microsoft no Azure Active Directory
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 06/28/2018
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.openlocfilehash: 4b3ff0b2481b42f516d28ac17f2616685730b7d5
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37872419"
---
# <a name="linkedin-account-connections-for-microsoft-apps-and-services"></a>Ligações de conta do LinkedIn para serviços e aplicações da Microsoft
Neste artigo, pode saber como gerir ligações de conta do LinkedIn para o seu inquilino no Centro de administração do Azure Active Directory (Azure AD). 

> [!IMPORTANT]
> Funcionalidade de ligações de conta do LinkedIn está atualmente a ser implementada em inquilinos do Azure AD. Quando ele é implementado com o seu inquilino, ele é habilitado por padrão. Não está disponível para clientes do Governo dos Estados Unidos e as organizações com caixas de correio do Exchange Online alojadas na Austrália, Canadá, China, França, Alemanha, Índia, Coreia do Sul, Reino Unido, Japão e África do Sul. Suporte para esses locais de caixa de correio estará disponível brevemente.  Para obter uma exibição atualizada de informações de implementação, consulte a [plano do Office 365](https://products.office.com/business/office-365-roadmap?filters=%26freeformsearch=linkedin#abc) página.

## <a name="benefit-to-users"></a>Benefício para os utilizadores
Depois dos utilizadores ligar a respetiva conta do LinkedIn, informações do LinkedIn são usadas para mostrar informações personalizadas e funcionalidades em vários serviços ou aplicações da Microsoft. Os utilizadores podem ver informações sobre as pessoas que trabalham com o cartão de perfil da Microsoft, mesmo que as pessoas fora da sua organização. Ao longo do tempo, a experiência do LinkedIn também estará mais relevantes e personalizadas para seu trabalho. Por exemplo, o LinkedIn pode sugerir novas ligações com base em que os utilizadores trabalham com ou obter informações sobre as pessoas em seus calendários nesse dia.

## <a name="how-linkedin-account-connections-appear-to-the-user"></a>Como as ligações de conta do LinkedIn são apresentadas ao utilizador
Ligações de conta do LinkedIn permitem aos utilizadores ver informações de perfil do LinkedIn públicas dentro de algumas das suas aplicações da Microsoft. Os utilizadores no seu inquilino podem optar por ligar o seu trabalho LinkedIn e da Microsoft contas escolares ou profissionais para ver informações de perfil do LinkedIn adicionais. Para obter mais informações, consulte [LinkedIn informações e funcionalidades nos serviços e aplicações da Microsoft](https://go.microsoft.com/fwlink/?linkid=850740).

Quando os utilizadores na sua organização ligar o seu trabalho LinkedIn e da Microsoft contas escolares ou profissionais, eles tem duas opções: 
* Conceder permissão para partilhar dados entre ambas as contas. Isso significa que eles conceder permissão para a sua conta LinkedIn para partilhar dados com o trabalho da Microsoft ou conta da instituição de ensino, bem como da Microsoft conta escolar ou profissional para partilhar dados com a respetiva conta do LinkedIn. Dados que são partilhados com o LinkedIn deixam os serviços online. 
* Conceder permissão para partilhar dados apenas a partir de sua conta do LinkedIn com o respetivo trabalho da Microsoft e a conta escolar

Para obter mais informações sobre os dados que são partilhados entre LinkedIn e da Microsoft dos utilizadores de trabalho ou escolares de contas, veja [LinkedIn em aplicativos da Microsoft em sua empresa ou escola](https://www.linkedin.com/help/linkedin/answer/84077). 
* [Os utilizadores podem desligar as contas](https://www.linkedin.com/help/linkedin/answer/85097) e remover dados, permissões de partilha em qualquer altura. 
* [Os utilizadores podem controlar como o seu próprio perfil do LinkedIn é visualizado](https://www.linkedin.com/help/linkedin/answer/83), incluindo se o respetivo perfil pode ser visualizado nas aplicações da Microsoft.

## <a name="privacy-considerations"></a>Considerações de privacidade
Ativar o LinkedIn ligações de conta permite que aplicações da Microsoft e serviços para aceder a algumas das informações do LinkedIn dos seus utilizadores. Leitura a [declaração de privacidade do Microsoft](https://privacy.microsoft.com/privacystatement/) para saber mais sobre as considerações de privacidade ao ativar ligações de conta do LinkedIn no Azure AD. 

## <a name="manage-linkedin-account-connections"></a>Gerir ligações de conta do LinkedIn
Funcionalidade de ligações de conta do LinkedIn está ativada por predefinição para todo o seu inquilino. Pode optar por desativar ligações de conta do LinkedIn para todo o seu inquilino ou ativar as ligações de conta do LinkedIn para utilizadores selecionados no seu inquilino. 

### <a name="enable-or-disable-linkedin-account-connection-for-your-tenant-in-the-azure-portal"></a>Ativar ou desativar a ligação da conta LinkedIn para o seu inquilino no portal do Azure

1. Inicie sessão para o [Centro de administração do Azure Active Directory](https://aad.portal.azure.com/) com uma conta que seja um administrador global do inquilino do Azure AD.
2. Selecione **utilizadores**.
3. Sobre o **usuários** painel, selecione **as definições de utilizador**.
4. Sob **ligações de conta do LinkedIn**:
  * Selecione **Sim** para permitir ligações de conta do LinkedIn para todos os utilizadores no seu inquilino
  * Selecione **selecionados** para ativar o LinkedIn conta ligações para apenas inquilino selecionado utilizadores
  * Selecione **não** para desativar as ligações de conta do LinkedIn para todos os utilizadores ![ligações do LinkedIn ativar conta](./media/linkedin-integration/linkedin-integration.png)
5. Guardar as definições, quando tiver terminado, selecionando **guardar**.

### <a name="enable-or-disable-linkedin-account-connections-for-your-organizations-office-2016-apps-using-group-policy"></a>Ativar ou desativar ligações de conta do LinkedIn para aplicações do Office 2016 da sua organização usando a diretiva de grupo

1. Transferir o [arquivos de modelo de administração do Office 2016 (ADMX/ADML)](https://www.microsoft.com/download/details.aspx?id=49030)
2. Extrair os **ADMX** arquivos e copie-os para o armazenamento central.
3. Gerenciamento de diretiva de grupo de Open.
4. Criar um objeto de política de grupo com a definição seguinte: **configuração do usuário** > **modelos administrativos** > **Microsoft Office 2016**  >  **Diversos** > **funcionalidades do LinkedIn Mostrar em aplicativos do Office**.
5. Selecione **habilitado** ou **desativado**.
  * Quando a política está **Enabled**, o **LinkedIn mostrar funcionalidades nas aplicações do Office** encontrado na caixa de diálogo Opções do Office 2016 está habilitada. Isso também significa que os utilizadores na sua organização podem utilizar as funcionalidades do LinkedIn nas aplicações do Office.
  * Quando a política estiver **desativada**, o **LinkedIn mostrar funcionalidades nas aplicações do Office** definição encontrada nas opções de 2016 do Office a caixa de diálogo está definida como o estado desativado e os utilizadores finais não é possível alterar esta definição. Os utilizadores na sua organização não é possível utilizar funcionalidades do LinkedIn em seus aplicativos do Office 2016.

Esta política de grupo afeta apenas as aplicações do Office 2016 para um computador local. Os utilizadores podem ver as funcionalidades de LinkedIn em cartões de perfil em todo o Office 365, mesmo que elas desativam LinkedIn nas suas aplicações do Office 2016. 

### <a name="learn-more"></a>Saiba mais 
* [Informações do LinkedIn e funcionalidades nas suas aplicações da Microsoft](https://go.microsoft.com/fwlink/?linkid=850740)

* [Centro de ajuda do LinkedIn](https://www.linkedin.com/help/linkedin)

## <a name="next-steps"></a>Passos Seguintes
Utilize a seguinte ligação para ver as ligações de conta atuais do LinkedIn definição no portal do Azure:

[Configurar ligações de conta do LinkedIn](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings) 