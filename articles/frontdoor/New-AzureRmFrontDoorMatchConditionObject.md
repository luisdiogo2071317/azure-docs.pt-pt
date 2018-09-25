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
online version: https://docs.microsoft.com/powershell/module/azurerm.frontdoor/new-azurermfrontdoormatchconditionobject
schema: 2.0.0
ms.openlocfilehash: 3a6804abf38126e9296760f5802f75ef35335294
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47047775"
---
# Novo AzureRmFrontDoorMatchConditionObject
Criar o objeto de MatchCondition WAF para criação de políticas

## SINTAXE

```
New-AzureRmFrontDoorMatchConditionObject -MatchVariable <PSMatchVariable>
 -OperatorProperty <PSOperatorProperty> -MatchValue <String[]> [-Selector <String>]
 [-NegateCondition <Boolean>] [-DefaultProfile <IAzureContextContainer>] [<CommonParameters>]
```

## DESCRIÇÃO
Criar o objeto de MatchCondition WAF para criação de políticas

## EXEMPLOS

### Exemplo 1
```powershell
PS C:\> New-AzureRmFrontDoorMatchConditionObject -MatchVariable RequestHeader -OperatorProperty Contains -Selector "UserAgent" -MatchValue "Windows"


MatchVariable    : RequestHeader
OperatorProperty : Contains
MatchValue       : {Windows}
Selector         : UserAgent
NegateCondition  : False
```

Criar um objeto de MatchCondition

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

### -MatchValue
Corresponde ao valor.

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -MatchVariable
Variável de correspondência.
Os valores possíveis incluem: 'RemoteAddr', 'RequestMethod', 'Cadeia de consulta', 'PostArgs', 'RequestUri', 'RequestHeader', 'RequestBody'

```yaml
Type: Microsoft.Azure.Commands.FrontDoor.Models.PSMatchVariable
Parameter Sets: (All)
Aliases:
Accepted values: RemoteAddr, RequestMethod, QueryString, PostArgs, RequestUri, RequestHeader, RequestBody

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -NegateCondition
Descreve se isso é negar a condição ou não o valor predefinido é false

```yaml
Type: System.Boolean
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -OperatorProperty
Descreve o operador para corresponder.
Os valores possíveis incluem: 'Any', 'IPMatch', "GeoMatch", "Igual", 'Contains', 'LessThan', 'GreaterThan', 'LessThanOrEqual', 'GreaterThanOrEqual', 'BeginsWith', ' EndsWith '

```yaml
Type: Microsoft.Azure.Commands.FrontDoor.Models.PSOperatorProperty
Parameter Sets: (All)
Aliases:
Accepted values: Any, IPMatch, GeoMatch, Equal, Contains, LessThan, GreaterThan, LessThanOrEqual, GreaterThanOrEqual, BeginsWith, EndsWith

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Seletor de
Nome do Seletor na RequestHeader ou RequestBody seja correspondida

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
