---
title: "Como utilizar um utilizador atribuído geridos serviço identidade da SDKs do Azure numa VM"
description: "Exemplos de código para utilizar os SDKs do Azure com um MSI utilizador atribuído numa VM."
services: active-directory
documentationcenter: 
author: BryanLa
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/22/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: f9a31a0500a6f5f1c49fc45d5811e28788e6f2b1
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/22/2017
---
# <a name="use-azure-sdks-with-a-user-assigned-managed-service-identity-msi"></a>Utilize SDKs do Azure com um utilizador atribuído geridos serviço de identidade (MSI)

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]Este artigo fornece uma lista de amostras SDK, que demonstram a utilização de suporte dos seus respetivos Azure do SDK para o utilizador atribuído MSI.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

> [!IMPORTANT]
> - Todos os código/script de exemplo neste artigo assume que o cliente está em execução numa máquina Virtual ativada de MSI. Utilize a funcionalidade VM "Ligar" no portal do Azure, para ligar remotamente à VM. Para obter mais informações sobre como ativar MSI numa VM, consulte [configurar uma VM geridos serviço de identidade (MSI) utilizando a CLI do Azure](msi-qs-configure-cli-windows-vm.md), ou um dos artigos variantes (utilizando o PowerShell, Portal do Azure, um modelo ou um SDK do Azure). 

## <a name="sdk-code-samples"></a>Exemplos de código do SDK

| SDK             | Exemplo de código |
| --------------- | ----------- |
| Python          | [Utilizar MSI para autenticar simplesmente de dentro de uma VM](https://azure.microsoft.com/resources/samples/resource-manager-python-manage-resources-with-msi/) |
| Ruby            | [Gerir recursos a partir de uma VM MSI-ativado](https://azure.microsoft.com/resources/samples/resources-ruby-manage-resources-with-msi/) |

## <a name="next-steps"></a>Passos Seguintes

- Consulte [Azure SDKs](https://azure.microsoft.com/downloads/) para a lista completa dos recursos do SDK do Azure, incluindo as transferências de biblioteca, documentação e muito mais.

Utilize a seguinte secção de comentários para fornecer comentários e ajudam-nos refinar e formam o nosso conteúdo.








