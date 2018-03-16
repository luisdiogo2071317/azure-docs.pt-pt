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
ms.date: 03/12/2018
ms.author: ergreenl
ms.openlocfilehash: e1be075ba2d3e6ae7512ccc030073fd7f1862502
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="troubleshoot-invalid-service-principal-configuration-for-your-managed-domain"></a>Resolver problemas de configuração de Principal de serviço inválida para o seu domínio gerido

Este artigo ajuda-o a resolver erros de configuração relacionados com o principal de serviço que resultam a seguinte mensagem de alerta:

## <a name="alert-aadds102-service-principal-not-found"></a>Alerta AADDS102: Principal de serviço não encontrado

**Mensagem de alerta:** *um Principal de serviço necessária para os serviços de domínio do Azure AD para funcionarem corretamente foi eliminado do diretório do Azure AD. Esta configuração afeta a capacidade da Microsoft para monitorizar, gerir, patches e sincronizar o seu domínio gerido.*

[Principais de serviço](../active-directory/develop/active-directory-application-objects.md) são aplicações de que a Microsoft utiliza para gerir, atualizar e manter o seu domínio gerido. Se estes sejam eliminados, interrompe a capacidade da Microsoft para atender o seu domínio.


## <a name="check-for-missing-service-principals"></a>Verifique a existência de falta de principais de serviço
Utilize os seguintes passos para determinar o serviço tem de ser recriadas principais:

1. Navegue para o [aplicações da empresa - todas as aplicações](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps) página no portal do Azure.
2. No **mostrar** lista pendente, selecione **todas as aplicações** e clique em **aplicar**.
3. Pesquisa para cada ID de aplicação, colando o ID na caixa de pesquisa e premindo utilizando a seguinte tabela, introduza. Se os resultados da pesquisa estiverem vazios, tem de recriar o principal de serviço, seguindo os passos na coluna "Resolução".

