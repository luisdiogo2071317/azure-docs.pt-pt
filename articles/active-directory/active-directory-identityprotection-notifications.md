---
title: "Notificações de proteção de identidade do Active Directory do Azure | Microsoft Docs"
description: "Saiba como notificações suportam as atividades de investigação."
services: active-directory
keywords: "proteção de identidade do Azure Active Directory, o cloud app discovery, gestão de aplicações, segurança, risco, nível de risco, vulnerabilidade, política de segurança"
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 65ca79b9-4da1-4d5b-bebd-eda776cc32c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: bea21439afef4fda453732edffc84c62667dfe38
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-identity-protection-notifications"></a>Notificações de proteção de identidade do Active Directory do Azure

Azure AD Identity Protection envia dois tipos de e-mails de notificação automatizada para o ajudar a gerir o risco de utilizador e os eventos de risco:

- Os utilizadores em risco detetados e-mail
- E-mail de texto implícita semanal

Este artigo fornece uma descrição geral de ambos os e-mails de notificação.


## <a name="users-at-risk-detected-email"></a>Os utilizadores em risco detetados e-mail

Em resposta a uma conta detetada em risco, o Azure AD Identity Protection gera um alerta de e-mail com **utilizadores em risco detetados** como requerente. Mensagem de correio eletrónico inclui uma ligação para o  **[utilizadores sinalizados para risco](active-directory-reporting-security-user-at-risk.md)**  relatório. Como melhor prática, deve investigar imediatamente os utilizadores em risco.

![Os utilizadores em risco detetados e-mail](./media/active-directory-identityprotection-notifications/01.png)


### <a name="configuration"></a>Configuração

Como administrador, pode definir:

- **O nível de risco que aciona a geração deste e-mail** -por predefinição, o nível de risco é definido como "Alto" risco.
- **Os destinatários deste e-mail** -por predefinição, os destinatários incluem todos os administradores globais. Administradores globais também podem adicionar outros leitores de segurança de administradores globais, administradores de segurança, como os destinatários.  


Para abrir a caixa de diálogo relacionada, clique em **alertas** no **definições** secção o **Identity Protection** página.

![Os utilizadores em risco detetados e-mail](./media/active-directory-identityprotection-notifications/05.png)


## <a name="weekly-digest-email"></a>E-mail de texto implícita semanal

O e-mail de texto implícita semanal contém um resumo dos novos eventos de risco.  
Inclui:

- Utilizadores em risco

- Atividades suspeitas

- Vulnerabilidades detetadas

- Ligações para os relatórios relacionados no Identity Protection

    ![Remediação](./media/active-directory-identityprotection-notifications/400.png "remediação")

### <a name="configuration"></a>Configuração

Como administrador, pode mudar a enviar um e-mail de texto implícita semanal.

![Riscos de utilizador](./media/active-directory-identityprotection-notifications/62.png "riscos de utilizador")

Para abrir a caixa de diálogo relacionada, clique em **resumida semanal** no **definições** secção o **Identity Protection** página.

![Os utilizadores em risco detetados e-mail](./media/active-directory-identityprotection-notifications/04.png)


## <a name="see-also"></a>Consultar também

- [Proteção de identidade do Azure Active Directory](active-directory-identityprotection.md)
