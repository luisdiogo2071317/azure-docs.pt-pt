---
title: 'Azure Active Directory Domain Services: Resolução de problemas configuração do Principal de serviço | Documentos da Microsoft'
description: Resolução de problemas de Principal de serviço de configuração para o Azure AD Domain Services
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: ''
editor: ''
ms.assetid: f168870c-b43a-4dd6-a13f-5cfadc5edf2c
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ergreenl
ms.openlocfilehash: 407b9732574880cd64036e92fe0c7fac169b7346
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39503289"
---
# <a name="troubleshoot-invalid-service-principal-configuration-for-your-managed-domain"></a>Resolver problemas de configuração de Principal de serviço inválido para o seu domínio gerido

Este artigo ajuda-o a resolver erros de configuração relacionadas com o principal de serviço que fazer com a seguinte mensagem de alerta:

## <a name="alert-aadds102-service-principal-not-found"></a>Alerta AADDS102: Principal de serviço não foi encontrado

**Mensagem de alerta:** *um Principal de serviço necessárias para o Azure AD Domain Services funcionar corretamente foi eliminado do diretório do Azure AD. Esta configuração afeta a capacidade da Microsoft para monitorizar, gerir, corrigir e sincronizar o domínio gerido.*

[Principais de serviço](../active-directory/develop/active-directory-application-objects.md) são aplicações que a Microsoft utiliza para gerir, atualizar e manter o seu domínio gerido. Se eles forem excluídos, interrompe a capacidade da Microsoft para atender a seu domínio.


## <a name="check-for-missing-service-principals"></a>Procurar em falta principais de serviço
Utilize os seguintes passos para determinar qual serviço tem de ser recriadas principais:

1. Navegue para o [aplicações empresariais - todas as aplicações](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps) página no portal do Azure.
2. Na **mostrar** menu pendente, selecione **todos os aplicativos** e clique em **aplicar**.
3. Pesquisa para cada ID de aplicação, colar o ID na caixa de pesquisa e prima utilizando a tabela seguinte, introduza. Se os resultados da pesquisa estiverem vazios, tem de recriar o principal de serviço ao seguir os passos na coluna "Resolução".

