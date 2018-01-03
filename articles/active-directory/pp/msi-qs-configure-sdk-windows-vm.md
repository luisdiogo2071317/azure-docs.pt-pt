---
title: "Como configurar um MSI atribuído por utilizador para uma VM do Azure utilizando um SDK do Azure"
description: "Passo pelo passo as instruções para configurar um utilizador atribuído geridos serviço de identidade (MSI) para uma VM do Azure, utilizando um SDK do Azure."
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
ms.date: 12/22/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 19b6179803b8de4102818c1522b00e6b4881d0f0
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/22/2017
---
# <a name="configure-a-user-assigned-managed-service-identity-msi-for-a-vm-using-an-azure-sdk"></a>Configurar um utilizador atribuído geridos serviço de identidade (MSI) para uma VM, utilizando um SDK do Azure

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Identidade de serviço geridas fornece serviços do Azure com uma identidade gerido no Azure Active Directory. Pode utilizar esta identidade para se autenticarem em serviços que suportam a autenticação do Azure AD, sem necessitar de credenciais no seu código. 

Neste artigo, irá aprender a ativar e remover um MSI atribuído por utilizador para uma VM do Azure, utilizando um SDK do Azure.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

## <a name="azure-sdks-with-msi-support"></a>SDKs do Azure com suporte MSI 

Azure suporta várias plataformas de programação através de uma série de [Azure SDKs](https://azure.microsoft.com/downloads). Muitos dos mesmos foram atualizados para suportar o MSI e fornecem exemplos correspondentes para demonstrar a utilização. Esta lista é atualizada como obter suporte adicional for adicionado:

| SDK | Sample |
| --- | ------ | 
| Python | [Criar uma VM com MSI ativada](https://azure.microsoft.com/resources/samples/compute-python-msi-vm/) |
| Ruby   | [Criar a VM do Azure com um MSI](https://azure.microsoft.com/resources/samples/compute-ruby-msi-vm/) |

## <a name="next-steps"></a>Passos Seguintes

- Consulte os artigos relacionados em "Configurar MSI para uma VM do Azure", para saber como pode configurar um MSI utilizador atribuído numa VM do Azure.

Utilize a seguinte secção de comentários para fornecer comentários e ajudam-nos refinar e formam o nosso conteúdo.
