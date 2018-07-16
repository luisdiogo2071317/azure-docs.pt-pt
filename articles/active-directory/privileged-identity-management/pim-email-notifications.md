---
title: Notificações no Azure AD PIM por e-mail | Documentos da Microsoft
description: Descreve as notificações por e-mail no Azure AD Privileged Identity Management (PIM)
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: protection
ms.date: 07/14/2018
ms.author: rolyon
ms.reviewer: hanki
ms.custom: pim
ms.openlocfilehash: 6c329554b5854f113fb216f874fa5a918110f9c5
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39059782"
---
# <a name="email-notifications-in-azure-ad-pim"></a>Notificações por e-mail no Azure AD PIM

Quando ocorrem eventos chave no Azure AD Privileged Identity Management (PIM), notificações por e-mail são enviadas para o utilizador ou administrador relevante. Por exemplo, o PIM envia e-mails para os seguintes eventos:

- Quando uma ativação de função com privilégios está com aprovação pendente
- Quando um pedido de ativação de função com privilégios é aprovado
- Quando uma função com privilégios é ativada
- Quando é atribuída uma função com privilégios
- Quando está ativado o Azure AD PIM

Começando no final de Julho de 2018, notificações por e-mail enviadas por meio do PIM terá um novo endereço de e-mail do remetente e um novo design visual. Esta atualização irá afetar os dois PIM do Azure AD e o PIM para recursos do Azure. Todos os eventos que foram acionados anteriormente uma notificação por e-mail irão continuar a enviar um e-mail. Esta atualização é apenas uma alteração visual sem alterações à funcionalidade.

## <a name="sender-email-address"></a>Endereço de e-mail do remetente

Começando no final de Julho de 2018, as notificações de e-mail tem o seguinte endereço:

- Endereço de e-mail:  **azure-noreply@microsoft.com**
- Nome a apresentar: Microsoft Azure

Notificações por e-mail tinham anteriormente, o seguinte endereço:

- Endereço de e-mail:  **azureadnotifications@microsoft.com**
- Nome a apresentar: o serviço de notificação do Microsoft Azure AD

## <a name="pim-emails-for-azure-ad-roles"></a>E-mails PIM para funções do Azure AD

Começando no final de Julho de 2018, as notificações de e-mail do PIM para funções do Azure AD tem um design atualizado. O código a seguir mostra uma mensagem de e-mail de exemplo que é enviada quando um utilizador ativa a uma função com privilégios para a organização Contoso fictícia.

![Novo e-mail do PIM para funções do Azure AD](./media/pim-email-notifications/email-directory-new.png)

Anteriormente, quando um utilizador ativado uma função com privilégios, viu a mensagem de e-mail semelhante ao seguinte.

![E-mail antigo do PIM para funções do Azure AD](./media/pim-email-notifications/email-directory-old.png)

## <a name="pim-emails-for-azure-resource-roles"></a>E-mails PIM para funções de recursos do Azure

Começando no final de Julho de 2018, as notificações de e-mail do PIM para funções de recursos do Azure tem um design atualizado. O código a seguir mostra uma mensagem de e-mail de exemplo que é enviada quando um utilizador é atribuído uma função com privilégios para a organização Contoso fictícia.

![Novo e-mail do PIM para funções de recursos do Azure](./media/pim-email-notifications/email-resources-new.png)

Anteriormente, quando um utilizador foi atribuído uma função com privilégios, viu a mensagem de e-mail semelhante ao seguinte.

![E-mail antigo do PIM para funções de recursos do Azure](./media/pim-email-notifications/email-resources-old.png)

## <a name="next-steps"></a>Passos Seguintes

- [Como gerir as definições de ativação de função no Azure AD PIM](pim-how-to-change-default-settings.md)
- [Aprovações no Azure AD PIM](azure-ad-pim-approval-workflow.md)
