---
title: Gerir os dados de utilizador no Centro de segurança do Azure | Microsoft Docs
description: " Saiba como gerir os dados de utilizador no Centro de segurança do Azure. "
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2018
ms.author: terrylan
ms.openlocfilehash: 2495bae5c63cdafe049ec39f71ab53106c5f2df7
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655723"
---
# <a name="manage-user-data-in-azure-security-center"></a>Gerir os dados de utilizador no Centro de segurança do Azure
Este artigo fornece informações sobre como pode gerir os dados de utilizador no Centro de segurança do Azure. Gestão de dados de utilizador inclui a capacidade de aceder, eliminar ou exportar dados.

[!INCLUDE [gdpr-intro-sentence.md](../../includes/gdpr-intro-sentence.md)]

Um utilizador do Centro de segurança atribuída a função de leitor de proprietário, Contribuidor ou administrador de conta podem aceder a dados de cliente dentro da ferramenta. Consulte [funções incorporadas para controlo de acesso baseado em funções do Azure](../role-based-access-control/built-in-roles.md) para saber mais sobre as funções do leitor, proprietário e contribuinte. Consulte [os administradores da subscrição do Azure](../billing/billing-add-change-azure-subscription-administrator.md) para saber mais sobre a função de administrador de conta.

## <a name="searching-for-and-identifying-personal-data"></a>Procurar e identificar os dados pessoais
Um utilizador do Centro de segurança pode ver os respetivos dados pessoais através do portal do Azure. Centro de segurança só armazena detalhes de contacto de segurança, tais como endereços de e-mail e números de telefone. Consulte [fornecer detalhes de contacto de segurança no Centro de segurança do Azure](security-center-provide-security-contact-details.md) para obter mais informações.

No portal do Azure, um utilizador pode ver as configurações de IP permitidas utilizando o Centro de segurança apenas na funcionalidade de acesso VM de tempo. Veja [Gerir o acesso da máquina virtual através do just in time](security-center-just-in-time.md), para obter mais informações.

No portal do Azure, um utilizador pode ver os alertas de segurança fornecidos pelo centro de segurança, incluindo endereços IP e os detalhes do atacante. Consulte [gerir e responder a alertas de segurança no Centro de segurança do Azure](security-center-managing-and-responding-alerts.md) para obter mais informações.

## <a name="classifying-personal-data"></a>Classificar os dados pessoais
Não é necessário classificar os dados pessoais encontrados na funcionalidade de contacto de segurança do Centro de segurança. Os dados guardados são um endereço de e-mail (ou vários endereços de e-mail) e um número de telefone. [Contacte dados](security-center-provide-security-contact-details.md) é validado pelo centro de segurança.

Não é necessário classificar os endereços IP e guardados pelo centro de segurança de números de porta [apenas no tempo](security-center-just-in-time.md) funcionalidade.

Apenas um utilizador atribuída a função de administrador, pode classificar os dados pessoais por [visualizar alertas](security-center-managing-and-responding-alerts.md) no Centro de segurança.

## <a name="securing-and-controlling-access-to-personal-data"></a>Proteger e controlar o acesso a dados pessoais
Um utilizador do Centro de segurança atribuída a função de leitor de proprietário, Contribuidor ou administrador de conta podem aceder [dados de contacto de segurança](security-center-provide-security-contact-details.md).

Um utilizador do Centro de segurança atribuída a função de leitor de proprietário, Contribuidor ou administrador de conta pode aceder a respetiva [apenas no tempo](security-center-just-in-time.md) políticas.

Um utilizador do Centro de segurança atribuída a função de leitor de proprietário, Contribuidor ou conta de administrador pode ver os respetivos [alertas](security-center-managing-and-responding-alerts.md).

## <a name="updating-personal-data"></a>Atualizar dados pessoais
Um utilizador do Centro de segurança atribuída a função de proprietário, contribuinte, ou pode atualizar a conta de administrador [dados de contacto de segurança](security-center-provide-security-contact-details.md) através do portal do Azure.

Um utilizador do Centro de segurança atribuída a função de proprietário, contribuinte, ou o administrador de conta pode atualizar os respetivos [apenas nas políticas de tempo](security-center-just-in-time.md).

Um administrador de conta não é possível editar o alerta incidentes. Um [incidente de alerta](security-center-managing-and-responding-alerts.md) são considerados dados de segurança e é só de leitura.

## <a name="deleting-personal-data"></a>Eliminar dados pessoais
Um utilizador do Centro de segurança atribuída a função de proprietário, contribuinte, ou pode eliminar a conta de administrador [dados de contacto de segurança](security-center-provide-security-contact-details.md) através do portal do Azure.

Um utilizador do Centro de segurança atribuída a função de proprietário, contribuinte, ou pode eliminar a conta de administrador a [apenas nas políticas de tempo](security-center-just-in-time.md) através do portal do Azure.

Um utilizador do Centro de segurança não é possível eliminar a incidentes de alerta. Devido a necessidades de segurança, um [incidente de alerta](security-center-managing-and-responding-alerts.md) é considerado dados só de leitura.

## <a name="exporting-personal-data"></a>Exportar os dados pessoais
Um utilizador do Centro de segurança atribuída a função de leitor de proprietário, Contribuidor ou administrador de conta pode exportar [dados de contacto de segurança](security-center-provide-security-contact-details.md) por:

- Efetuar uma cópia do portal do Azure
- Executar a chamada de API REST do Azure, obter HTTP:
```HTTP
GET https://<endpoint>/subscriptions/{subscriptionId}/providers/Microsoft.Security/securityContacts?api-version={api-version}
```

Um utilizador de centro de segurança atribuído a função de administrador de conta pode exportar o [apenas nas políticas de tempo](security-center-just-in-time.md) que contém o IP endereços por:

- Efetuar uma cópia do portal do Azure
- Executar a chamada de API REST do Azure, obter HTTP:
```HTTP
GET https://<endpoint>/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Security/locations/{location}/jitNetworkAccessPolicies/default?api-version={api-version}
```

Um administrador de conta pode exportar os detalhes do alerta por:

- Efetuar uma cópia do portal do Azure
- Executar a chamada de API REST do Azure, obter HTTP:
```HTTP
GET https://<endpoint>/subscriptions/{subscriptionId}/providers/microsoft.Security/alerts?api-version={api-version}
```

Consulte [obter alertas de segurança (obter coleção)](https://msdn.microsoft.com/library/mt704050.aspx) para obter mais informações.

## <a name="restricting-the-use-of-personal-data-for-profiling-or-marketing-without-consent"></a>Restringir a utilização dos dados pessoais para a criação de perfis ou sem o consentimento de marketing
Um utilizador do Centro de segurança pode optar ativamente por eliminar os respetivos [dados de contacto de segurança](security-center-provide-security-contact-details.md).

[Apenas nos dados de tempo](security-center-just-in-time.md) é considerado dados não pessoais e é mantido durante um período de 30 dias.

[Dados de alerta](security-center-managing-and-responding-alerts.md) são considerados dados de segurança e é mantido durante um período de dois anos.

## <a name="auditing-and-reporting"></a>Auditoria e de relatórios
Registos de contacto de segurança de auditoria apenas no tempo e alertas de atualizações são mantidas no [registos de atividade do Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre a gestão de dados do utilizador, consulte [gerir os dados de utilizador encontrados numa investigação de centro de segurança do Azure](security-center-investigation-user-data.md).
