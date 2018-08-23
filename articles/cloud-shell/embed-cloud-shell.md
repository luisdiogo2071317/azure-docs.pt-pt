---
title: Incorporar o Azure Cloud Shell | Documentos da Microsoft
description: Aprenda a incorporar o Azure Cloud Shell.
services: cloud-shell
documentationcenter: ''
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: juluk
ms.openlocfilehash: 0bd5382e5ea37f7c3c52d119e9d39fe7e0bfdc7c
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/09/2018
ms.locfileid: "42059521"
---
# <a name="embed-azure-cloud-shell"></a>Incorporar o Azure Cloud Shell

Incorporar o Cloud Shell permite que os programadores e criadores de conteúdo abrir o Cloud Shell diretamente a um URL dedicado [shell.azure.com](https://shell.azure.com). Isso traz imediatamente todo o potencial da autenticação do Cloud Shell, ferramentas, e atualizados do Azure CLI/Azure PowerShell ferramentas aos seus utilizadores.

Botão de tamanho regular

[![](https://shell.azure.com/images/launchcloudshell.png "Inicie o Azure Cloud Shell")](https://shell.azure.com)

Botão de tamanho grande

[![](https://shell.azure.com/images/launchcloudshell@2x.png "Inicie o Azure Cloud Shell")](https://shell.azure.com)

## <a name="how-to"></a>Procedimentos

Integre o botão de lançamento do Cloud Shell em ficheiros de markdown ao copiar o seguinte:

```markdown
[![Launch Cloud Shell](https://shell.azure.com/images/launchcloudshell.png "Launch Cloud Shell")](https://shell.azure.com)
```

O HTML para incorporar um pop-up, o Cloud Shell é abaixo:
```html
<a style="cursor:pointer" onclick='javascript:window.open("https://shell.azure.com", "_blank", "toolbar=no,scrollbars=yes,resizable=yes,menubar=no,location=no,status=no")'><img alt="Launch Azure Cloud Shell" src="https://shell.azure.com/images/launchcloudshell.png" /></a>
```

## <a name="customize-experience"></a>Personalizar a experiência

Defina uma experiência de shell específica ao aumentar o seu URL.
|Experiência   |do IdP   |
|---|---|
|Mais recentemente utilizado shell   |[shell.azure.com](https://shell.azure.com)           |
|Bash                       |[shell.Azure.com/bash](https://shell.azure.com/bash)       |
|PowerShell                 |[shell.Azure.com/PowerShell](https://shell.azure.com/powershell) |

## <a name="next-steps"></a>Passos Seguintes
[Guia de introdução do Cloud Shell de bash](quickstart.md)<br>
[PowerShell no início rápido do Cloud Shell](quickstart-powershell.md)
