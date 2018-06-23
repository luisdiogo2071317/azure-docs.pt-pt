---
title: Ver utilização de LUIS a partir da Shell de nuvem do Azure | Microsoft Docs
description: Saiba como obter as informações de utilização na Shell de nuvem do Azure para LUIS.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/08/2017
ms.author: v-geberr
ms.openlocfilehash: 2de25645e5377efdd53bcc980695804d34db5ee2
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354566"
---
# <a name="manage-luis-service-from-azure-cloud-shell"></a>Gerir o serviço de LUIS a partir da Shell de nuvem do Azure
O portal do Azure permite-lhe utilizar cmdlets do PowerShell para trabalhar com recursos LUIS. 

Estes cmdlets permitem-lhe [criar](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/new-azurermcognitiveservicesaccount?view=azurermps-6.0.0) uma subscrição de LUIS, obter informações sobre a subscrição, incluindo [utilização](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/get-azurermcognitiveservicesaccountusage?view=azurermps-6.0.0), e [remover](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/remove-azurermcognitiveservicesaccount?view=azurermps-6.0.0) a subscrição. 

## <a name="cloud-shell-storage-account-and-authentication"></a>Conta de armazenamento de shell de nuvem e de autenticação
Para poder utilizar o PowerShell no portal do Azure [nuvem shell](https://docs.microsoft.com/azure/cloud-shell/quickstart-powershell), tem de ter uma conta de armazenamento do Azure. Se não tiver um [conta de armazenamento](https://docs.microsoft.com/en-us/azure/cloud-shell/persisting-shell-storage#set-up-a-clouddrive-file-share), será solicitado para criar um. A conta de armazenamento permite-lhe guardar scripts do PowerShell na shell de nuvem.  

Também tem de autenticar para o Azure, na shell para aceder a quaisquer recursos na nuvem. 

Depois de ter uma conta de armazenamento e são autenticados, pode executar cmdlets do PowerShell.

## <a name="open-cloud-shell"></a>Abrir o Cloud Shell
Quando utiliza a shell de nuvem de portal do Azure, são sempre na versão mais recente do PowerShell. 

Utilize o **inicie o Shell de nuvem** botão para abrir a Shell de nuvem ou abra um browser com [ https://shell.azure.com ](https://shell.azure.com). 

<a style="cursor:pointer" onclick='javascript:window.open("https://shell.azure.com", "_blank", "toolbar=no,scrollbars=yes,resizable=yes,menubar=no,location=no,status=no")'><image src="https://shell.azure.com/images/launchcloudshell.png" /></a>

## <a name="luis-endpoint-usage-information"></a>Informações de utilização do ponto final de LUIS

O cmdlet do PowerShell 6. x, `Get-AzureRmCognitiveServicesAccountUsage`, fornece informações de utilização cognitivos nos serviços da Microsoft, incluindo LUIS. [Get-AzureRmCognitiveServicesAccountUsage](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/get-azurermcognitiveservicesaccountusage?view=azurermps-6.0.0) necessita do grupo de recursos e o nome de recurso do serviço. 

A sintaxe de comando é:

```
Get-AzureRmCognitiveServicesAccountUsage -ResourceGroupName my-resource-group -Name my-luis-service-name
```

No exemplo seguinte, o nome do grupo de recursos é `luis-westus-rg` e o nome de subscrição de serviço LUIS é `luis-westus-1`. Ambos estes nomes são escolhidos quando o serviço de LUIS é criado. 

O cmdlet devolve informações de utilização de 16 10 000 pedidos de ponto final utilizados num período de 30 dias com o período de terminados 7 de Junho de:

```
CurrentValue  : 16
Name          : LUIS.Calls
Limit         : 10000
Status        : Included
Unit          : Count
QuotaPeriod   : 30.00:00:00
NextResetTime : 2018-06-07T18:28:52Z
```

Guarde o comando como um ficheiro de PowerShell, *.ps1, na conta do storage do Azure associada com a shell de nuvem e de ser executado em qualquer altura. 

![Executar script do armazenamento](./media/luis-how-to-manage-from-powershell/run-script-from-storage.png)

Assim que o script é guardado na unidade de nuvem, pode executar o script do PowerShell do Azure shell de nuvem da aplicação de telefone. 

![Executar script do armazenamento na aplicação de telefone](./media/luis-how-to-manage-from-powershell/phone-app.png)