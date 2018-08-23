---
title: Criar um contentor de serviço cloud com o PowerShell | Documentos da Microsoft
description: Este artigo explica como criar um contentor de serviço cloud com o PowerShell. O contentor aloja funções web e de trabalho.
services: cloud-services
documentationcenter: .net
author: cawaMS
manager: timlt
editor: ''
ms.assetid: c8f32469-610e-4f37-a3aa-4fac5c714e13
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 11/18/2016
ms.author: cawa
ms.openlocfilehash: 0a05b4fadf2cd7e794680907d39f355882a4330c
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/10/2018
ms.locfileid: "42055153"
---
# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a>Utilizar um comando do PowerShell do Azure para criar um contentor de serviço cloud vazio
Este artigo explica como criar rapidamente um contentor de serviços em nuvem utilizando cmdlets do PowerShell do Azure. Siga os passos abaixo:

1. Instalar o cmdlet PowerShell do Microsoft Azure a partir da [downloads do Azure PowerShell](http://aka.ms/webpi-azps) página.
2. Abra a linha de comandos do PowerShell.
3. Utilize o [Add-AzureAccount](/powershell/module/servicemanagement/azure/add-azureaccount?view=azuresmps-4.0.0) para iniciar sessão.

   > [!NOTE]
   > Para obter mais instruções sobre como instalar o cmdlet do PowerShell do Azure e ligar a sua subscrição do Azure, consulte [como instalar e configurar o Azure PowerShell](/powershell/azure/overview).
   >
   >
4. Utilize o **New-AzureService** cmdlet para criar um contentor de serviço cloud do Azure vazia.

   ```powershell
   New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   ```
5. Siga este exemplo para invocar o cmdlet:

   ```powershell
   New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
   ```

Para obter mais informações sobre como criar o serviço cloud do Azure, execute:

```
Get-help New-AzureService
```

### <a name="next-steps"></a>Passos Seguintes
* Para gerir a implementação de serviço em nuvem, consulte a [Get-AzureService](/powershell/module/servicemanagement/azure/Get-AzureService?view=azuresmps-4.0.0), [Remove-AzureService](/powershell/module/servicemanagement/azure/Remove-AzureService?view=azuresmps-4.0.0), e [Set-AzureService](/powershell/module/servicemanagement/azure/set-azureservice?view=azuresmps-4.0.0) comandos. Também pode consultar [como configurar os serviços cloud](cloud-services-how-to-configure-portal.md) para obter mais informações.
* Para publicar o seu projeto de serviço cloud no Azure, consulte a **PublishCloudService.ps1** código de exemplo da [repositório de serviços cloud arquivados](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Scripts/cloud-services-continuous-delivery).
