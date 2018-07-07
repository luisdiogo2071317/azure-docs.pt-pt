---
title: Como utilizar uma identidade de serviço gerido do Azure VM com SDKs do Azure
description: Exemplos de código para utilizar os SDKs do Azure com um MSI de VM do Azure.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: daveba
ms.openlocfilehash: 1c7a0d10f8ff005d90bb77f33bf40a00f97e078f
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/07/2018
ms.locfileid: "37901759"
---
# <a name="how-to-use-an-azure-vm-managed-service-identity-msi-with-azure-sdks"></a>Como utilizar um Azure VM Managed Service Identity (MSI) com SDKs do Azure 

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]  
Este artigo fornece uma lista de exemplos do SDK, que demonstram a utilização de suporte a do seus respectivos SDK do Azure MSI.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

> [!IMPORTANT]
> - Todos os código/script de exemplo neste artigo supõe que o cliente está em execução numa máquina Virtual ativado de MSI. Utilize a funcionalidade de "Ligar" de VM no portal do Azure, para ligar remotamente à VM. Para obter detalhes sobre como ativar o MSI numa VM, consulte [configurar uma VM Managed Service Identity (MSI) com o portal do Azure](qs-configure-portal-windows-vm.md), ou um dos artigos variantes (usando o PowerShell, CLI, um modelo ou um SDK do Azure). 

## <a name="sdk-code-samples"></a>Exemplos de código do SDK

| SDK             | Exemplo de código |
| --------------- | ----------- |
| .NET            | [Implementar um modelo Azure Resource Manager a partir de uma VM do Windows com a identidade do serviço gerido](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet) |
| .NET Core       | [Chamar os serviços do Azure a partir de uma VM do Linux com a identidade do serviço gerido](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/) |
| Node.js         | [Gerir recursos com a identidade do serviço gerido](https://azure.microsoft.com/resources/samples/resources-node-manage-resources-with-msi/) |
| Python          | [Utilizar o MSI para autenticar a partir de dentro de uma VM](https://azure.microsoft.com/resources/samples/resource-manager-python-manage-resources-with-msi/) |
| Ruby            | [Gerir recursos a partir de uma VM com capacidade de MSI](https://azure.microsoft.com/resources/samples/resources-ruby-manage-resources-with-msi/) |

## <a name="related-content"></a>Conteúdo relacionado

- Ver [SDKs do Azure](https://azure.microsoft.com/downloads/) para a lista completa de recursos do SDK do Azure, incluindo downloads de biblioteca, documentação e muito mais.
- Para ativar o MSI numa VM do Azure, consulte [configurar uma VM Managed Service Identity (MSI) com o portal do Azure](qs-configure-portal-windows-vm.md).

Utilize a seguinte secção de comentários para fornecer comentários e ajude-na refinar e moldar o nosso conteúdo.