| ID da aplicação | Resolução |
| :--- | :--- | :--- |
| 2565bd9d-da50-47d4-8b85-4c97f669dc36 | [Recriar um principal de serviço em falta com o PowerShell](#recreate-a-missing-service-principal-with-powershell) |
| 443155a6-77f3-45e3-882b-22b3a8d431fb | [Voltar a registar para o espaço de nomes de Microsoft.AAD](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| abba844e-bc0e-44b0-947a-dc74e5d09022  | [Voltar a registar para o espaço de nomes de Microsoft.AAD](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| d87dcbc6-a371-462e-88e3-28ad15ec4e64 | [Principais de serviço que autónoma corrigir](#service-principals-that-self-correct) |

## <a name="recreate-a-missing-service-principal-with-powershell"></a>Recriar um Principal de serviço em falta com o PowerShell
Siga estes passos, se um principal de serviço com o ID de ```2565bd9d-da50-47d4-8b85-4c97f669dc36``` está em falta no diretório do Azure AD.

**Resolução:** terá o Azure AD PowerShell para concluir estes passos. Para obter informações sobre como instalar o Azure AD PowerShell, consulte [este artigo](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0.).

Para resolver este problema, escreva os seguintes comandos numa janela do PowerShell:
1. Instalar o módulo Azure AD PowerShell e importe-o.

    ```powershell
    Install-Module AzureAD
    Import-Module AzureAD
    ```

2. Verifique se o principal de serviço necessário para o Azure AD Domain Services está em falta no seu diretório, executando o seguinte comando do PowerShell:

    ```powershell
    Get-AzureAdServicePrincipal -filter "AppId eq '2565bd9d-da50-47d4-8b85-4c97f669dc36'"
    ```

3. Crie o principal de serviço, escrevendo o seguinte comando do PowerShell:

    ```powershell
    New-AzureAdServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"
    ```

4. Depois de criar o serviço em falta principal, aguarde duas horas e verifique o estado de funcionamento do seu domínio gerido.


## <a name="re-register-to-the-microsoft-aad-namespace-using-the-azure-portal"></a>Voltar a registar para o espaço de nomes do Microsoft AAD com o portal do Azure
Siga estes passos, se um principal de serviço com o ID ```443155a6-77f3-45e3-882b-22b3a8d431fb``` ou ```abba844e-bc0e-44b0-947a-dc74e5d09022``` está em falta no diretório do Azure AD.

**Resolução:** utilize os seguintes passos para restaurar os serviços de domínio no seu diretório:

1. Navegue para o [subscrições](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) página no portal do Azure.
2. Escolha a subscrição da tabela que está associada ao seu domínio gerido
3. Usando o painel de navegação esquerda, escolha **fornecedores de recursos**
4. Procure "Microsoft.AAD" na tabela e clique em **voltar a registar**
5. Para garantir que o alerta é resolvido, ver a página de estado de funcionamento para o seu domínio gerido nas duas horas.


## <a name="service-principals-that-self-correct"></a>Principais de serviço que autónoma corrigir
Siga estes passos, se um principal de serviço com o ID de ```d87dcbc6-a371-462e-88e3-28ad15ec4e64``` está em falta no diretório do Azure AD.

**Resolução:** dos serviços de domínio do Azure AD pode detectar quando este principal de serviço específico está em falta, configurada incorretamente ou eliminado. O serviço recria automaticamente este principal de serviço. No entanto, terá de eliminar a aplicação e o objeto que trabalhou com a aplicação eliminado, assim como quando a certificação passa o tempo limite, a aplicação e o objeto já não poderá ser modificada pelo novo principal de serviço. Isso irá gerar um erro de novo no seu domínio. Siga os passos descritos no [na secção AADDS105](#alert-aadds105-password-synchronization-application-is-out-of-date) para evitar este problema. Depois, verifique o estado de funcionamento do seu domínio gerido depois de duas horas para se certificar de que o novo principal de serviço ter sido recriado.


## <a name="alert-aadds105-password-synchronization-application-is-out-of-date"></a>Alerta AADDS105: A aplicação de sincronização de palavra-passe está desatualizada

**Mensagem de alerta:** o principal de serviço com o ID da aplicação "d87dcbc6-a371-462e-88e3-28ad15ec4e64" foi eliminado e, em seguida, recriado. A recriação de deixa para trás inconsistentes permissões em recursos de serviços de domínio do Azure AD necessários para atender a seu domínio gerido. Sincronização de palavras-passe no seu domínio gerido poderá ser afetada.


**Resolução:** terá o Azure AD PowerShell para concluir estes passos. Para obter informações sobre como instalar o Azure AD PowerShell, consulte [este artigo](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0.).

Para resolver este problema, escreva os seguintes comandos numa janela do PowerShell:
1. Instalar o módulo Azure AD PowerShell e importe-o.

    ```powershell
    Install-Module AzureAD
    Import-Module AzureAD
    ```
2. Eliminar a aplicação antiga e o objeto com os seguintes comandos do PowerShell

    ```powershell
    $app = Get-AzureADApplication -Filter "IdentifierUris eq 'https://sync.aaddc.activedirectory.windowsazure.com'"
    Remove-AzureADApplication -ObjectId $app.ObjectId
    $spObject = Get-AzureADServicePrincipal -Filter "DisplayName eq 'Azure AD Domain Services Sync'"
    Remove-AzureADServicePrincipal -ObjectId $app.ObjectId
    ```
3. Depois de ter eliminado ambos, o sistema irá remediar em si e recrie aos aplicativos necessários para a sincronização de palavra-passe. Para garantir que o alerta foi remediado, aguarde duas horas e verifique o estado de funcionamento do seu domínio.


## <a name="contact-us"></a>Contacte-nos
Contacte a equipa de produto do Azure Active Directory Domain Services para [partilhar comentários ou para o suporte](active-directory-ds-contact-us.md).
