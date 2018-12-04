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
ms.date: 11/30/2018
ms.author: rolyon
ms.reviewer: hanki
ms.custom: pim
ms.openlocfilehash: 00b096f59e70962b6883a8024744e8c91a5f9ae3
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52846900"
---
# <a name="email-notifications-in-pim"></a>Notificações por e-mail no PIM

O Azure AD Privileged Identity Management (PIM) permite-lhe saber quando ocorrem eventos importantes, como quando uma função é atribuída ou ativada. PIM mantém-se informado com o envio de que notificações de e-mail do utilizador e outros participantes. Estes e-mails também podem incluir ligações para tarefas relevantes, tais ativando ou renovar uma função. Este artigo descreve a aparência estes e-mails, quando são enviados e quem receberá-los.

## <a name="sender-email-address-and-subject-line"></a>Linha de endereço e o assunto de e-mail do remetente

E-mails enviados do PIM do Azure AD e funções de recursos do Azure têm o seguinte endereço de e-mail do remetente:

- Endereço de e-mail:  **azure-noreply@microsoft.com**
- Nome a apresentar: Microsoft Azure

Estes e-mails incluem uma **PIM** prefixo na linha de assunto. Segue-se um exemplo:

- PIM: Alain Charon permanentemente foi atribuída a função de leitor de cópia de segurança

## <a name="pim-emails-for-azure-ad-roles"></a>E-mails PIM para funções do Azure AD

PIM envia mensagens de correio eletrónico quando ocorrem os seguintes eventos para funções do Azure AD:

- Quando uma ativação de função com privilégios está com aprovação pendente
- Quando um pedido de ativação de função com privilégios é concluído
- Quando uma função com privilégios é atribuída como elegíveis
- Quando está ativado o Azure AD PIM

Quem receberá estes e-mails para funções do Azure AD depende de sua função, o evento e as notificações de definição:

| Utilizador | Ativação de função está com aprovação pendente | Conclusão do pedido de ativação de função | Função é atribuída elegíveis | PIM está ativada |
| --- | --- | --- | --- | --- |
| Administrador de Função com Privilégios </br>(Ativado/elegíveis) | Sim</br>(apenas se nenhum aprovadores explícitos são especificados) | Sim* | Sim | Sim |
| Administrador de Segurança</br>(Ativado/elegíveis) | Não | Sim* | Sim | Sim |
| Administrador Global</br>(Ativado/elegíveis) | Não | Sim* | Sim | Sim |

\* Se o [ **notificações** definição](pim-how-to-change-default-settings.md#notifications) está definida como **ativar**.

O código a seguir mostra uma mensagem de e-mail de exemplo que é enviada quando um utilizador ativa a uma função do Azure AD para a organização Contoso fictícia.

![Novo e-mail do PIM para funções do Azure AD](./media/pim-email-notifications/email-directory-new.png)

### <a name="weekly-pim-digest-email-for-azure-ad-roles"></a>E-mail de resumo semanal PIM para funções do Azure AD

Um e-mail de resumo de semanal PIM para funções do Azure AD é enviado para os administradores de função com privilégios, os administradores de segurança e os administradores globais que tiver ativado o PIM. Este e-mail semanal fornece um instantâneo de atividades do PIM para a semana, bem como as atribuições de funções privilegiadas. Só está disponível para os inquilinos na cloud pública. Eis um e-mail de exemplo:

![E-mail de resumo semanal PIM para funções do Azure AD](./media/pim-email-notifications/email-directory-weekly.png)

O e-mail inclui quatro mosaicos:

| mosaico | Descrição |
| --- | --- |
| **Utilizadores ativados** | Número de vezes que os utilizadores ativado a respetiva função elegível dentro do inquilino. |
| **Tornar-se permanentes de usuários** | Número de vezes que os utilizadores com uma atribuição elegível é tornar-se permanentes. |
| **Atribuições de funções no PIM** | Número de vezes que os utilizadores são atribuídos uma função elegível dentro do PIM. |
| **Atribuições de funções fora do PIM** | Número de vezes que os utilizadores são atribuídos uma função permanente fora do PIM (dentro do Azure AD). |

O **descrição geral das suas funções principais** seção lista top cinco funções no seu inquilino com base no número total de administradores elegíveis e permanentes para cada função. O **agir** abre-se de vincular a [assistente PIM](pim-security-wizard.md) onde pode converter os administradores permanentes para os administradores elegíveis em lotes.

## <a name="pim-emails-for-azure-resource-roles"></a>E-mails PIM para funções de recursos do Azure

PIM envia mensagens de correio eletrónico para os titulares e administradores de acesso do utilizador quando ocorrem os seguintes eventos para funções de recursos do Azure:

- Quando uma atribuição de função está com aprovação pendente
- Quando uma função é atribuída
- Quando uma função estiver prestes a expirar
- Quando uma função é elegível para expandir
- Quando uma função está a ser renovada por um usuário final
- Quando um pedido de ativação de função está concluído

PIM envia mensagens de correio eletrónico aos utilizadores finais quando ocorrem os seguintes eventos para funções de recursos do Azure:

- Quando uma função é atribuída ao utilizador
- Quando a função de um usuário está expirada
- Quando a função de um utilizador é expandida
- Quando o pedido de ativação de função de um utilizador está concluído

O código a seguir mostra uma mensagem de e-mail de exemplo que é enviada quando um utilizador seja atribuído uma função de recursos do Azure para a organização Contoso fictícia.

![Novo e-mail do PIM para funções de recursos do Azure](./media/pim-email-notifications/email-resources-new.png)

## <a name="next-steps"></a>Passos Seguintes

- [Configurar definições de função de diretório do Azure AD no PIM](pim-how-to-change-default-settings.md)
- [Aprovar ou recusar pedidos para funções de diretório do Azure AD no PIM](azure-ad-pim-approval-workflow.md)
