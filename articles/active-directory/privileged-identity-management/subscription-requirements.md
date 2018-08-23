---
title: Privilegiado subscrições da gestão de identidades - Azure | Documentos da Microsoft
description: Explica a subscrição e os requisitos para gerir e utilizar o Azure AD Privileged Identity Management no seu inquilino de licenciamento
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.assetid: 34367721-8b42-4fab-a443-a2e55cdbf33d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: pim
ms.date: 06/01/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: dbe4efa57a7c4c31b2bdca84665eef8179564a8f
ms.sourcegitcommit: a62cbb539c056fe9fcd5108d0b63487bd149d5c3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/22/2018
ms.locfileid: "42617074"
---
# <a name="azure-active-directory-privileged-identity-management-subscription-requirements"></a>Requisitos de subscrição do Azure Active Directory Privileged Identity Management

O Azure AD Privileged Identity Management está disponível como parte da edição Premium P2 do Azure AD. Para obter mais informações sobre os outros recursos de P2 e o compara ao Premium P1, consulte [edições do Azure Active Directory](../active-directory-editions.md).

>[!NOTE]
Quando o Azure Active Directory (Azure AD) Privileged Identity Management foi em pré-visualização, não havia nenhum verificações de licença para um inquilino experimentar o serviço.  Agora que o Azure AD Privileged Identity Management atingiu a disponibilidade geral, uma subscrição de avaliação ou paga tem de estar presente para o inquilino continuar a utilizar o Privileged Identity Management depois de Dezembro de 2016.
  

## <a name="confirm-your-trial-or-paid-subscription"></a>Confirmar a sua subscrição de avaliação ou paga

Se não tiver a certeza se a sua organização tem uma versão de avaliação ou adquirido a subscrição, em seguida, pode verificar se existe uma subscrição no seu inquilino, utilizando os comandos incluídos no Azure Active Directory módulo para o Windows PowerShell V1. 
1. Abra uma janela do PowerShell.
2. Introduza `Connect-MsolService` para se autenticar como um utilizador no seu inquilino.
3. Introduza `Get-MsolSubscription | ft SkuPartNumber,IsTrial,Status`.

Este comando obtém uma lista das subscrições no seu inquilino. Se não existirem não existem linhas devolvidas, terá de obter uma Azure AD Premium P2, avaliação, compra uma subscrição P2 do Azure AD Premium ou EMS E5 subscrição para utilizar o Azure AD Privileged Identity Management.  Para obter uma versão de avaliação e começar a utilizar o Azure AD Privileged Identity Management, leia [introdução ao Azure AD Privileged Identity Management](pim-getting-started.md).

Se este comando devolve uma linha na qual SkuPartNumber é "AAD_PREMIUM_P2" ou "EMSPREMIUM" e IsTrial for "True", isto indica que uma versão de avaliação do Azure AD Premium P2 está presente no inquilino.  Se o estado da subscrição não está ativado e não tiver uma subscrição P2 do Azure AD Premium ou EMS E5, comprar, em seguida, tem de comprar uma subscrição P2 do Azure AD Premium ou EMS E5 subscrição para continuar a utilizar o Azure AD Privileged Identity Management.

O Azure AD Premium P2 está disponível por meio de um [contrato Enterprise da Microsoft](https://www.microsoft.com/en-us/licensing/licensing-programs/enterprise.aspx), o [programa de licenciamento em Volume Open](https://www.microsoft.com/en-us/licensing/licensing-programs/open-license.aspx)e o [programa de fornecedores de soluções Cloud](https://partner.microsoft.com/cloud-solution-provider). Subscritores do Azure e do Office 365 também podem comprar o Azure AD Premium P2 online.  Obter mais informações sobre preços do Azure AD Premium e como encomendar online podem ser encontradas em [preços do Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="azure-ad-privileged-identity-management-is-not-available-in-tenant"></a>O Azure AD Privileged Identity Management não está disponível no inquilino

O Azure AD Privileged Identity Management já não estará disponível no seu inquilino se:
- Sua organização foi utilizar o Azure AD Privileged Identity Management quando ele foi em pré-visualização e não compra a subscrição do Azure AD Premium P2 ou subscrição do EMS E5.
- Sua organização tinha uma P2 do Azure AD Premium ou EMS E5 avaliação que expirou.
- Sua organização tinha uma subscrição adquirida que expirou.

Quando uma subscrição P2 do Azure AD Premium ou EMS E5 subscrição expira, ou uma organização que estava a utilizar o Azure AD Privileged Identity Management em pré-visualização não obter a subscrição do Azure AD Premium P2 ou EMS E5:

- Atribuições de função permanente para funções do Azure AD não serão afetadas.
- A extensão do Azure AD Privileged Identity Management no portal do Azure, como os cmdlets do Graph API e as interfaces de PowerShell do Azure AD Privileged Identity Management, deixará de estar disponível para os utilizadores ativarem funções com privilégios, gerir com privilégios aceder ou executar revisões de acesso de funções com privilégios.
- Serão removidas as atribuições de função elegível de funções do Azure AD, como os utilizadores já não poderão ativar as funções com privilégios.
- Quaisquer revisões de acesso contínuo de funções do Azure AD irão terminar e as definições de configuração do Azure AD Privileged Identity Management serão removidas.
- O Azure AD Privileged Identity Management já não irá enviar e-mails sobre as alterações de atribuição de função.

## <a name="next-steps"></a>Passos Seguintes

- [Introdução ao Azure AD Privileged Identity Management](pim-getting-started.md)
- [Funções no Azure AD Privileged Identity Management](pim-roles.md)
