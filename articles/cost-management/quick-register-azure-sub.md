---
title: Registe a sua subscrição do Azure com o Azure Cost Management | Microsoft Docs
description: Utilize a subscrição do Azure para registar com o Azure Cost Management.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 01/30/2018
ms.topic: quickstart
ms.custom: mvc
ms.service: cost-management
manager: carmonm
ms.openlocfilehash: b7c1fee3276d98b6b7c611a09133804c1253384a
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/09/2018
---
# <a name="register-an-individual-azure-subscription-and-view-cost-data"></a>Registe uma subscrição do Azure individual e veja os dados de custos

Utilize a subscrição do Azure para registar com o Azure Cost Management. O registo concede acesso ao portal Cloudyn. Este guia de introdução detalha o processo de registo necessário para criar uma subscrição de avaliação do Cloudyn e iniciar sessão no portal Cloudyn. Também lhe mostra como pode começar a ver de imediato os dados dos custos.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

- Inicie sessão no portal do Azure em http://portal.azure.com.

## <a name="create-a-trial-registration"></a>Criar um registo de avaliação

1. No portal do Azure, clique em **Cost Management + Faturação** na lista de serviços.
2. Em **Descrição geral**, clique em **Cost Management**  
    ![Página do Cost Management](./media/quick-register-azure-sub/cost-mgt-billing-service.png)
3. Na página **Cost Management**, clique em **Aceda ao Cost Management** para abrir a página de registo da Cloudyn numa nova janela.
4. Na página de registo de avaliação do portal da Cloudyn, escreva o nome da empresa e, em seguida, selecione **Proprietário de Subscrição Individual do Azure** e clique em **Seguinte**. O nome de conta e o ID do Inquilino é automaticamente adicionado ao formulário.  
    ![registo de avaliação](./media/quick-register-azure-sub/trial-reg-ind.png)
5. Selecione o **ID de Oferta - Nome** associado à subscrição. Se não tiver a certeza de qual é o ID de Tarifa da sua subscrição, pode ver a fatura do Azure e procurar por **ID da Oferta**.
6. Aceite os Termos de Utilização e, em seguida, valide as suas informações e clique em **Seguinte**.
7. Na página **Reunir dados adicionais**, clique em **Seguinte** para autorizar a Cloudyn a recolher dados de recursos do Azure. Os dados recolhidos incluem dados de utilização, de desempenho, de faturação e da etiqueta das suas subscrições.  
    ![reunir dados adicionais](./media/quick-register-azure-sub/gather-additional.png)
8. O browser leva-o para a página de início de sessão da Cloudyn. Inicie sessão com as credenciais de subscrição do Azure.
9. Clique em **Aceder à Cloudyn** para abrir o portal da Cloudyn e, em seguida, na página **Gestão de Contas**, deverá ver informações da sua conta de subscrição do Azure.  
    ![Gestão de Contas](./media/quick-register-azure-sub/accounts-mgt.png)

Para ver um vídeo tutorial sobre como registar a sua subscrição do Azure, veja [Finding your Directory GUID and Rate ID for use in Azure Cost Management (Encontrar o GUID de Diretório e o ID da Tarifa para utilizar no Azure Cost Management)](https://youtu.be/PaRjnyaNGMI).

[!INCLUDE [cost-management-create-account-view-data](../../includes/cost-management-create-account-view-data.md)]

## <a name="next-steps"></a>Passos seguintes

Neste guia de introdução utilizou as sua informações da subscrição do Azure para registar com o Cost Management. Também iniciou sessão no portal Cloudyn e começou a ver os dados dos custos. Para saber mais sobre o Azure Cost Management, continue para o tutorial do Cost Management.

> [!div class="nextstepaction"]
> [Rever a utilização e os custos](./tutorial-review-usage.md)
