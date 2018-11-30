---
title: 'Azure AD Connect: Instâncias do serviço de sincronização | Documentos da Microsoft'
description: Esta página documenta considerações especiais sobre instâncias do Azure AD.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: f340ea11-8ff5-4ae6-b09d-e939c76355a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: d086b958ddca6caded19cc02a790f8091aba993e
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/27/2018
ms.locfileid: "52425187"
---
# <a name="azure-ad-connect-special-considerations-for-instances"></a>O Azure AD Connect: Considerações especiais sobre instâncias
O Azure AD Connect é geralmente utilizado com a instância de nível mundial do Azure AD e o Office 365. Mas também existem outras instâncias e estes têm diferentes requisitos para URLs e outras considerações especiais.

## <a name="microsoft-cloud-germany"></a>Microsoft Cloud Alemanha
O [Microsoft Cloud Alemanha](https://www.microsoft.de/cloud-deutschland) é uma cloud soberana, operada por um dados na Alemanha.

| URLs para abrir o servidor proxy |
| --- |
| \*.microsoftonline.de |
| \*.windows.net |
| + Listas de revogação de certificados |

Quando inicia sessão no inquilino do Azure AD, tem de utilizar uma conta no domínio onmicrosoft.de.

Recursos atualmente não estão presentes na Microsoft Cloud Alemanha:

* **Repetição de escrita de palavra-passe** está disponível para pré-visualização do Azure AD Connect versão 1.1.570.0 e depois.
* Outros serviços do Azure AD Premium não estão disponíveis.

## <a name="microsoft-azure-government-cloud"></a>Cloud do Microsoft Azure Government
O [cloud do Microsoft Azure Government](https://azure.microsoft.com/features/gov/) é uma cloud para administração pública dos EUA.

Esta nuvem tem sido suportada por versões anteriores do DirSync. A partir de compilação 1.1.180 do Azure AD Connect, a próxima geração da cloud é suportada. Esta geração está a utilizar pontos de extremidade com base apenas dos EUA e ter uma lista diferente de URLs para abrir no seu servidor proxy.

| URLs para abrir o servidor proxy |
| --- |
| \*.microsoftonline.com |
| \*.microsoftonline.us |
| \*. windows.net (necessário para automático deteção de inquilino do Governo do Azure AD) |
| \*.gov.us.microsoftonline.com |
| + Listas de revogação de certificados |

> [!NOTE]
> A partir da versão de AAD Connect 1.1.647.0, definindo o valor de AzureInstance no registo já não é necessário fornecido que *. windows.net está aberta no seu servidor ou servidores de proxy.

Recursos atualmente não estão presentes na cloud do Microsoft Azure Government:

* **Repetição de escrita de palavra-passe** está disponível para pré-visualização do Azure AD Connect versão 1.1.570.0 e depois.
* Outros serviços do Azure AD Premium não estão disponíveis.

## <a name="next-steps"></a>Passos Seguintes
Saiba mais sobre como [Integrar as identidades no local ao Azure Active Directory](whatis-hybrid-identity.md).
