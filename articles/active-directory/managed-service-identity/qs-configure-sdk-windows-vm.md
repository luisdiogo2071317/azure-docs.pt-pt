---
title: Como configurar uma identidade do serviço gerido da VM do Azure com um SDK do Azure ativada
description: Passo a passo de instruções para configurar e utilizar uma identidade do serviço gerido na VM do Azure, com um SDK do Azure.
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
ms.date: 09/28/2017
ms.author: daveba
ms.openlocfilehash: 2763c78d309f5a90d68429caa46581e50f8b4303
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/26/2018
ms.locfileid: "39257665"
---
# <a name="configure-a-vm-managed-service-identity-using-an-azure-sdk"></a>Configurar uma identidade de serviço VM-Managed com um SDK do Azure

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Identidade de serviço gerida fornece serviços do Azure com uma identidade gerida automaticamente no Azure Active Directory (AD). Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter credenciais em seu código. 

Neste artigo, saiba como ativar e remover a identidade do serviço gerido para a VM do Azure, com um SDK do Azure.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="azure-sdks-with-managed-service-identity-support"></a>SDKs do Azure com suporte de identidade do serviço gerido 

O Azure suporta várias plataformas de programação através de uma série de [SDKs do Azure](https://azure.microsoft.com/downloads). Várias delas foram atualizadas para suportar a identidade do serviço gerido e fornecer exemplos de correspondentes para demonstrar a utilização. Esta lista é atualizada à medida que é adicionado suporte adicional:

| SDK | Sample |
| --- | ------ | 
| .NET   | [Gerir recursos a partir de uma VM com capacidade de MSI](https://azure.microsoft.com/resources/samples/aad-dotnet-manage-resources-from-vm-with-msi/) |
| Java   | [Gerir o armazenamento a partir de uma VM com capacidade de MSI](https://azure.microsoft.com/resources/samples/compute-java-manage-resources-from-vm-with-msi-in-aad-group/)|
| Node.js| [Criar uma VM com o MSI ativada](https://azure.microsoft.com/resources/samples/compute-node-msi-vm/) |
| Python | [Criar uma VM com o MSI ativada](https://azure.microsoft.com/resources/samples/compute-python-msi-vm/) |
| Ruby   | [Criar VM do Azure com um MSI](https://azure.microsoft.com/resources/samples/compute-ruby-msi-vm/) |

## <a name="next-steps"></a>Passos Seguintes

- Consulte artigos relacionados em "Configurar Managed Service Identity para VM do Azure", para saber como também pode utilizar os modelos de portal, PowerShell, CLI e recursos do Azure.

Utilize a seguinte secção de comentários para fornecer comentários e ajude-na refinar e moldar o nosso conteúdo.
