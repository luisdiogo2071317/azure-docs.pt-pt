---
title: "Azure Active Directory dos serviços de domínio: Configuração do Principal de serviço de resolução de problemas | Microsoft Docs"
description: "Resolver problemas de configuração do Principal de serviço para os serviços de domínio do Azure AD"
services: active-directory-ds
documentationcenter: 
author: eringreenlee
manager: 
editor: 
ms.assetid: f168870c-b43a-4dd6-a13f-5cfadc5edf2c
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2018
ms.author: ergreenl
ms.openlocfilehash: e6144a4018f7fbe7dbf7b4693e3f41884e4a80a2
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-domain-services---troubleshooting-service-principal-configuration"></a>Serviços de domínio do Azure AD - configuração do Principal de serviço de resolução de problemas

Para o serviço, gerir e atualizar o seu domínio, a Microsoft utiliza vários [principais de serviço](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-application-objects) para comunicar com o seu diretório. Se um está configurado incorretamente ou eliminado, pode causar uma interrupção no seu serviço.

## <a name="aadds102-service-principal-not-found"></a>AADDS102: Principal de serviço não foi encontrado

**Mensagem de alerta:**

*Um Principal de serviço necessária para os serviços de domínio do Azure AD para funcionarem corretamente foi eliminado do diretório do Azure AD. Esta configuração afeta a capacidade da Microsoft para monitorizar, gerir, patches e sincronizar o seu domínio gerido.*

Principais de serviço são aplicações de que a Microsoft utiliza para gerir, atualizar e manter o seu domínio gerido. Se estes sejam eliminados, interrompe a capacidade da Microsoft para atender o seu domínio. Utilize os seguintes passos para determinar qual serviço principais têm de ser recriadas.

1. Navegue para o [aplicações da empresa - todas as aplicações](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps) página no portal do Azure.
2. Utilizar a lista Mostrar pendente, selecione **todas as aplicações** e clique em **aplicar**.
3. Pesquisa para cada ID de aplicação, colando o ID na caixa de pesquisa e premindo utilizando a seguinte tabela, introduza. Se os resultados da pesquisa estiverem vazios, tem de recriar o principal de serviço, seguindo os passos na coluna "Resolução".

| ID da aplicação | Resolução |
| :--- | :--- | :--- |
| 2565bd9d-da50-47d4-8b85-4c97f669dc36 | [A recriar um Principal de serviço em falta com o PowerShell](#recreate-a-missing-service-principal-with-powershell) |
| 443155a6-77f3-45e3-882b-22b3a8d431fb | [Volte a registar para o espaço de nomes Microsoft.AAD](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| abba844e-bc0e-44b0-947a-dc74e5d09022  | [Volte a registar para o espaço de nomes Microsoft.AAD](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| d87dcbc6-a371-462e-88e3-28ad15ec4e64 | [Principais de serviço próprio corrigir](#service-principals-that-self-correct) |

### <a name="recreate-a-missing-service-principal-with-powershell"></a>Recrie um Principal de serviço em falta com o PowerShell

*Siga estes passos em falta IDs: 2565bd9d-da50-47d4-8b85-4c97f669dc36*

**Remediação:**

É necessário o Azure AD PowerShell para concluir estes passos. Para obter informações sobre como instalar o Azure AD PowerShell, consulte [neste artigo](https://docs.microsoft.com/en-us/powershell/azure/active-directory/install-adv2?view=azureadps-2.0.).

Para resolver este problema, escreva os comandos seguintes numa janela do PowerShell:
1. Instalar módulo AzureAD
2. AzureAD Import-Module
3. Verifique se o principal de serviço necessário para os serviços de domínio do Azure AD está em falta no seu diretório executando o seguinte comando do PowerShell:
      ```PowerShell
      Get-AzureAdServicePrincipal -filter "AppId eq '2565bd9d-da50-47d4-8b85-4c97f669dc36'"
      ```
4. Crie o principal de serviço, escrevendo o seguinte comando do PowerShell:
     ```PowerShell
     New-AzureAdServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"
     ```
5. Depois de criar o Principal de serviço em falta, aguarde duas horas e verifique o estado de funcionamento do seu domínio.


### <a name="re-register-to-the-microsoft-aad-namespace-using-the-azure-portal"></a>Volte a registar para o espaço de nomes do Microsoft AAD no portal do Azure

*Siga estes passos em falta IDs: 443155a6-77f3-45e3-882b-22b3a8d431fb e abba844e-bc0e-44b0-947a-dc74e5d09022*

**Remediação:**

Utilize os seguintes passos para restaurar os serviços de domínio no seu diretório:

1. Navegue para o [subscrições](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) página no portal do Azure.
2. Selecione a subscrição da tabela que está associada ao seu domínio gerido
3. Utilizando o painel de navegação da esquerda, escolha **fornecedores de recursos**
4. Procure "Microsoft.AAD" na tabela e clique em **voltar a registar**
5. Para verificar para se certificar de que o alerta foi resolvido, ver a página de alerta de estado de funcionamento nas duas horas.


### <a name="service-principals-that-self-correct"></a>Principais de serviço próprio corrigir

*Siga estes passos em falta IDs: d87dcbc6-a371-462e-88e3-28ad15ec4e64*

**Remediação:**

Microsoft pode identificar quando específico principais de serviço estão em falta, configurado incorretamente ou eliminado. Para resolver o serviço rapidamente, Microsoft irão recriar as principais do serviço. Verifique o estado de funcionamento do seu domínio depois de duas horas para se certificar de que o principal foi recriado.

## <a name="contact-us"></a>Contacte-nos
Contacte a equipa de produto do Azure Active Directory Domain Services para [partilhar comentários ou para suporte](active-directory-ds-contact-us.md).
