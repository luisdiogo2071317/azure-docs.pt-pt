---
title: Permitir ou bloquear convites aos utilizadores B2B de organizações específicas - Azure Active Directory | Microsoft Docs
description: Mostra como um administrador pode utilizar o portal do Azure ou o PowerShell para definir um acesso ou negar lista para permitir ou bloquear os utilizadores de B2B de determinados domínios.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 04/19/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 3b4b57dd2299c6278fe823f59a4f2c7d8721f712
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="allow-or-block-invitations-to-b2b-users-from-specific-organizations"></a>Permitir ou bloquear convites aos utilizadores B2B de organizações específicas

Pode utilizar uma lista de permissões ou uma lista de negações para ou de bloqueios convites aos utilizadores B2B de organizações específicas. Por exemplo, se pretender bloquear domínios de endereço de e-mail pessoal, pode configurar uma lista de negações contém domínios como Gmail.com e Outlook.com. Ou, se a sua empresa tem uma parceria com outras empresas como Contoso.com, Fabrikam.com e Litware.com e quiser restringir convites para apenas destas organizações, pode adicionar Contoso.com, Fabrikam.com e Litware.com à sua lista de permissões.
  
## <a name="important-considerations"></a>Considerações importantes sobre o

- Pode criar uma lista de permissões ou uma lista de negações. Não é possível configurar ambos os tipos de listas. Por predefinição, qualquer domínios não estão na lista de permissões estão na lista de negações e vice-versa. 
- Pode criar apenas uma política por organização. Pode atualizar a política de modo a incluir mais domínios ou, pode eliminar a política para criar um novo. 
- Esta lista funciona independentemente do OneDrive para empresas e ao SharePoint Online listas de permitir/bloquear. Se pretender restringir a partilha no SharePoint Online de ficheiros individuais, terá de configurar um permitir ou negar lista para o OneDrive para empresas e o SharePoint Online. Para obter mais informações, consulte [restrito domínios de partilha no SharePoint Online e OneDrive para empresas](https://support.office.com/article/restricted-domains-sharing-in-sharepoint-online-and-onedrive-for-business-5d7589cd-0997-4a00-a2ba-2320ec49c4e9).
- Esta lista não é aplicável a utilizadores externos que já tenham resgatada o convite. A lista será imposta depois de configurar a lista. Se um convite do utilizador está no estado pendente e definir uma política que bloqueia o seu domínio, tentar o utilizador resgatar o convite irá falhar.

## <a name="set-the-allow-or-deny-list-policy-in-the-portal"></a>Conjunto de permissões ou negar a política de lista no portal do

Por predefinição, o **permitir convites para serem enviadas a qualquer domínio (mais inclusivo)** definição está ativada. Neste caso, pode convidar os utilizadores de B2B a partir de qualquer organização.

### <a name="add-a-deny-list"></a>Adicione uma lista de negações

Este é o cenário mais comum, onde a organização pretende trabalhar com praticamente qualquer organização, mas advogados pretende impedir que os utilizadores de domínios específicos para ser convidados como utilizadores de B2B.

Para adicionar uma lista de negações:

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione **do Azure Active Directory** > **utilizadores** > **as definições de utilizador**.
3. Em **utilizadores externos**, selecione **gerir definições de colaboração externa**.
4. Em **restrições de colaboração**, selecione **negar convites para os domínios especificados**.
5. Em **domínios de destino**, introduza o nome de um dos domínios que pretende bloquear. Para vários domínios, introduza cada domínio de uma linha nova. Por exemplo:

   ![Mostra a opção de negar com domínios adicionados](./media/allow-deny-list/DenyListSettings.png)
 
6. Quando tiver terminado, clique em **guardar**.

Depois de definir a política, se tentar convidar um utilizador de domínio bloqueado, receberá uma mensagem a indicar que o domínio do utilizador está atualmente bloqueado pela política convite.
 
### <a name="add-an-allow-list"></a>Adicionar uma lista de permissões

Esta é uma configuração mais restritiva, onde pode definir domínios específicos na lista de permissões e restringir convites para quaisquer outras organizações ou em domínios que não são mencionados. 

Se pretender utilizar uma lista de permissões, certifique-se de que demora tempo totalmente avaliar o que as necessidades da empresa são. Se fizer esta política demasiado restritivo, os utilizadores podem optar por enviar documentos através de e-mail ou encontrar outras formas de aprovadas não-IT de colaborar.


Para adicionar uma lista de permissões:

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione **do Azure Active Directory** > **utilizadores** > **as definições de utilizador**.
3. Em **utilizadores externos**, selecione **gerir definições de colaboração externa**.
4. Em **restrições de colaboração**, selecione **permitir convites para apenas para os domínios especificados (mais restritivas)**.
5. Em **domínios de destino**, introduza o nome de um dos domínios que pretende permitir. Para vários domínios, introduza cada domínio de uma linha nova. Por exemplo:

   ![Mostra a opção de permitir com domínios adicionados](./media/allow-deny-list/AllowListSettings.png)
 
6. Quando tiver terminado, clique em **guardar**.

Depois de definir a política, se tentar convidar um utilizador de domínio que não está na lista de permissões, receberá uma mensagem a indicar que o domínio do utilizador está atualmente bloqueado pela política convite.

### <a name="switch-from-allow-to-deny-list-and-vice-versa"></a>Comutador de permitir para negar lista e vice-versa se 

Se mudar de uma política para outro, este rejeita a configuração da política existente. Certifique-se efetuar cópias de segurança detalhes da sua configuração antes de executar o comutador. 

## <a name="set-the-allow-or-deny-list-policy-using-powershell"></a>Conjunto de permissões ou negar a política de lista com o PowerShell

### <a name="prerequisite"></a>Pré-requisito

Para definir o permitir ou negar lista utilizando o PowerShell, tem de instalar a versão de pré-visualização do módulo Azure Active Directory para Windows PowerShell. Especificamente, instale a versão do módulo 2.0.0.98 de AzureADPreview ou posterior.

Para verificar a versão do módulo (e ver se está instalado):
 
1. Abra o Windows PowerShell como um utilizador elevados (executar como administrador). 
2. Execute o seguinte comando para ver se tiver quaisquer versões do módulo Azure Active Directory para Windows PowerShell instalada no seu computador:

   ````powershell  
   Get-Module -ListAvailable AzureAD*
   ````

Se o módulo não está instalado ou não tem uma versão necessária, efetue um dos seguintes:

- Se não existem resultados são devolvidos, execute o seguinte comando para instalar a versão mais recente do módulo AzureADPreview:
  
   ````powershell  
   Install-Module AzureADPreview
   ````
- Se apenas o módulo de AzureAD é apresentado nos resultados, execute os seguintes comandos para instalar o módulo de AzureADPreview: 

   ````powershell 
   Uninstall-Module AzureAD 
   Install-Module AzureADPreview 
   ````
- Se apenas o módulo de AzureADPreview é apresentado nos resultados, mas a versão é inferior a 2.0.0.98, execute os seguintes comandos para atualizá-lo: 

   ````powershell 
   Uninstall-Module AzureADPreview 
   Install-Module AzureADPreview 
   ````

- Se o AzureAD e AzureADPreview módulos são apresentados nos resultados, mas a versão do módulo AzureADPreview é inferior a 2.0.0.98, execute os seguintes comandos para atualizá-lo: 

   ````powershell 
   Uninstall-Module AzureAD 
   Uninstall-Module AzureADPreview 
   Install-Module AzureADPreview 
    ````

### <a name="use-the-azureadpolicy-cmdlets-to-configure-the-policy"></a>Utilizar os cmdlets de AzureADPolicy para configurar a política

Para criar um permitir ou negar lista, utilize o [New-AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/new-azureadpolicy?view=azureadps-2.0-preview) cmdlet. O exemplo seguinte mostra como definir uma lista de negações que bloqueia o domínio de "live.com".

````powershell 
$policyValue = @("{`"B2BManagementPolicy`":{`"InvitationsAllowedAndBlockedDomainsPolicy`":{`"AllowedDomains`": [],`"BlockedDomains`": [`"live.com`"]}}}")

