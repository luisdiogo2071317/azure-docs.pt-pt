---
title: Assistente de segurança do AD funções do Azure no PIM | Documentos da Microsoft
description: Descreve o Assistente de segurança que pode utilizar para converter permanente privilegiado atribuições de funções do Azure AD para elegíveis com o Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 11/09/2018
ms.author: rolyon
ms.custom: pim ; H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: b66dcc834ab089f28e6bb47c2905e6a2a3907091
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56208763"
---
# <a name="azure-ad-roles-security-wizard-in-pim"></a>Assistente de segurança do AD funções do Azure no PIM

Se for a primeira pessoa a executar o Azure AD Privileged Identity Management (PIM) para a sua organização, será apresentado um assistente. O assistente ajuda-o a compreender os riscos de segurança de identidades privilegiadas e como utilizar o PIM para reduzir esses riscos. Não precisa de fazer alterações às atribuições de funções existente no assistente, se prefira fazê-lo mais tarde.

## <a name="wizard-overview"></a>Descrição geral do Assistente

Antes de sua organização começa a utilizar o PIM, todas as atribuições de função são permanentes: os utilizadores são sempre nessas funções, mesmo que eles não precisam atualmente seus privilégios. A primeira etapa do assistente mostra uma lista de funções de privilégios elevados e quantos utilizadores estão atualmente nessas funções. Pode explorar a uma função específica para saber mais sobre os utilizadores se um ou mais deles não estão familiarizados.

A segunda etapa do assistente fornece uma oportunidade para alterar as atribuições de funções do administrador.  

> [!WARNING]
> É importante que tem, pelo menos, um Administrador Global e mais do que um administrador com função privilegiada com uma conta institucional (não uma conta Microsoft). Se houver apenas um administrador com função privilegiada, a organização não será capaz de gerir PIM se essa conta é eliminada.
> Além disso, mantenha as atribuições de funções permanente se um utilizador tiver uma conta Microsoft (uma conta que utilizam para iniciar sessão para serviços Microsoft como o Skype e o Outlook.com). Se quiser exigir a MFA para ativação para essa função, que o utilizador será bloqueado.

## <a name="run-the-wizard"></a>Executar o assistente

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).

1. Open **do Azure AD Privileged Identity Management**.

1. Clique em **funções do Azure AD** e, em seguida, clique em **assistente**.

    ![Funções do Azure AD - Assistente](./media/pim-security-wizard/wizard-start.png)

1. Clique em **1 deteção de funções privilegiadas**.

1. Reveja a lista de funções com privilégios para ver a quais usuários são permanentes ou elegíveis.

    ![Detetar utilizadores de funções privilegiadas](./media/pim-security-wizard/discover-privileged-roles-users.png)

1. Clique em **seguinte** para selecionar os membros que pretende tornar elegíveis.

    ![Converter membros para elegíveis](./media/pim-security-wizard/convert-members-eligible.png)

1. Assim que tiver selecionado os membros, clique em **seguinte**.

    ![Rever alterações](./media/pim-security-wizard/review-changes.png)

1. Clique em **OK** para converter as atribuições de permanentes para elegíveis.

    Quando a conversão for concluída, verá uma notificação.

    ![Notificações](./media/pim-security-wizard/notification-completion.png)

Se precisar de converter outras atribuições de funções privilegiadas elegível, pode executar o assistente novamente. Se quiser usar a interface PIM em vez do assistente, consulte [atribuir funções de diretório do Azure AD no PIM](pim-how-to-add-role-to-user.md).

## <a name="next-steps"></a>Passos Seguintes

- [Atribuir funções de diretório do Azure AD no PIM](pim-how-to-add-role-to-user.md)
- [Conceder acesso a outros administradores para gerir o PIM](pim-how-to-give-access-to-pim.md)
