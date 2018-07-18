---
title: Gerir a CDN do Azure com o PowerShell | Documentos da Microsoft
description: Saiba como utilizar cmdlets do Azure PowerShell para gerir a CDN do Azure.
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: fb6f57a5-6e26-4847-8fd9-b51fb05a79eb
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2018
ms.author: mazha
ms.openlocfilehash: 15feb7b1d2873bc3f088eaad78079df2e063d73b
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39114077"
---
# <a name="manage-azure-cdn-with-powershell"></a>Gerir a CDN do Azure com o PowerShell
PowerShell fornece um dos métodos mais flexíveis para gerir os seus perfis de CDN do Azure e pontos de extremidade.  Pode utilizar o PowerShell interativamente ou ao escrever scripts para automatizar tarefas de gestão.  Este tutorial demonstra várias das tarefas mais comuns que pode realizar com o PowerShell para gerir os seus perfis de CDN do Azure e pontos de extremidade.

## <a name="prerequisites"></a>Pré-requisitos
Para utilizar o PowerShell para gerir os seus perfis de CDN do Azure e os pontos finais, tem de ter o módulo Azure PowerShell instalado.  Para saber como instalar o Azure PowerShell e ligue-se para o Azure com o `Connect-AzureRmAccount` cmdlet, consulte [como instalar e configurar o Azure PowerShell](/powershell/azure/overview).

> [!IMPORTANT]
> Tem de iniciar sessão com `Connect-AzureRmAccount` antes de pode executar cmdlets do PowerShell do Azure.
> 
> 

## <a name="listing-the-azure-cdn-cmdlets"></a>Listar os cmdlets do CDN do Azure
Pode listar todos os cmdlets de CDN do Azure com o `Get-Command` cmdlet.

```text
PS C:\> Get-Command -Module AzureRM.Cdn

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Get-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpointNameAvailability             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfileSsoUrl                        2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Publish-AzureRmCdnEndpointContent                  2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnCustomDomain                      2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnEndpoint                          2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnProfile                           2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Start-AzureRmCdnEndpoint                           2.0.0      AzureRm.Cdn
Cmdlet          Stop-AzureRmCdnEndpoint                            2.0.0      AzureRm.Cdn
Cmdlet          Test-AzureRmCdnCustomDomain                        2.0.0      AzureRm.Cdn
Cmdlet          Unpublish-AzureRmCdnEndpointContent                2.0.0      AzureRm.Cdn
```

## <a name="getting-help"></a>Obter ajuda
Pode obter ajuda com qualquer um destes cmdlets com a `Get-Help` cmdlet.  `Get-Help` fornece sintaxe e de utilização e, opcionalmente, mostra exemplos.

```text
PS C:\> Get-Help Get-AzureRmCdnProfile

NAME
    Get-AzureRmCdnProfile

SYNOPSIS
    Gets an Azure CDN profile.


SYNTAX
    Get-AzureRmCdnProfile [-ProfileName <String>] [-ResourceGroupName <String>] [-InformationAction
    <ActionPreference>] [-InformationVariable <String>] [<CommonParameters>]


DESCRIPTION
    Gets an Azure CDN profile and all related information.


RELATED LINKS

REMARKS
    To see the examples, type: "get-help Get-AzureRmCdnProfile -examples".
    For more information, type: "get-help Get-AzureRmCdnProfile -detailed".
    For technical information, type: "get-help Get-AzureRmCdnProfile -full".

```

## <a name="listing-existing-azure-cdn-profiles"></a>Lista de perfis de CDN do Azure existentes
O `Get-AzureRmCdnProfile` cmdlet sem quaisquer parâmetros obtém todos os seus perfis CDN existentes.

```powershell
Get-AzureRmCdnProfile
```

Este resultado pode ser enviada por pipe para cmdlets de enumeração.

```powershell
# Output the name of all profiles on this subscription.
Get-AzureRmCdnProfile | ForEach-Object { Write-Host $_.Name }

# Return only **Azure CDN from Verizon** profiles.
Get-AzureRmCdnProfile | Where-Object { $_.Sku.Name -eq "Standard_Verizon" }
```

Também podem retornar um único perfil ao especificar o grupo de recursos e nome de perfil.

```powershell
Get-AzureRmCdnProfile -ProfileName CdnDemo -ResourceGroupName CdnDemoRG
```

> [!TIP]
> É possível ter vários perfis da CDN com o mesmo nome, desde que eles estão em grupos de recursos diferentes.  Omitindo o `ResourceGroupName` parâmetro retorna todos os perfis com um nome correspondente.
> 
> 

## <a name="listing-existing-cdn-endpoints"></a>Lista de pontos finais CDN existentes
`Get-AzureRmCdnEndpoint` Pode obter um ponto de extremidade individual ou de todos os pontos finais num perfil.  

