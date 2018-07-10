---
title: Como configurar habilitados no MSI VM do Azure com um SDK do Azure
description: Passo a passo de instruções para configurar e utilizar uma identidade de serviço gerida (MSI) na VM do Azure, com um SDK do Azure.
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
ms.openlocfilehash: dee4a3e27623150ce3fa648d73542db0cbb23e93
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/07/2018
ms.locfileid: "37901447"
---
# <a name="configure-a-vm-managed-service-identity-msi-using-an-azure-sdk"></a>Configurar um VM-Managed Service Identity (MSI) com um SDK do Azure

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Identidade de serviço gerida fornece serviços do Azure com uma identidade gerida automaticamente no Azure Active Directory (AD). Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter credenciais em seu código. 

Neste artigo, saiba como ativar e remover MSI de VM do Azure, com um SDK do Azure.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="azure-sdks-with-msi-support"></a>SDKs do Azure com suporte MSI 

O Azure suporta várias plataformas de programação através de uma série de [SDKs do Azure](https://azure.microsoft.com/downloads). Várias delas foram atualizadas para suportar o MSI e fornecer exemplos de correspondentes para demonstrar a utilização. Esta lista é atualizada à medida que é adicionado suporte adicional:

| SDK | Sample |
| --- | ------ | 
| .NET   | [Gerir recursos a partir de uma VM com capacidade de MSI](https://azure.microsoft.com/resources/samples/aad-dotnet-manage-resources-from-vm-with-msi/) |
| Java   | [Gerir o armazenamento a partir de uma VM com capacidade de MSI](https://azure.microsoft.com/resources/samples/compute-java-manage-resources-from-vm-with-msi-in-aad-group/)|
| Node.js| [Criar uma VM com o MSI ativada](https://azure.microsoft.com/resources/samples/compute-node-msi-vm/) |
| Python | [Criar uma VM com o MSI ativada](https://azure.microsoft.com/resources/samples/compute-python-msi-vm/) |
| Ruby   | [Criar VM do Azure com um MSI](https://azure.microsoft.com/resources/samples/compute-ruby-msi-vm/) |

## <a name="next-steps"></a>Passos Seguintes

- Consulte artigos relacionados em "Configurar MSI para uma VM do Azure", para saber como também pode utilizar os modelos de portal, PowerShell, CLI e recursos do Azure.

Utilize a seguinte secção de comentários para fornecer comentários e ajude-na refinar e moldar o nosso conteúdo.
