---
title: Auditoria e relatórios um utilizador de colaboração do Azure Active Directory B2B | Documentos da Microsoft
description: As propriedades do utilizador convidado são configuráveis em colaboração do Azure Active Directory B2B
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6fc36128f8f998d78dd2cf9ef112fe5961bbef5b
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56204615"
---
# <a name="auditing-and-reporting-a-b2b-collaboration-user"></a>Auditoria e relatórios um utilizador de colaboração B2B
Com os utilizadores convidados, tem capacidades semelhantes de auditoria com utilizadores membros. 

## <a name="access-reviews"></a>Revisões de acesso
Pode utilizar as revisões de acesso para verificar periodicamente se os utilizadores convidados ainda precisam de acesso aos seus recursos. O **as revisões de acesso** funcionalidade está disponível no **Azure Active Directory** sob **gerir** > **relações organizacionais**. (Também pode procurar "revisões de acesso" partir **todos os serviços** no portal do Azure.) Para saber como utilizar as revisões de acesso, veja [revisões de acesso de convidado de gerir com o acesso do Azure AD](../governance/manage-guest-access-with-access-reviews.md).

## <a name="audit-logs"></a>Registos de auditoria

A auditoria do Azure AD registos fornecem registos das atividades de sistema e do usuário, incluindo atividades iniciadas por utilizadores convidados. Para aceder aos registos de auditoria, no **do Azure Active Directory**, em **monitorização**, selecione **registos de auditoria**. Eis um exemplo do histórico de convite e resgate de convidado Sam Oogle:

![registo de Auditoria](./media/auditing-and-reporting/audit-log.png)

Pode se aprofundar em cada um desses eventos para obter os detalhes. Por exemplo, vamos examinar os detalhes de aceitação.

![Detalhes da atividade](./media/auditing-and-reporting/activity-details.png)

Também pode exportar estes registos do Azure AD e utilize a ferramenta de geração de relatórios da sua preferência para obter relatórios personalizados.

### <a name="next-steps"></a>Passos Seguintes

- [Propriedades do utilizador de colaboração B2B](user-properties.md)

