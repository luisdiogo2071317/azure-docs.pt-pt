---
title: Descarregar as ferramentas de pilha do Azure a partir do GitHub | Microsoft Docs
description: Saiba como transferir as ferramentas que são necessárias para trabalhar com a pilha do Azure.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 05/10/2018
ms.author: mabrigg
ms.reviewer: thoroet
ms.openlocfilehash: a116b7a048ff95ca601a65633cdc63f98fefee9d
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/12/2018
---
# <a name="download-azure-stack-tools-from-github"></a>Descarregar as ferramentas de pilha do Azure a partir do GitHub

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

**Ferramentas de AzureStack** é um repositório do GitHub que aloja os módulos do PowerShell para gerir e implementar recursos com pilha do Azure. Se estiver a planear estabelecer conectividade VPN, pode transferir estes módulos do PowerShell para o Kit de desenvolvimento de pilha do Azure ou para um cliente externo baseado em Windows. Para obter estas ferramentas, clone o repositório do GitHub ou transferir o **AzureStack ferramentas** pasta executando o seguinte script:

```PowerShell
# Change directory to the root directory. 
cd \

# Download the tools archive.
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12 
invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

# Expand the downloaded files.
expand-archive master.zip `
  -DestinationPath . `
  -Force

# Change to the tools directory.
cd AzureStack-Tools-master

```

## <a name="functionality-provided-by-the-modules"></a>Funcionalidade fornecida por módulos

O **AzureStack ferramentas** repositório inclui módulos do PowerShell que suportam as seguintes funcionalidades para a pilha do Azure:  

| Funcionalidade | Descrição | Quem pode utilizar este módulo? |
| --- | --- | --- |
| [Capacidades de nuvem](user/azure-stack-validate-templates.md) | Utilize este módulo para obter as capacidades de nuvem de uma nuvem. Por exemplo, ao utilizar este módulo, pode obter as capacidades de nuvem, tais como versão de API e de recursos do Azure Resource Manager. Também pode obter as extensões VM para a pilha do Azure e nuvens do Azure utilizando este módulo. | Os operadores da nuvem e de utilizadores |
| [Política do Gestor de recursos para a pilha do Azure](user/azure-stack-policy-module.md) | Utilize este módulo para configurar uma subscrição do Azure ou um grupo de recursos do Azure com a disponibilidade de serviço e controlo de versões do mesma como pilha do Azure. | Os operadores da nuvem e de utilizadores |
| [Registar com o Azure](azure-stack-register.md) | Utilize este módulo para registar a sua instância do kit de desenvolvimento com o Azure. Após o registo, pode transferir os itens do marketplace a partir do Azure e utilizá-los na pilha do Azure. | Operadores da nuvem |
| [Implementação de pilha do Azure](azure-stack-run-powershell-script.md) | Utilize este módulo para preparar o computador do anfitrião de pilha do Azure para implementar e volte a implementar utilizando a imagem de disco rígido virtual (VHD) de pilha do Azure. | Operadores da nuvem|
| [Ligar a pilha do Azure](azure-stack-connect-powershell.md) | Utilize este módulo para configurar a conectividade VPN pilha do Azure. | Os operadores da nuvem e de utilizadores |
| [A validação do modelo](user/azure-stack-validate-templates.md) | Utilize este módulo para verificar se um existente ou um novo modelo pode ser implementado com pilha do Azure. | Os operadores da nuvem e de utilizadores|


## <a name="next-steps"></a>Passos Seguintes
* [Configurar o Azure pilha ambiente do utilizador do PowerShell](user/azure-stack-powershell-configure-user.md)   
* [Ligar ao Kit de desenvolvimento de pilha do Azure através de uma VPN](azure-stack-connect-azure-stack.md)  
