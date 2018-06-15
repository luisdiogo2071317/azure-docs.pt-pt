---
title: Azure reservado erros de nomes de recursos | Microsoft Docs
description: Descreve como resolver erros ao fornecer um nome de recurso que inclui uma palavra reservada.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/08/2017
ms.author: tomfitz
ms.openlocfilehash: b91a53d17d64afb0a56f745505f10e8cabbc22cc
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
ms.locfileid: "34357066"
---
# <a name="resolve-reserved-resource-name-errors"></a>Resolver erros de nomes de recursos reservados

Este artigo descreve o erro que surgir ao implementar um recurso que inclui uma palavra reservada no respetivo nome.

## <a name="symptom"></a>Sintoma

Quando implementar um recurso que está disponível através de um ponto final público, poderá receber o erro seguinte:

```
Code=ReservedResourceName;
Message=The resource name <resource-name> or a part of the name is a trademarked or reserved word.
```

## <a name="cause"></a>Causa

Recursos que tenham um ponto final público não é possível utilizar palavras reservadas ou marcas comerciais no nome.

As seguintes palavras estão reservadas:

* ACESSO
* AZURE
* BING
* ELEGÍVEIS DO BIZSPARK
* BIZTALK
* CORTANA
* POR DIRECTX
* DOTNET
* DYNAMICS
* EXCEL
* EXCHANGE
* FOREFRONT
* GROOVE
* HOLOLENS
* HYPER-V
* KINECT
* LYNC
* MSDN
* O365
* OFFICE
* OFFICE 365
* ONEDRIVE
* ONENOTE
* OUTLOOK
* POWERPOINT
* SHAREPOINT
* SKYPE
* VISIO
* VISUALSTUDIO

As seguintes palavras não podem ser utilizadas como uma palavra de toda ou uma subcadeia no nome:

* INÍCIO DE SESSÃO
* MICROSOFT
* WINDOWS
* XBOX

## <a name="solution"></a>Solução

Forneça um nome que não utilize uma das palavras reservadas.