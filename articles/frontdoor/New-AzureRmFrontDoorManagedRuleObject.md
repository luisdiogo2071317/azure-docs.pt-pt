---
title: Serviço de porta de entrada do Azure - referência do PowerShell | Documentos da Microsoft
description: Esta referência fornece detalhes sobre os cmdlets do PowerShell diferentes, suportados para o serviço de porta de entrada do Azure
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2018
ms.author: sharadag
Module Name: AzureRM.FrontDoor
online version: https://docs.microsoft.com/powershell/module/azurerm.frontdoor/new-azurermfrontdoormanagedruleobject
schema: 2.0.0
ms.openlocfilehash: dad037243603c0f282f158c615ef69372d9967c5
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47047769"
---
# Novo AzureRmFrontDoorManagedRuleObject
Criar o objeto de ManagedRule WAF para criação de políticas

## SINTAXE

```
New-AzureRmFrontDoorManagedRuleObject -Priority <Int32> [-Version <String>]
 [-RuleGroupOverride <PSAzureRuleGroupOverride[]>] [-DefaultProfile <IAzureContextContainer>]
 [<CommonParameters>]
```

## DESCRIÇÃO
Criar o objeto de ManagedRule WAF para criação de políticas

## EXEMPLOS

### Exemplo 1
```powershell
PS C:\> New-AzureRmFrontDoor
ManagedRuleObject -Priority 1 -Version 0 -RuleGroupOverride $override1

RuleGroupOverrides                                                   Priority Version
------------------                                                   -------- -------
{Microsoft.Azure.Commands.FrontDoor.Models.PSAzureRuleGroupOverride}        1 0
```

Criar um objeto de ManagedRule

## PARÂMETROS

### -DefaultProfile
As credenciais, conta, inquilino e subscrição utilizada para comunicação com o Azure.

```yaml
Type: Microsoft.Azure.Commands.Common.Authentication.Abstractions.IAzureContextContainer
Parameter Sets: (All)
Aliases: AzureRmContext, AzureCredential

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Prioridade
Descreve a prioridade da regra

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -RuleGroupOverride
Lista do azure geridos a configuração de substituição do fornecedor

```yaml
Type: Microsoft.Azure.Commands.FrontDoor.Models.PSAzureRuleGroupOverride[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Versão
Versão de regras

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### CommonParameters
Este cmdlet suporta os parâmetros comuns: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction e -WarningVariable. Para obter mais informações, veja about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