New-AzureADPolicy -Definition $policyValue -DisplayName B2BManagementPolicy -Type B2BManagementPolicy -IsOrganizationDefault $true 
````

O seguinte mostra o mesmo exemplo, mas com o inline de definição de política.

````powershell  
New-AzureADPolicy -Definition @("{`"B2BManagementPolicy`":{`"InvitationsAllowedAndBlockedDomainsPolicy`":{`"AllowedDomains`": [],`"BlockedDomains`": [`"live.com`"]}}}") -DisplayName B2BManagementPolicy -Type B2BManagementPolicy -IsOrganizationDefault $true 
````

Para definir o permitir ou negar a política de lista, utilize o [conjunto AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/set-azureadpolicy?view=azureadps-2.0-preview) cmdlet. Por exemplo:

````powershell   
Set-AzureADPolicy -Definition $policyValue -Id $currentpolicy.Id 
````

Para obter a política, utilize o [Get-AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/get-azureadpolicy?view=azureadps-2.0-preview) cmdlet. Por exemplo:

````powershell
$currentpolicy = Get-AzureADPolicy | ?{$_.Type -eq 'B2BManagementPolicy'} | select -First 1 
````

Para remover a política, utilize o [remover AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/remove-azureadpolicy?view=azureadps-2.0-preview) cmdlet. Por exemplo:

````powershell
Remove-AzureADPolicy -Id $currentpolicy.Id 
````

## <a name="next-steps"></a>Passos Seguintes

- Para uma descrição geral do Azure AD B2B, consulte [o que é a colaboração B2B do Azure AD?](what-is-b2b.md)
- Para obter informações sobre o acesso condicional e de colaboração B2B, consulte [acesso condicional para os utilizadores de colaboração do B2B](conditional-access.md).



