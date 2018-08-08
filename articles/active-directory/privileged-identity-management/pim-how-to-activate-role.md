---
title: Como ativar ou desativar uma função | Documentos da Microsoft
description: Aprenda a ativar as funções para as identidades privilegiadas com a aplicação do Azure Privileged Identity Management.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 02/14/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 33c41becb8ed741b7db5e3f89d193ca2ae7b6390
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39617137"
---
# <a name="how-to-activate-or-deactivate-roles-in-azure-ad-privileged-identity-management"></a>Como ativar ou desativar funções no Azure AD Privileged Identity Management
O Azure Active Directory (AD) Privileged Identity Management simplifica como as empresas a gerir o acesso privilegiado a recursos no Azure AD e outros serviços online da Microsoft, como o Office 365 ou o Microsoft Intune.  

Se tiver sido efetuado elegível para uma função administrativa, o que significa que pode ativar essa função, quando precisa efetuar ações privilegiadas. Por exemplo, se ocasionalmente gerir recursos do Office 365, os administradores de função com privilégios da sua organização podem não fazer é um Administrador Global permanente, uma vez que essa função tem impacto sobre outros serviços, também. Em vez disso, eles fazem-no elegível para funções do Azure AD, como o administrador do Exchange Online. Pode pedir para ativar essa função, quando precisar de seus privilégios, e, em seguida, terá controlo de administração durante um período de tempo predeterminado.

Este artigo destina-se a administradores que precisam de ativar a respetiva função no Azure AD Privileged Identity Management (PIM). Ele explica os passos para ativar uma função, quando necessita das permissões e desativar a função quando tiver terminado. Além disso, os administradores de função com privilégios podem exigir aprovação para ativar uma função (pré-visualização). Saiba mais sobre [fluxos de trabalho de aprovação de PIM](./azure-ad-pim-approval-workflow.md) aqui.

## <a name="add-the-privileged-identity-management-application"></a>Adicionar a aplicação Privileged Identity Management
Utilizar a aplicação do Azure AD Privileged Identity Management no [portal do Azure](https://portal.azure.com/) para pedir a ativação de função, mesmo que vai operar no outro portal ou PowerShell. Se não tiver a aplicação Azure AD Privileged Identity Management no portal do Azure, siga estes passos para começar a utilizar.

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).
2. Selecione o nome de utilizador no canto superior direito do portal do Azure e selecione o diretório em que irá ser operacional.
3. Selecione **Todos os serviços** e utilize a caixa de texto Filtro para procurar **Azure AD Privileged Identity Management**.
4. Marque **Afixar ao dashboard** e, em seguida, clique em **Criar**. A aplicação Privileged Identity Management abre.

## <a name="activate-a-role"></a>Ativar uma função
Quando tiver de desempenhar uma função, pode pedir a ativação ao selecionar o **minhas funções** opção de navegação na aplicação do Azure AD Privileged Identity Management coluna de navegação esquerdo.

1. Inicie sessão para o [portal do Azure](https://portal.azure.com/) e selecione o mosaico do Azure AD Privileged Identity Management.
2. Selecione **minhas funções**. Uma lista das suas funções elegíveis atribuídas são apresentados no agrupamento na parte superior da página.
3. Selecione uma função que pretende ativar.
4. Selecione **ativar**. O **pedir ativação da função** é apresentado o painel.
5. Algumas funções exigem multi-factor Authentication (MFA) para ativar a função. Só precisa que autenticar uma vez por sessão.
   
    ![Certifique-se com a MFA antes da ativação de função - captura de ecrã](./media/pim-how-to-activate-role/PIM_activation_MFA.png)
6. Introduza o motivo para o pedido de ativação no campo de texto.  Algumas funções exigem que forneça um número de pedido de suporte de problemas.
7. Selecione **OK**.  Se a função não necessita de aprovação, está agora ativado e a função é apresentada na lista de funções ativas (diretamente abaixo da lista de atribuições de funções elegíveis). Se o [a função requer aprovação](./azure-ad-pim-approval-workflow.md) para ativar, uma notificação de alerta resumidamente aparecerá no canto superior direito do seu navegador informando o pedido está com aprovação pendente.

    ![Pedido pendente notificação - captura de ecrã](./media/pim-how-to-activate-role/PIM_Request_Pending_Toast2.png)

## <a name="deactivate-a-role"></a>Desativar uma função
Depois de uma função, automaticamente desativa quando for atingido o limite de tempo (duração elegível).

Se concluir as tarefas administrativas no início, que também pode desativar uma função manualmente na aplicação do Azure AD Privileged Identity Management.  Selecione **funções do meu**, escolha a função já está através do **atribuições de funções ativas** agrupamentos e selecione **desativar**.  

## <a name="cancel-a-pending-request"></a>Cancelar um pedido pendente
No caso de não necessitar de ativação de uma função que requer aprovação, pode cancelar um pedido pendente em qualquer altura. Basta selecionar o **minhas funções** opção de navegação na aplicação do Azure AD Privileged Identity Management coluna de navegação esquerdo.

1. Inicie sessão para o [portal do Azure](https://portal.azure.com/) e selecione o mosaico do Azure AD Privileged Identity Management.
2. Selecione **minhas funções**. Uma lista das suas funções elegíveis atribuídas são apresentados no agrupamento na parte superior da página.
3. Selecione uma função.
4. Selecione o **ativação está com aprovação pendente** faixa no painel de detalhes de ativação de função.
5. Selecione **Cancelar** na parte superior a **com aprovação pendente** painel.

   ![Cancelar pedido pendente captura de ecrã](./media/pim-how-to-activate-role/PIM_Request_Pending_Banner_Cancel.png)

## <a name="next-steps"></a>Passos Seguintes
Se estiver interessado em aprender mais sobre o Azure AD Privileged Identity Management, os links a seguir tem de obter mais informações.

[!INCLUDE [active-directory-privileged-identity-management-toc](../../../includes/active-directory-privileged-identity-management-toc.md)]
