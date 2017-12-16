---
title: Incorporar Shell em nuvem do Azure | Microsoft Docs
description: Saiba como incorporar a Shell de nuvem do Azure.
services: cloud-shell
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: juluk
ms.openlocfilehash: 78b539136971aa282e5447d7882ecb02f73f346b
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/16/2017
---
# <a name="embed-azure-cloud-shell"></a>Incorporar Shell em nuvem do Azure

Ao incorporar Shell da nuvem permite aos programadores e escritores conteúdos abrir diretamente Shell de nuvem a partir de um URL dedicado, [shell.azure.com](https://shell.azure.com). Isto coloca imediatamente a capacidade total de autenticação da Shell de nuvem, ferramentas, e atualizada do Azure CLI/Azure PowerShell ferramentas aos seus utilizadores.

[![](https://shell.azure.com/images/launchcloudshell.png "Inicie o Shell de nuvem do Azure")](https://shell.azure.com)

## <a name="how-to"></a>Procedimentos

Integre no botão de início da nuvem Shell ficheiros markdown copiando o seguinte:

```markdown
[![Launch Cloud Shell](https://shell.azure.com/images/launchcloudshell.png "Launch Cloud Shell")](https://shell.azure.com)
```

HTML para incorporar um pop-up que shell de nuvem é abaixo:
```html
<a style="cursor:pointer" onclick='javascript:window.open("https://shell.azure.com", "_blank", "toolbar=no,scrollbars=yes,resizable=yes,menubar=no,location=no,status=no")'><image src="https://shell.azure.com/images/launchcloudshell.png" /></a>
```

## <a name="customize-experience"></a>Personalizar a experiência

Defina uma experiência de shell específica, aumentar o seu URL.
|Experiência   |URL   |
|---|---|
|A maioria utilizados recentemente shell   |shell.Azure.com           |
|Bash                       |shell.Azure.com/bash       |
|PowerShell                 |shell.Azure.com/PowerShell |

## <a name="next-steps"></a>Passos seguintes
[Bash no início rápido da Shell de nuvem](quickstart.md)<br>
[PowerShell no início rápido da Shell de nuvem](quickstart-powershell.md)
