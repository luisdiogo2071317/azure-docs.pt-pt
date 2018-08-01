---
title: Utilizar a API de Relatórios do Azure AD com certificados para obter dados | Microsoft Docs
description: Explica como utilizar a API de Relatórios do Azure AD com credenciais de certificados para obter dados de diretórios sem intervenção do utilizador.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: compliance-reports
ms.date: 05/07/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 44ec19721ba59898915f6547231fb586cb44eb30
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/01/2018
ms.locfileid: "39389757"
---
# <a name="get-data-using-the-azure-active-directory-reporting-api-with-certificates"></a>Obter dados com a API de relatórios do Azure Active Directory com certificados

As [APIs de relatórios Azure Active Directory (Azure AD)](active-directory-reporting-api-getting-started-azure-portal.md) proporcionam acesso programático aos dados através de um conjunto de APIs baseadas em REST. Pode chamar estas APIs a partir de várias linguagens e ferramentas de programação. Se desejar acessar a API de relatórios de AD do Azure sem intervenção do utilizador, pode configurar o acesso ao utilizar certificados.

Isso envolve os seguintes passos:

1. [Instale os pré-requisitos](#install-prerequisites)
2. [Registre-se o certificado na sua aplicação](#register-the-certificate-in-your-app)
3. [Obter um acesso de token para MS Graph API](#get-an-access-token-for-ms-graph-api)
4. [Consultar os pontos de extremidade do MS Graph API](#query-the-ms-graph-api-endpoints)


## <a name="install-prerequisites"></a>Pré-requisitos da instalação

1. Primeiro, certifique-se de que concluiu o [pré-requisitos para aceder ao Azure Active Directory reporting API](active-directory-reporting-api-prerequisites-azure-portal.md). 

2. Baixe e instale o Azure AD Powershell V2, seguir as instruções em [Azure Active Directory PowerShell](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure AD Cmdlets/AzureAD/index.md)

3. Instalar o MSCloudIDUtils do [PowerShellGallery - MSCloudIdUtils](https://www.powershellgallery.com/packages/MSCloudIdUtils/). Este módulo disponibiliza vários cmdlets de utilitário, incluindo:
    - As bibliotecas ADAL necessárias à autenticação
    - Tokens de acesso de utilizador, chaves de aplicação e certificados, através da ADAL
    - Graph API que processa resultados paginados

4. Se for a primeira vez utilizando o módulo executar **Install-MSCloudIdUtilsModule** para concluir a configuração, caso contrário, pode simplesmente importá-lo utilizando o **Import-Module** comando do Powershell.

A sua sessão deve ter um aspeto semelhante a este ecrã:

  ![Windows Powershell](./media/active-directory-reporting-api-with-certificates/module-install.png)

## <a name="register-the-certificate-in-your-app"></a>Registre-se o certificado na sua aplicação

1. Em primeiro lugar, aceda à página de registo da aplicação. Pode fazê-lo ao navegar para o [portal do Azure](https://portal.azure.com), clicando em **Azure Active Directory**, em seguida, clicar em **registos das aplicações** e escolher a sua aplicação na lista. 

2. Em seguida, siga os passos para [registar o certificado com o Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-certificate-credentials#register-your-certificate-with-azure-ad) para a aplicação. 

3. Tenha em atenção que o ID da aplicação e o thumbprint do certificado que acabou de registar com a sua aplicação. Para encontrar o thumbprint, a partir da sua página de aplicativo no portal, aceda a **configurações** e clique em **chaves**. O thumbprint será sob o **chaves públicas** lista.

  
## <a name="get-an-access-token-for-ms-graph-api"></a>Obter um acesso de token para MS Graph API

Para obter um acesso token para a Graph API do MS, utilize o **Get-MSCloudIdMSGraphAccessTokenFromCert** cmdlet do módulo do PowerShell de MSCloudIdUtils. 

>[!NOTE]
>Tem de utilizar o ID da aplicação (também conhecido como ClientID) e o thumbprint do certificado do certificado com a chave privada instalada no arquivo de certificados do computador (arquivo de certificados CurrentUser ou LocalMachine).
>

 ![Portal do Azure](./media/active-directory-reporting-api-with-certificates/getaccesstoken.png)

## <a name="use-the-access-token-to-call-the-graph-api"></a>Utilizar o token de acesso para chamar a Graph API

Agora, pode utilizar o token de acesso no seu script de Powershell para consultar a Graph API. Abaixo está um exemplo usando o **Invoke-MSCloudIdMSGraphQuery** cmdlet a partir de MSCloudIDUtils enumerar o ponto final signins ou diectoryAudits. Este cmdlet processa resultados paginados múltiplos e, em seguida, envia-os para o pipeline do PowerShell.

### <a name="query-the-directoryaudits-endpoint"></a>Consultar o ponto de extremidade DirectoryAudits
 ![Portal do Azure](./media/active-directory-reporting-api-with-certificates/query-directoryAudits.png)

 ### <a name="query-the-signins-endpoint"></a>Consultar o ponto de extremidade SignIns
 ![Portal do Azure](./media/active-directory-reporting-api-with-certificates/query-signins.png)

Agora, pode optar por exportar estes dados para um CSV e guardar num sistema SIEM. Também pode encapsular o script numa tarefa agendada para obter dados do Azure AD a partir do seu inquilino periodicamente, sem ter de armazenar chaves de aplicação no código de origem. 


## <a name="next-steps"></a>Passos Seguintes

* [Obter uma primeira impressão das APIs de relatórios](active-directory-reporting-api-getting-started-azure-portal.md)
* [Referência da API de auditoria](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [Relatório de atividade de início de sessão de referência da API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)



