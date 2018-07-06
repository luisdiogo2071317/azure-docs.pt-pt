---
title: Os utilizadores do Azure do Cloud Shell para Windows | Documentos da Microsoft
description: Guia para utilizadores que não estão familiarizados com sistemas Linux
services: azure
documentationcenter: ''
author: maertendMSFT
manager: hemantm
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/03/2018
ms.author: damaerte
ms.openlocfilehash: 5e318a0f64033aa0c4b306e547c11e1994afa229
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37861900"
---
# <a name="powershell-in-azure-cloud-shell-for-windows-users"></a>PowerShell dos utilizadores do Azure Cloud Shell para Windows

Em Maio de 2018, as alterações foram [anunciado](https://azure.microsoft.com/blog/pscloudshellrefresh/) para o PowerShell no Azure Cloud Shell.  A experiência do PowerShell no Azure Cloud Shell é agora o PowerShell Core 6 no Linux.
Com esta alteração, existem alguns aspetos do PowerShell no Cloud Shell, que podem ser diferentes daquelas que é esperado no Windows PowerShell 5.1.

## <a name="case-sensitivity"></a>Maiúsculas e minúsculas

No Windows, o sistema de ficheiros diferencia maiúsculas de minúsculas.  No Linux, o sistema de ficheiros diferencia maiúsculas de minúsculas.
Isso significa que anteriormente `file.txt` e `FILE.txt` foram consideradas para ser o mesmo ficheiro, agora eles são considerados como arquivos diferentes.
Maiúsculas/minúsculas adequadas têm de ser utilizadas enquanto `tab` concluir no sistema de arquivos.  Experiências específicas do PowerShell, tal como `tab` cmdlets, não diferenciam maiúsculas de minúsculas. 

## <a name="windows-powershell-alias-vs-linux-utilities"></a>Windows PowerShell alias vs utilitários do Linux

Existente comandos no Linux, como `ls`, `sort`, e `sleep` têm precedência sobre os seus aliases do PowerShell.  Seguem-se aliases removidos comuns, bem como os comandos equivalentes:  

|Alias removido   |Comando equivalente   |
|---|---|
|`ls`     | `dir` <br> `Get-ChildItem` |
|`sort`   | `Sort-Object` |
|`sleep`  | `Start-Sleep` |

## <a name="persisting-home-vs-homeclouddrive"></a>Persistência $home vs $home\clouddrive

Para os utilizadores de que persistentes scripts e outros arquivos na respetiva unidade na Cloud, o diretório de $HOME agora é mantido entre sessões.

## <a name="powershell-profile"></a>Perfil do PowerShell

Por predefinição, não é criado um perfil do PowerShell.  Para criar seu perfil, crie uma `PowerShell` diretório em `$HOME/.config`.  Na `$HOME/.config/PowerShell`, pode criar seu perfil sob o nome `Microsoft.PowerShell_profile.ps1`.

## <a name="whats-new-in-powershell-core-6"></a>O que há de novo no PowerShell Core 6

Para obter mais informações sobre o que há de novo no PowerShell Core 6, fazer referência a [documentos do PowerShell](https://docs.microsoft.com/powershell/scripting/whats-new/what-s-new-in-powershell-core-60?view=powershell-6) e o [introdução ao PowerShell Core](https://blogs.msdn.microsoft.com/powershell/2017/06/09/getting-started-with-powershell-core-on-windows-mac-and-linux/) mensagem de blogue
