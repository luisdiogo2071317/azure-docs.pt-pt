---
title: "Criar um contentor de serviços em nuvem com o PowerShell | Microsoft Docs"
description: "Este artigo explica como criar um contentor de serviços em nuvem com o PowerShell. O contentor aloja funções web e de trabalho."
services: cloud-services
documentationcenter: .net
author: cawaMS
manager: timlt
editor: 
ms.assetid: c8f32469-610e-4f37-a3aa-4fac5c714e13
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 11/18/2016
ms.author: cawa
ms.openlocfilehash: b55a0dd7800448c50897af784092b4a60fa7a25e
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a>Utilizar um comando do PowerShell do Azure para criar um contentor do serviço de nuvem vazio
Este artigo explica como criar rapidamente um contentor de serviços em nuvem utilizando cmdlets do PowerShell do Azure. Siga os passos abaixo:

1. Instalar o cmdlet PowerShell do Microsoft Azure a partir de [transferências do Azure PowerShell](http://aka.ms/webpi-azps) página.
2. Abra a linha de comandos do PowerShell.
3. Utilize o [Add-AzureAccount](/powershell/module/Azure/Add-AzureAccount?view=azuresmps-4.0.0) para iniciar sessão.

   > [!NOTE]
   > Para obter mais instruções sobre como instalar o cmdlet do PowerShell do Azure e ligar à sua subscrição do Azure, consulte [como instalar e configurar o Azure PowerShell](/powershell/azure/overview).
   >
   >
4. Utilize o **New-AzureService** cmdlet para criar um contentor de serviços em nuvem do Azure vazio.

   ```powershell
   New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   ```
5. Siga este exemplo para invocar o cmdlet:

   ```powershell
   New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
   ```

Para obter mais informações sobre como criar o serviço em nuvem do Azure, execute:

```
Get-help New-AzureService
```

### <a name="next-steps"></a>Passos Seguintes
* Para gerir a implementação do serviço em nuvem, consulte o [Get-AzureService](/powershell/module/Azure/Get-AzureService?view=azuresmps-4.0.0), [Remove-AzureService](/powershell/module/Azure/Remove-AzureService?view=azuresmps-4.0.0), e [Set-AzureService](/powershell/module/azure/set-azureservice?view=azuresmps-4.0.0) comandos. Também pode referenciar a [como configurar os serviços de nuvem](cloud-services-how-to-configure-portal.md) para obter mais informações.
* Para publicar o projeto de serviço cloud do Azure, consulte o **PublishCloudService.ps1** exemplo de código do [repositório de serviços de nuvem arquivado](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Scripts/cloud-services-continuous-delivery).
