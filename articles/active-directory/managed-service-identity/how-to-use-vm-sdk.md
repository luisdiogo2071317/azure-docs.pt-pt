---
title: Como utilizar uma identidade de serviço gerida do Azure VM com os SDKs do Azure
description: Exemplos de código para utilizar os SDKs do Azure com um MSI de VM do Azure.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: daveba
ms.openlocfilehash: 389b9967003c681b974f4580264701d22c103b7e
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="how-to-use-an-azure-vm-managed-service-identity-msi-with-azure-sdks"></a>Como utilizar um Azure VM geridos serviço de identidade (MSI) com os SDKs do Azure 

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]  
Este artigo fornece uma lista de amostras SDK, que demonstram a utilização de suporte dos seus respetivos Azure do SDK para MSI.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

> [!IMPORTANT]
> - Todos os código/script de exemplo neste artigo assume que o cliente está em execução numa máquina Virtual ativada de MSI. Utilize a funcionalidade VM "Ligar" no portal do Azure, para ligar remotamente à VM. Para obter mais informações sobre como ativar MSI numa VM, consulte [configurar uma VM geridos serviço de identidade (MSI) no portal do Azure](qs-configure-portal-windows-vm.md), ou um dos artigos variantes (utilizando o PowerShell, CLI, um modelo ou um SDK do Azure). 

## <a name="sdk-code-samples"></a>Exemplos de código do SDK

| SDK             | Exemplo de código |
| --------------- | ----------- |
| .NET            | [Implementar um modelo Azure Resource Manager a partir de uma VM do Windows com a identidade de serviço geridas](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet) |
| .NET Core       | [Chamar serviços do Azure a partir de uma VM com Linux através da identidade de serviço geridas](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/) |
| Node.js         | [Gerir recursos com a identidade de serviço geridas](https://azure.microsoft.com/resources/samples/resources-node-manage-resources-with-msi/) |
| Python          | [Utilizar MSI para autenticar simplesmente de dentro de uma VM](https://azure.microsoft.com/resources/samples/resource-manager-python-manage-resources-with-msi/) |
| Ruby            | [Gerir recursos a partir de uma VM MSI-ativado](https://azure.microsoft.com/resources/samples/resources-ruby-manage-resources-with-msi/) |

## <a name="related-content"></a>Conteúdo relacionado

- Consulte [Azure SDKs](https://azure.microsoft.com/downloads/) para a lista completa dos recursos do SDK do Azure, incluindo as transferências de biblioteca, documentação e muito mais.
- Para ativar o MSI numa VM do Azure, consulte [configurar uma VM geridos serviço de identidade (MSI) no portal do Azure](qs-configure-portal-windows-vm.md).

Utilize a seguinte secção de comentários para fornecer comentários e ajudam-nos refinar e formam o nosso conteúdo.








