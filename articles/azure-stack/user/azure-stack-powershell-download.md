---
title: Descarregar as ferramentas de pilha do Azure a partir do GitHub | Microsoft Docs
description: Saiba como transferir as ferramentas necessárias para trabalhar com a pilha do Azure.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 28F360AD-789A-488D-965F-FC6E6CCF3329
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: mabrigg
ms.openlocfilehash: a5bc23ee6f986da80630371bafcd8ec80dde3577
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
ms.locfileid: "34258883"
---
# <a name="download-azure-stack-tools-from-github"></a>Descarregar as ferramentas de pilha do Azure a partir do GitHub

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Ferramentas de AzureStack é um repositório do GitHub que aloja os módulos do PowerShell que pode utilizar para gerir e implementar recursos com pilha do Azure.

## <a name="download-targets"></a>Transferir destinos

Pode transferir e utilizar estes módulos do PowerShell para o Kit de desenvolvimento de pilha do Azure ou para um cliente externo baseado em Windows que utiliza uma ligação VPN.

## <a name="how-to-get-the-tools"></a>Como obter as ferramentas

Para obter estas ferramentas, clone o repositório GitHub AzureStack ferramentas ou transferir a pasta Ferramentas AzureStack executando o seguinte script:

```PowerShell
# Change directory to the root directory
cd \

# Download the tools archive
invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

# Expand the downloaded files
expand-archive master.zip `
  -DestinationPath . `
  -Force

# Change to the tools directory
cd AzureStack-Tools-master

```

## <a name="functionalities-provided-by-the-modules"></a>Funcionalidades fornecidas pelos módulos

O repositório de ferramentas AzureStack contém módulos do PowerShell que suportam as seguintes funcionalidades para a pilha do Azure:

| Funcionalidade | Descrição | Quem pode utilizar este módulo? |
| --- | --- | --- |
| [Capacidades de nuvem](azure-stack-validate-templates.md) | Utilize este módulo para obter as capacidades de nuvem de uma nuvem. Por exemplo, pode obter as capacidades de nuvem, tais como a versão de API, os recursos do Azure Resource Manager, etc. as extensões de VM para a pilha do Azure e nuvens do Azure utilizando este módulo. | Os administradores de nuvem e os utilizadores. |
| [Política do Gestor de recursos para a pilha do Azure](azure-stack-policy-module.md) | Utilize este módulo para configurar uma subscrição do Azure ou um grupo de recursos do Azure com a disponibilidade de serviço e controlo de versões do mesma como pilha do Azure. | Os administradores de nuvem e de utilizadores |
| [Ligar a pilha do Azure](azure-stack-connect-azure-stack.md) | Utilize este módulo para ligar a uma instância de pilha do Azure através do PowerShell bem como para configurar a conectividade VPN pilha do Azure. | Os administradores de nuvem e de utilizadores |
| [A validação do modelo](azure-stack-validate-templates.md) | Utilize este módulo para verificar se um existente ou um novo modelo pode ser implementado com pilha do Azure. | Os administradores de nuvem e de utilizadores |

## <a name="next-steps"></a>Passos Seguintes

* [Configurar o Azure pilha ambiente do utilizador do PowerShell](azure-stack-powershell-configure-user.md)
* [Ligar ao Kit de desenvolvimento de pilha do Azure através de uma VPN](azure-stack-connect-azure-stack.md)