```powershell
# Get a single endpoint.
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Get all of the endpoints on a given profile. 
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG

# Return all of the endpoints on all of the profiles.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint

# Return all of the endpoints in this subscription that are currently running.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Where-Object { $_.ResourceState -eq "Running" }
```

## <a name="creating-cdn-profiles-and-endpoints"></a>Criação de perfis da CDN e os pontos finais
`New-AzureRmCdnProfile` e `New-AzureRmCdnEndpoint` são utilizados para criar perfis de CDN e os pontos finais. São suportados os SKUs seguintes:
- Standard_Verizon
- Premium_Verizon
- Custom_Verizon
- Standard_Akamai
- Standard_ChinaCdn

> [!NOTE]
> O SKU de Standard_Microsoft não é suportado enquanto estiverem em pré-visualização.

```powershell
# Create a new profile
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku Standard_Akamai -Location "Central US"

# Create a new endpoint
New-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Location "Central US" -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

# Create a new profile and endpoint (same as above) in one line
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku Standard_Akamai -Location "Central US" | New-AzureRmCdnEndpoint -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

```

## <a name="checking-endpoint-name-availability"></a>A verificar a disponibilidade de nome de ponto final
`Get-AzureRmCdnEndpointNameAvailability` Devolve um objeto que indica se um nome de ponto final está disponível.

```powershell
# Retrieve availability
$availability = Get-AzureRmCdnEndpointNameAvailability -EndpointName "cdnposhdoc"

# If available, write a message to the console.
If($availability.NameAvailable) { Write-Host "Yes, that endpoint name is available." }
Else { Write-Host "No, that endpoint name is not available." }
```

## <a name="adding-a-custom-domain"></a>Adicionar um domínio personalizado
`New-AzureRmCdnCustomDomain` Adiciona um nome de domínio personalizado para um ponto final existente.

> [!IMPORTANT]
> Tem de configurar o CNAME com o fornecedor DNS conforme descrito em [como mapear um domínio personalizado ao ponto final de rede de entrega de conteúdos (CDN)](cdn-map-content-to-custom-domain.md).  Pode testar o mapeamento antes de modificar seu ponto final com `Test-AzureRmCdnCustomDomain`.
> 
> 

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Check the mapping
$result = Test-AzureRmCdnCustomDomain -CdnEndpoint $endpoint -CustomDomainHostName "cdn.contoso.com"

# Create the custom domain on the endpoint
If($result.CustomDomainValidated){ New-AzureRmCdnCustomDomain -CustomDomainName Contoso -HostName "cdn.contoso.com" -CdnEndpoint $endpoint }
```

## <a name="modifying-an-endpoint"></a>Modificar um ponto final
`Set-AzureRmCdnEndpoint` Modifica um ponto final existente.

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Set up content compression
$endpoint.IsCompressionEnabled = $true
$endpoint.ContentTypesToCompress = "text/javascript","text/css","application/json"

# Save the changed endpoint and apply the changes
Set-AzureRmCdnEndpoint -CdnEndpoint $endpoint
```

## <a name="purgingpre-loading-cdn-assets"></a>Remover/Pre-loading CDN ativos
`Unpublish-AzureRmCdnEndpointContent` limpezas de cache ativos, enquanto `Publish-AzureRmCdnEndpointContent` pré-carregar ativos em pontos finais suportados.

```powershell
# Purge some assets.
Unpublish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -PurgeContent "/images/kitten.png","/video/rickroll.mp4"

# Pre-load some assets.
Publish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -LoadContent "/images/kitten.png","/video/rickroll.mp4"

# Purge everything in /images/ on all endpoints.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Unpublish-AzureRmCdnEndpointContent -PurgeContent "/images/*"
```

## <a name="startingstopping-cdn-endpoints"></a>Pontos finais da CDN do iniciar/parar
`Start-AzureRmCdnEndpoint` e `Stop-AzureRmCdnEndpoint` pode ser utilizado para iniciar e parar pontos finais individuais ou grupos de pontos de extremidade.

```powershell
# Stop the cdndocdemo endpoint
Stop-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Stop all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Stop-AzureRmCdnEndpoint

# Start all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Start-AzureRmCdnEndpoint
```

## <a name="deleting-cdn-resources"></a>Eliminar recursos CDN
`Remove-AzureRmCdnProfile` e `Remove-AzureRmCdnEndpoint` pode ser utilizado para remover perfis e pontos finais.

```powershell
# Remove a single endpoint
Remove-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Remove all the endpoints on a profile and skip confirmation (-Force)
Get-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG | Get-AzureRmCdnEndpoint | Remove-AzureRmCdnEndpoint -Force

# Remove a single profile
Remove-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG
```

## <a name="next-steps"></a>Próximos Passos
Saiba como automatizar o CDN do Azure com [.NET](cdn-app-dev-net.md) ou [node. js](cdn-app-dev-node.md).

Para saber mais sobre as funcionalidades CDN, veja [descrição geral da CDN](cdn-overview.md).

