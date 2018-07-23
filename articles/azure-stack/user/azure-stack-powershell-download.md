---
title: Transferir ferramentas do Azure Stack a partir do GitHub | Documentos da Microsoft
description: Aprenda a transferir as ferramentas necessárias para trabalhar com o Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2018
ms.author: mabrigg
ms.openlocfilehash: 9c1e4abe50b06db58a4ca05a99e1ae4a531b2294
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39187456"
---
# <a name="download-azure-stack-tools-from-github"></a>Transferir ferramentas do Azure Stack a partir do GitHub

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Ferramentas de AzureStack é um repositório do GitHub que aloja os módulos do PowerShell que pode utilizar para gerir e implementar recursos no Azure Stack.

## <a name="download-targets"></a>Transferir destinos

Pode transferir e utilizar estes módulos do PowerShell para o Development Kit do Azure Stack ou para um cliente externo com base em Windows que utiliza uma ligação VPN.

## <a name="how-to-get-the-tools"></a>Como obter as ferramentas

Para obter essas ferramentas, clone o repositório GitHub de ferramentas de AzureStack ou transfira a pasta de ferramentas de AzureStack executando o seguinte script:

```PowerShell
# Change directory to the root directory
cd \

# Download the tools archive
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
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

## <a name="functionalities-provided-by-the-modules"></a>Funcionalidades fornecidos por módulos

O repositório de ferramentas de AzureStack contém módulos do PowerShell que suportam as seguintes funcionalidades para o Azure Stack:

| Funcionalidade | Descrição | Quem pode utilizar este módulo? |
| --- | --- | --- |
| [Recursos de nuvem](https://github.com/Azure/AzureStack-Tools/tree/master/CloudCapabilities) | Utilize este módulo para obter os recursos de nuvem de uma nuvem. Por exemplo, pode obter os recursos de nuvem, como versão de API, os recursos do Azure Resource Manager, etc. as extensões de VM para o Azure Stack e clouds do Azure com este módulo. | Os administradores de nuvem e os utilizadores. |
| [Política do Gestor de recursos para o Azure Stack](azure-stack-policy-module.md) | Utilize este módulo para configurar uma subscrição do Azure ou um grupo de recursos do Azure com a mesmo controle de versão e disponibilidade do serviço como o Azure Stack. | Os administradores de nuvem e de utilizadores |
| [Ligar ao Azure Stack](azure-stack-connect-azure-stack.md) | Utilize este módulo para ligar a uma instância do Azure Stack através do PowerShell e para configurar a conectividade VPN para o Azure Stack. | Os administradores de nuvem e de utilizadores |
| [A validação do modelo](azure-stack-validate-templates.md) | Utilize este módulo para verificar se um existente ou um novo modelo pode ser implementado para o Azure Stack. | Os administradores de nuvem e de utilizadores |

## <a name="next-steps"></a>Passos Seguintes

- [Configurar o ambiente do PowerShell do utilizador do Azure Stack](azure-stack-powershell-configure-user.md)
- [Ligar ao Kit de desenvolvimento do Azure Stack através de uma VPN](azure-stack-connect-azure-stack.md)
