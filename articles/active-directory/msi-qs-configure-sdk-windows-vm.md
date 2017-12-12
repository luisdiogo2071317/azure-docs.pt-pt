---
title: Como configurar preparados MSI VM do Azure utilizando um SDK do Azure
description: "Passo por instruções passo para configurar e utilizar uma identidade de serviço geridas (MSI) na VM do Azure, utilizando um SDK do Azure."
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/28/2017
ms.author: bryanla
ms.openlocfilehash: d5fb9decd1417a51c85f23d6b178c8a86cf292bd
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2017
---
# <a name="configure-a-vm-managed-service-identity-msi-using-an-azure-sdk"></a>Configurar uma VM geridos serviço de identidade (MSI) utilizando um SDK do Azure

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Identidade de serviço geridas fornece serviços do Azure com uma identidade gerido automaticamente no Azure Active Directory (AD). Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter de credenciais no seu código. 

Neste artigo, irá aprender a ativar e remover MSI para uma VM do Azure, utilizando um SDK do Azure.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="azure-sdks-with-msi-support"></a>SDKs do Azure com suporte MSI 

Azure suporta várias plataformas de programação através de uma série de [Azure SDKs](https://azure.microsoft.com/downloads). Muitos dos mesmos foram atualizados para suportar o MSI e fornecem exemplos correspondentes para demonstrar a utilização. Esta lista é atualizada como obter suporte adicional for adicionado:

| SDK | Sample |
| --- | ------ | 
| .NET   | [Gerir recursos a partir de uma VM MSI-ativado](https://azure.microsoft.com/resources/samples/aad-dotnet-manage-resources-from-vm-with-msi/) |
| Java   | [Gerir o armazenamento a partir de uma VM MSI-ativado](https://azure.microsoft.com/resources/samples/compute-java-manage-resources-from-vm-with-msi-in-aad-group/)|
| Node.js| [Criar uma VM com MSI ativada](https://azure.microsoft.com/resources/samples/compute-node-msi-vm/) |
| Python | [Criar uma VM com MSI ativada](https://azure.microsoft.com/resources/samples/compute-python-msi-vm/) |
| Ruby   | [Criar a VM do Azure com um MSI](https://azure.microsoft.com/resources/samples/compute-ruby-msi-vm/) |

## <a name="next-steps"></a>Passos seguintes

- Consulte os artigos relacionados em "Configurar MSI para uma VM do Azure", para saber como também pode utilizar os modelos de recurso, CLI, PowerShell e portal do Azure.

Utilize a seguinte secção de comentários para fornecer comentários e ajudam-nos refinar e formam o nosso conteúdo.