| ID da aplicação | Resolução |
| :--- | :--- | :--- |
| 2565bd9d-da50-47d4-8b85-4c97f669dc36 | [Recrie um principal de serviço em falta com o PowerShell](#recreate-a-missing-service-principal-with-powershell) |
| 443155a6-77f3-45e3-882b-22b3a8d431fb | [Volte a registar para o espaço de nomes Microsoft.AAD](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| abba844e-bc0e-44b0-947a-dc74e5d09022  | [Volte a registar para o espaço de nomes Microsoft.AAD](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| d87dcbc6-a371-462e-88e3-28ad15ec4e64 | [Principais de serviço próprio corrigir](#service-principals-that-self-correct) |

## <a name="recreate-a-missing-service-principal-with-powershell"></a>Recrie um Principal de serviço em falta com o PowerShell
Siga estes passos, se um principal de serviço com o ID ```2565bd9d-da50-47d4-8b85-4c97f669dc36``` está em falta o diretório do Azure AD.

**Resolução:** precisa do Azure AD PowerShell para concluir estes passos. Para obter informações sobre como instalar o Azure AD PowerShell, consulte [neste artigo](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0.).

Para resolver este problema, escreva os comandos seguintes numa janela do PowerShell:
1. Instalar o módulo Azure AD PowerShell e importe-o.

    ```powershell
    Install-Module AzureAD
    Import-Module AzureAD
    ```

2. Verifique se o principal de serviço necessário para os serviços de domínio do Azure AD está em falta no seu diretório executando o seguinte comando do PowerShell:

    ```powershell
    Get-AzureAdServicePrincipal -filter "AppId eq '2565bd9d-da50-47d4-8b85-4c97f669dc36'"
    ```

3. Crie o principal de serviço, escrevendo o seguinte comando do PowerShell:

    ```powershell
    New-AzureAdServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"
    ```

4. Depois de criar o serviço em falta principal, aguarde duas horas e verifique o estado de funcionamento do seu domínio gerido.


## <a name="re-register-to-the-microsoft-aad-namespace-using-the-azure-portal"></a>Volte a registar para o espaço de nomes do Microsoft AAD no portal do Azure
Siga estes passos, se um principal de serviço com o ID ```443155a6-77f3-45e3-882b-22b3a8d431fb``` ou ```abba844e-bc0e-44b0-947a-dc74e5d09022``` está em falta o diretório do Azure AD.

**Resolução:** utilize os seguintes passos para restaurar os serviços de domínio no seu diretório:

1. Navegue para o [subscrições](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) página no portal do Azure.
2. Selecione a subscrição da tabela que está associada ao seu domínio gerido
3. Utilizando o painel de navegação da esquerda, escolha **fornecedores de recursos**
4. Procure "Microsoft.AAD" na tabela e clique em **voltar a registar**
5. Para garantir que o alerta é resolvido, visualize a página de estado de funcionamento para o seu domínio gerido nas duas horas.


## <a name="service-principals-that-self-correct"></a>Principais de serviço próprio corrigir
Siga estes passos, se um principal de serviço com o ID ```d87dcbc6-a371-462e-88e3-28ad15ec4e64``` está em falta o diretório do Azure AD.

**Resolução:** dos serviços de domínio do Azure AD pode detetar quando este principal de serviço específica está em falta, configurado incorretamente ou eliminado. O serviço recria automaticamente este principal de serviço. No entanto, terá de eliminar a aplicação e o objeto que trabalhou com a aplicação foi eliminada, como quando faz a certificação ao longo, as aplicações e do objeto deixarão de poder ser modificada pelo principal de serviço novo. Isto irá originar um erro de novo no seu domínio. Siga os passos descritos no [na secção AADDS105](#alert-aadds105-password-synchronization-application-is-out-of-date) para evitar este problema. Depois, verifique o estado de funcionamento do seu domínio gerido passadas duas horas para se certificar de que o novo principal de serviço foi recriado.


## <a name="alert-aadds105-password-synchronization-application-is-out-of-date"></a>Alerta AADDS105: A aplicação de sincronização de palavra-passe está desatualizada

**Mensagem de alerta:** o principal de serviço com o ID de aplicação "d87dcbc6-a371-462e-88e3-28ad15ec4e64" foi eliminado e a Microsoft foi capaz de recriar. Este serviço principal gere outro principal de serviço e uma aplicação que são utilizados para sincronização de palavra-passe. O principal de serviço gerida e a aplicação não estão autorizados sob o principal de serviço criado recentemente e fiquem Desatualizadas quando expira o certificado de sincronização. Isto significa que o principal de serviço criado recentemente será não é possível atualizar as antigas aplicações geridas e sincronização de objetos do AAD será afetada.


**Resolução:** precisa do Azure AD PowerShell para concluir estes passos. Para obter informações sobre como instalar o Azure AD PowerShell, consulte [neste artigo](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0.).

Para resolver este problema, escreva os comandos seguintes numa janela do PowerShell:
1. Instalar o módulo Azure AD PowerShell e importe-o.

    ```powershell
    Install-Module AzureAD
    Import-Module AzureAD
    ```
2. Eliminar a aplicação antiga e o objeto, utilizando os seguintes comandos do PowerShell

    ```powershell
    $app = Get-AzureADApplication -Filter "DisplayName eq 'Azure AD Domain Services Sync'"
    Remove-AzureADApplication -ObjectId $app.ObjectId
    $spObject = Get-AzureADServicePrincipal -Filter "DisplayName eq 'Azure AD Domain Services Sync'"
    Remove-AzureADServicePrincipal -ObjectId $app.ObjectId
    ```
3. Depois de ter eliminado ambos, o sistema irá remediar próprio e recriar as aplicações necessárias para a sincronização de palavra-passe. Para garantir que o alerta foi remediado, aguarde duas horas e verifique o estado de funcionamento do seu domínio.


## <a name="contact-us"></a>Contacte-nos
Contacte a equipa de produto do Azure Active Directory Domain Services para [partilhar comentários ou para suporte](active-directory-ds-contact-us.md).
