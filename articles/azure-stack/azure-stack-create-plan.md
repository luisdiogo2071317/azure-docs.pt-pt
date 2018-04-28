---
title: Criar um plano na pilha do Azure | Microsoft Docs
description: Como um administrador da nuvem, crie um plano que permite que os subscritores aprovisionar máquinas virtuais.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 3dc92e5c-c004-49db-9a94-783f1f798b98
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/23/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: b1bfff16c4f51a9fa53204930df78cbd2cf19b8d
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="create-a-plan-in-azure-stack"></a>Criar um plano no Azure Stack

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Os [Planos](azure-stack-key-features.md) consistem em agrupamentos de um ou mais serviços. Como um fornecedor, pode criar planos para oferecer aos seus utilizadores. Por sua vez, os utilizadores subscrevem as ofertas para utilizar os serviços incluem e planos. Este exemplo mostra como criar um plano que inclui computação, rede e os fornecedores de recursos de armazenamento. Este plano permite subscritores aprovisionar as máquinas virtuais.

1. Inicie sessão no portal de administrador a pilha do Azure (https://adminportal.local.azurestack.external).

2. Para criar um plano e a oferta que os utilizadores podem subscrever, selecione **novo** > **oferece + planos** > **plano**.  
   ![Selecionar um plano](media/azure-stack-create-plan/select-plan.png)

3. No **novo plano** painel, preencha **nome a apresentar** e **nome do recurso**. O nome de apresentação é o nome amigável do plano e que os utilizadores veem. Apenas o administrador pode ver o nome de recurso, o que é o nome que os administradores utilizam para trabalhar com o plano como um recurso do Azure Resource Manager.  
   ![Especifique os detalhes](media/azure-stack-create-plan/plan-name.png)

4. Crie um novo **grupo de recursos**, ou selecione um existente, como um contentor para o plano.  
   ![Especifique o grupo de recursos](media/azure-stack-create-plan/resource-group.png)

5. Selecione **serviços** e, em seguida, selecione a caixa de verificação **Microsoft. Compute**, **Network**, e **Microsoft**. Em seguida, escolha **selecione** para guardar a configuração. Caixas de verificação são apresentadas quando o rato passa sobre cada opção.  
   ![Selecione os serviços](media/azure-stack-create-plan/services.png)

6. Selecione **Quotas**, **Microsoft (local)** e, em seguida, escolha o quota predefinida ou selecione **criar nova quota** para personalizar a quota.  
   ![Quotas](media/azure-stack-create-plan/quotas.png)

7. Se estiver a criar uma nova quota, introduza um **nome** para a quota > especifique os valores de quota > selecione **OK**. O **Criar quota** fecha do painel.
   ![Nova quota](media/azure-stack-create-plan/new-quota.png)

   Em seguida, selecione a nova quota que criou. Selecionar a quota atribui-o e fecha o painel de seleção.  
   ![Atribuir a quota](media/azure-stack-create-plan/assign-quota.png)

8. Repita os passos 6 e 7 para criar e atribuir quotas para **Network (local)** e **Microsoft. Compute (local)**.  Quando os três serviços têm quotas atribuídas, aparecem semelhantes para a imagem seguinte.  
   ![Atribuições de quota concluída](media/azure-stack-create-plan/all-quotas-assigned.png)

9. No **Quotas** painel, escolha **OK**e, em seguida, no **novo plano** painel, escolha **criar** para criar o plano.  
    ![Criar o plano](media/azure-stack-create-plan/create.png)
10. Para ver o seu plano de novo, selecione **todos os recursos**, em seguida, procure o plano e selecione o respetivo nome. Se a lista de recursos é longa, utilize **pesquisa** para localizar o plano de por nome.  
   ![Rever o plano](media/azure-stack-create-plan/plan-overview.png)

### <a name="next-steps"></a>Passos Seguintes
[Criar uma oferta](azure-stack-create-offer.md)
