---
title: E-mail de notificações no PIM - Azure | Documentos da Microsoft
description: Descreve as notificações por e-mail no Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: pim
ms.date: 09/07/2018
ms.author: rolyon
ms.reviewer: hanki
ms.custom: pim
ms.openlocfilehash: de1d29d3ab1b370257c3a2d6b6ff9f677197fc2a
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/10/2018
ms.locfileid: "44303069"
---
# <a name="email-notifications-in-pim"></a>Notificações por e-mail no PIM

Quando ocorrem eventos chave no Azure AD Privileged Identity Management (PIM), são enviadas notificações de e-mail. Por exemplo, o PIM envia e-mails para os seguintes eventos:

- Quando uma ativação de função com privilégios está com aprovação pendente
- Quando um pedido de ativação de função com privilégios é concluído
- Quando uma função com privilégios é ativada
- Quando é atribuída uma função com privilégios
- Quando está ativado o Azure AD PIM

Notificações por e-mail são enviadas para os administradores do seguintes:

- Administrador de Função com Privilégios 
- Administrador de Segurança

Notificações de e-mail também são enviadas para o utilizador final que tenha a função com privilégios para os seguintes eventos:

- Quando um pedido de ativação de função com privilégios é concluído
- Quando é atribuída uma função com privilégios

Começando no final de Julho de 2018, notificações por e-mail enviadas por meio do PIM tem um novo endereço de e-mail do remetente e um novo design visual. Esta atualização irá afetar os dois PIM do Azure AD e o PIM para recursos do Azure. Todos os eventos que foram acionados anteriormente uma notificação por e-mail irão continuar a enviar um e-mail. Conteúdo de ter atualizado por alguns emails fornecendo informações mais específicas.

## <a name="sender-email-address"></a>Endereço de e-mail do remetente

Começando no final de Julho de 2018, as notificações de e-mail tem o seguinte endereço:

- Endereço de e-mail:  **azure-noreply@microsoft.com**
- Nome a apresentar: Microsoft Azure

Notificações por e-mail tinham anteriormente, o seguinte endereço:

- Endereço de e-mail:  **azureadnotifications@microsoft.com**
- Nome a apresentar: o serviço de notificação do Microsoft Azure AD

## <a name="email-subject-line"></a>Linha de assunto do e-mail

Começando no final de Julho de 2018, notificações por e-mail para ambas do Azure AD e funções de recursos do Azure terão uma **PIM** prefixo na linha de assunto. Segue-se um exemplo:

- PIM: Alain Charon permanentemente foi atribuída a função de leitor de cópia de segurança.

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

- [Configurar definições de função de diretório do Azure AD no PIM](pim-how-to-change-default-settings.md)
- [Aprovar ou recusar pedidos para funções de diretório do Azure AD no PIM](azure-ad-pim-approval-workflow.md)
