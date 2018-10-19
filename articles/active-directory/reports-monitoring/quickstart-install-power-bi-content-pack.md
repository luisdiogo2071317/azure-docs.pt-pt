---
title: Instalar o pacote de conteúdos do Power BI para o Azure AD | Microsoft Docs
description: Saiba como instalar o pacote de conteúdos do Power BI para o Azure AD
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
ms.assetid: fd5604eb-1334-4bd8-bfb5-41280883e2b5
ms.service: active-directory
ms.workload: identity
ms.component: report-monitor
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/23/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: ba2784663a585aebb82ee149be3b54e73920f6dd
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816655"
---
# <a name="quickstart-install-azure-active-directory-power-bi-content-pack"></a>Início Rápido: instalar o pacote de conteúdos do Power BI para o Azure Active Directory

|  |
|--|
|Atualmente, o pacote de conteúdos do Power BI para o Azure AD utiliza as APIs do Azure AD Graph para obter dados do seu inquilino do Azure AD. Como resultado, poderá observar alguma disparidade entre os dados disponíveis no pacote de conteúdos e os dados obtidos com as [APIs do Microsoft Graph para relatórios](concept-reporting-api.md). |
|  |

O pacote de conteúdos do Power BI para o Azure Active Directory permite obter informações aprofundadas do que se passa no seu diretório ativo. Pode transferir o pacote de conteúdos pré-criado e utilizá-lo para reportar todas as atividades do seu diretório com a avançada experiência de visualização que o Power BI oferece. Também pode criar o seu próprio dashboard e partilhá-lo facilmente com qualquer pessoa da sua organização. 

Neste início rápido, vai aprender a instalar o pacote de conteúdos.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este guia de início rápido, necessita de:

* Uma conta do Power BI. É a mesma conta que a sua conta do O365 ou do Azure AD. 
* O ID do seu inquilino do Azure AD. É o **ID de Diretório** do seu diretório da [página de propriedades](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Properties) do portal do Azure.
* Uma licença do Azure AD Premium (P1/P2). 

## <a name="install-azure-ad-power-bi-content-pack"></a>Instalar o pacote de conteúdos do Power BI para o Azure AD 

1. Inicie sessão no [Power BI](https://app.powerbi.com/groups/me/getdata/services) com a sua Conta do Power BI. É a mesma conta que a sua Conta do O365 ou do Azure AD.

2. Procure **Registos de Atividades do Azure Active Directory** na página **Aplicações** e selecione **Obter agora**. 

   ![Pacote de Conteúdos do Power BI para o Azure Active Directory](./media/quickstart-install-power-bi-content-pack/getitnow.png) 
    
3. Na janela de pop-up, escreva o ID do seu inquilino do Azure AD, introduza **7** como o número de dias a consultar e, em seguida, selecione **Seguinte**.
    
   ![Pacote de Conteúdos do Power BI para o Azure Active Directory](./media/quickstart-install-power-bi-content-pack/connect.png) 

4. Quando o seu dashboard de registos de Atividades do Azure Active Directory estiver criado, selecione-o.

   ![Pacote de Conteúdos do Power BI para o Azure Active Directory](./media/quickstart-install-power-bi-content-pack/dashboard.png) 
    
## <a name="next-steps"></a>Passos seguintes

* [Utilizar o pacote de conteúdos do Power BI](howto-power-bi-content-pack.md).
* [Resolver erros do pacote de conteúdos](troubleshoot-content-pack.md).
