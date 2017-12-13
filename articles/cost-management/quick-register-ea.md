---
title: Registe o seu Contrato Enterprise do Azure com o Azure Cost Management | Microsoft Docs
description: Utilize o Contrato Enterprise para registar com o Azure Cost Management da Cloudyn.
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 10/11/2017
ms.topic: quickstart
ms.custom: mvc
ms.service: cost-management
manager: carmonm
ms.openlocfilehash: 41a9df712b07253d9f5f9db8542fb9917592320f
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/12/2017
---
# <a name="register-an-azure-enterprise-agreement-and-view-cost-data"></a>Registe um Contrato Enterprise do Azure e veja os dados de custos

Utiliza o Contrato Enterprise do Azure para registar com o Azure Cost Management da Cloudyn. O registo concede acesso ao portal da Cloudyn. Este guia de introdução detalha o processo de registo necessário para criar uma subscrição de avaliação da Cloudyn e iniciar sessão no portal da Cloudyn. Também lhe mostra como começar a ver os dados de custo imediatamente.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

- Inicie sessão no portal do Azure em http://portal.azure.com.

## <a name="create-a-trial-registration"></a>Criar um registo de avaliação

1. No portal do Azure, clique em **Cost Management + Faturação** na lista de serviços.
2. Em **Descrição geral**, clique em **Cost Management**  
    ![Página do Cost Management](./media/quick-register-ea/cost-mgt-billing-service.png)
3. Na página **Cost Management**, **Aceda ao Cost Management** para abrir a página de registo da Cloudyn numa nova janela.
4. Na página de registo de avaliação do portal da Cloudyn, escreva o nome da empresa e, em seguida, selecione **Administrador de Inscrição do Azure Enterprise**.  
    ![registo de avaliação](./media/quick-register-ea/trial-reg.png)
5. Introduza a chave de API de inscrição do Portal Enterprise. Se não tiver a chave à mão, clique na ligação [Portal Enterprise](https://ea.azure.com) e realize os passos seguintes:
  1. Inicie sessão no site do Azure Enterprise e clique em **Relatórios**, clique em **Chave de Acesso da API** e, em seguida, copie a chave primária.  
    ![Chave de API EA](./media/quick-register-ea/ea-key.png)
  3. Volte à página de registo e cole a sua chave de API.
6. Aceite os Termos de Utilização e valide a sua chave. Clique em **Seguinte** para autorizar a Cloudyn a recolher dados de recursos do Azure. Os dados recolhidos incluem dados de utilização, de desempenho, de faturação e da etiqueta das suas subscrições.  
    ![validação da chave](./media/quick-register-ea/ea-key-validated.png)
7. Em **Convidar outros intervenientes**, pode adicionar utilizadores ao escrever os respetivos endereços de e-mail. Quando terminar, clique em **Seguinte**. Demora cerca de duas horas para todos os dados de faturação serem adicionados à Cloudyn.
8. Clique em **Aceder à Cloudyn** para abrir o portal da Cloudyn e, em seguida, na página **Gestão de Contas da Cloud**, deverá ver informações da sua conta EA registada.

Para ver um vídeo tutorial sobre como registar o Contrato Enterprise, veja [How to Find Your EA Enrollment ID and API Key for use in Azure Cost Management by Cloudyn (Como Encontrar o Seu ID de Inscrição EA e a Chave de API para utilização no Azure Cost Management da Cloudyn)](https://youtu.be/u_phLs_udig).

[!INCLUDE [cost-management-create-account-view-data](../../includes/cost-management-create-account-view-data.md)]

## <a name="next-steps"></a>Passos seguintes

Neste guia de introdução utilizou as sua informações do Contrato Enterprise do Azure para registar com o Cost Management. Também se inscreveu no portal da Cloudyn e começou a ver os dados de custos. Para saber mais sobre o Azure Cost Management da Cloudyn, continue para o tutorial do Cost Management.

> [!div class="nextstepaction"]
> [Rever a utilização e os custos](./tutorial-review-usage.md)
