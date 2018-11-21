---
title: Registar a sua subscrição do Azure na Cloudyn | Microsoft Docs
description: Utilize a subscrição do Azure para efetuar o registo na Cloudyn.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 08/07/2018
ms.topic: quickstart
ms.custom: ''
ms.service: cost-management
manager: benshy
ms.openlocfilehash: 4b0c0a6fdf8d84b6519d1228f148342b8486c282
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/21/2018
ms.locfileid: "52276402"
---
# <a name="register-an-individual-azure-subscription-and-view-cost-data"></a>Registe uma subscrição do Azure individual e veja os dados de custos

Utilize a subscrição do Azure para efetuar o registo na Cloudyn. O registo concede acesso ao portal Cloudyn. Este guia de introdução detalha o processo de registo necessário para criar uma subscrição de avaliação do Cloudyn e iniciar sessão no portal Cloudyn. Também lhe mostra como pode começar a ver de imediato os dados dos custos.

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

- Inicie sessão no portal do Azure em http://portal.azure.com.

## <a name="register-with-cloudyn"></a>Registar-se na Cloudyn

1. No portal do Azure, clique em **Cost Management + Faturação** na lista de serviços.
2. Em **Descrição geral**, clique em **Cloudyn**  
    ![Página da Cloudyn](./media/quick-register-azure-sub/cost-mgt-billing-service.png)
3. Na página **Cost Management**, clique em **Aceder à Cloudyn** para abrir a página de registo da Cloudyn numa janela nova.
4. Na página de registo de avaliação do portal da Cloudyn, escreva o nome da empresa e, em seguida, selecione **Proprietário de Subscrição Individual do Azure** e clique em **Seguinte**. O nome de conta e o ID do Inquilino é automaticamente adicionado ao formulário.  
    ![Registo de avaliação](./media/quick-register-azure-sub/trial-reg-ind.png)
5. Selecione o **ID de Oferta - Nome** associado à subscrição. Se não tiver a certeza de qual é o ID de Tarifa da sua subscrição, pode ver a fatura do Azure e procurar por **ID da Oferta**.
6. Aceite os Termos de Utilização e, em seguida, valide as suas informações e clique em **Seguinte**.
7. Na página **Reunir dados adicionais**, clique em **Seguinte** para autorizar a Cloudyn a recolher dados de recursos do Azure. Os dados recolhidos incluem dados de utilização, de desempenho, de faturação e da etiqueta das suas subscrições.  
    ![Reunir dados adicionais](./media/quick-register-azure-sub/gather-additional.png)
8. O browser leva-o para a página de início de sessão da Cloudyn. Inicie sessão com as credenciais de subscrição do Azure.
9. Clique em **Aceder à Cloudyn** para abrir o portal da Cloudyn e, em seguida, na página **Gestão de Contas**, deverá ver informações da sua conta de subscrição do Azure.  
    ![Gestão de Contas](./media/quick-register-azure-sub/accounts-mgt.png)

Para ver um vídeo tutorial sobre como registar a sua subscrição do Azure, consulte [Finding your Directory GUID and Rate ID for use in Cloudyn (Encontrar o GUID de Diretório e o ID da Tarifa para utilizar na Cloudyn)](https://youtu.be/PaRjnyaNGMI).

[!INCLUDE [cost-management-create-account-view-data](../../includes/cost-management-create-account-view-data.md)]

## <a name="next-steps"></a>Passos Seguintes

Neste início rápido, utilizou as suas informações de subscrição do Azure para efetuar o registo na Cloudyn. Também iniciou sessão no portal Cloudyn e começou a ver os dados dos custos. Para saber mais sobre a Cloudyn, continue para o tutorial da Cloudyn.

> [!div class="nextstepaction"]
> [Rever a utilização e os custos](./tutorial-review-usage.md)
