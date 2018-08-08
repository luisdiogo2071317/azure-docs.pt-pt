---
title: Desativar o registo convergido para SSPR do Azure AD e MFA (pré-visualização pública)
description: Desativar o AD multi-factor Authentication do Azure e o registo (pré-visualização pública) de reposição de palavra-passe self-service
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 08/02/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry, michmcla
ms.openlocfilehash: 3ce08f67f001a7c43602627b9eeda3ad60f867c1
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39623299"
---
# <a name="disable-azure-ad-converged-registration-public-preview"></a>Registo (pré-visualização pública) de convergência de desativação do Azure AD

Quando um utilizador regista o número de telefone e/ou a aplicação móvel no novo convergido experiência, nossa carimbos de data / serviço um conjunto de sinalizadores (StrongAuthenticationMethods) para esses métodos em que o utilizador. Esta funcionalidade permite ao utilizador executar o Azure multi-factor Authentication (MFA) com os métodos sempre que a MFA é necessária.

Os métodos que os utilizadores registem por meio da nova experiência de tem a propriedade de StrongAuthenticationMethods definida. Esse comportamento também irá ocorrer depois de pré-visualização pública está disponível. Se um administrador ativa a pré-visualização, os utilizadores registem através da experiência nova e, em seguida, o administrador desative a pré-visualização, os utilizadores podem inconscientemente ser registados para MFA também.

Se um utilizador que foi concluída convergido registo navega para a página de registo SSPR atual, em [ https://aka.ms/ssprsetup ](https://aka.ms/ssprsetup), eles serão solicitados a executar a MFA antes de poderem aceder nessa página. Este passo é um comportamento esperado do ponto de vista técnico, mas para os utilizadores que foram anteriormente registados para SSPR apenas, este passo é um novo comportamento. Embora essa etapa adicional melhorar a postura de segurança do usuário, fornecendo um nível adicional de segurança, os administradores querem podem revertê-lo aos utilizadores, para que eles já não são capazes de realização de MFA.  

## <a name="how-to-roll-back-users"></a>Como reverte os utilizadores

Se, como um administrador pretende repor as definições da MFA do utilizador, criamos um script do PowerShell que limpará a propriedade StrongAuthenticationMethods para aplicação móvel de um utilizador e/ou número de telefone. Executar este script para os seus utilizadores significa que o utilizador tem voltar a registar para MFA se for necessário. Recomendamos que teste a reversão com um ou dois utilizadores antes de reverter a todos os utilizadores afetados.

Os passos que se seguem irão ajudá-lo a reverter um utilizador ou grupo de utilizadores:

### <a name="pre-requisites"></a>Pré-requisitos

1. Terá de instalar os módulos do Azure AD PowerShell apropriados. Numa janela do PowerShell, execute estes comandos para instalar os módulos:

   ```powershell
   Install-Module -Name MSOnline
   Import-Module MSOnline
   ```

1. Guarde a lista de ID/IDs de objeto de utilizador afectado no seu computador como um ficheiro de texto com o ID de um por linha. Tome nota da localização do ficheiro.
1. Guarde o seguinte script no seu computador e tome nota da localização do script:

```powershell
<# 
//********************************************************
//*                                                      *
//*   Copyright (C) Microsoft. All rights reserved.      *
//*                                                      *
//********************************************************
#>

param($path)

# Define Remediation Fn
function RemediateUser {

    param  
    (
        $ObjectId
    )

    $user = Get-MsolUser -ObjectId $ObjectId

    Write-Host "Checking if user is eligible for rollback: UPN: "  $user.UserPrincipalName  " ObjectId: "  $user.ObjectId -ForegroundColor Yellow

    $hasMfaRelyingParty = $false
    foreach($p in $user.StrongAuthenticationRequirements)
    {
        if ($p.RelyingParty -eq "*")
        {
            $hasMfaRelyingParty = $true
            Write-Host "User was enabled for per-user MFA." -ForegroundColor Yellow
        }
    }

    if ($user.StrongAuthenticationMethods.Count -gt 0 -and -not $hasMfaRelyingParty)
    {
        Write-Host $user.UserPrincipalName " is eligible for rollback" -ForegroundColor Yellow
        Write-Host "Rolling back user ..." -ForegroundColor Yellow
        Reset-MsolStrongAuthenticationMethodByUpn -UserPrincipalName $user.UserPrincipalName
        Write-Host "Successfully rolled back user " $user.UserPrincipalName -ForegroundColor Green
    }
    else
    {
        Write-Host $user.UserPrincipalName " is not eligible for rollback. No action required."
    }

    Write-Host ""
    Start-Sleep -Milliseconds 750
}

# Connect
Import-Module MSOnline
Connect-MsolService

foreach($line in Get-Content $path)
{
    RemediateUser -ObjectId $line
}
```

### <a name="rollback"></a>Reversão

Numa janela do PowerShell, execute o seguinte comando depois de atualizar as localizações realçadas. Introduza as credenciais de administrador global quando lhe for pedido. O script irá transmitir o resultado de cada operação de atualização de utilizador.

`<script location> -path <user file location>`

## <a name="disable-preview-experience"></a>Desativar a experiência de pré-visualização

Para desativar a experiência de pré-visualização para os seus utilizadores, conclua os seguintes passos:

1. Inicie sessão no portal do Azure como um administrador global ou administrador de utilizadores.
2. Navegue até **do Azure Active Directory**, **definições do utilizador**, **gerir as definições para as funcionalidades de pré-visualização do painel de acesso**.
3. Sob **os utilizadores podem utilizar funcionalidades de pré-visualização para registar e gerir informações de segurança**, defina o seletor para **None** e clique em **guardar**.

Os utilizadores já não serão pedidos para se registar com a experiência de pré-visualização.

## <a name="next-steps"></a>Passos Seguintes

[Saiba mais sobre a pré-visualização pública do convergida de registo para reposição de palavra-passe self-service e o Azure multi-factor Authentication](concept-registration-mfa-sspr-converged.md)