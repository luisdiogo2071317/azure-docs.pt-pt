---
title: Criar um plano no Azure Stack | Documentos da Microsoft
description: Como um administrador da nuvem, crie um plano que permita aos subscritores aprovisionar máquinas virtuais.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 3dc92e5c-c004-49db-9a94-783f1f798b98
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/09/2019
ms.author: sethm
ms.reviewer: efemmano
ms.lastreviewed: 01/09/2019
ms.openlocfilehash: 8fb36ac19e731f9274975a3bf1183869f15fbceb
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2019
ms.locfileid: "55984618"
---
# <a name="create-a-plan-in-azure-stack"></a>Criar um plano no Azure Stack

*Aplica-se a: Integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Os [Planos](azure-stack-key-features.md) consistem em agrupamentos de um ou mais serviços. Como um fornecedor, pode criar planos para oferecer aos seus utilizadores. Por sua vez, os seus utilizadores subscrevem suas ofertas para utilizar os planos e serviços incluídos. Este exemplo mostra como criar um plano que inclui computação, rede e os fornecedores de recursos de armazenamento. Este plano dá os assinantes a capacidade de aprovisionar máquinas virtuais.

1. Inicie sessão para o [portal de administrador do Azure Stack](https://adminportal.local.azurestack.external).

2. Para criar um plano e uma oferta que os utilizadores podem subscrever, selecione **+ criar um recurso**, em seguida, **ofertas + planos**, em seguida, **plano**.
  
   ![Selecionar um plano](media/azure-stack-create-plan/select-plan.png)

3. Sob **novo plano**, introduza um **nome a apresentar** e um **nome do recurso**. O nome a apresentar é o nome amigável do plano que os utilizadores podem ver. Apenas o administrador pode ver o nome do recurso, o administrador utiliza para trabalhar com o plano como um recurso do Azure Resource Manager.

   ![Especifique os detalhes](media/azure-stack-create-plan/plan-name.png)

4. Criar uma nova **grupo de recursos**, ou selecione um existente, como um contêiner para o plano.

   ![Especifique o grupo de recursos](media/azure-stack-create-plan/resource-group.png)

5. Selecione **serviços** e, em seguida, selecione a caixa de verificação **Microsoft. Compute**, **Network**, e **Microsoft. Storage**. Em seguida, escolha **selecione** para guardar a configuração. Caixas de seleção aparecem quando o mouse passa sobre cada opção.
  
   ![Selecionar serviços](media/azure-stack-create-plan/services.png)

6. Selecione **Quotas**, **Microsoft. Storage (local)** e, em seguida, escolha o quota predefinida ou selecione **criar nova quota** para criar uma quota personalizada.
  
   ![Quotas](media/azure-stack-create-plan/quotas.png)

7. Se estiver a criar uma quota de novo, introduza um **Name** para a quota > especifique os valores de quota > selecione **OK**. O **Criar quota** fecha a caixa de diálogo.

   ![Nova quota](media/azure-stack-create-plan/new-quota.png)

   Em seguida, selecione a quota de novo que criou. Selecionar a quota, atribui-o e fecha a caixa de diálogo de seleção.
  
   ![Atribuir a quota](media/azure-stack-create-plan/assign-quota.png)

8. Repita os passos 6 e 7 para criar e atribuir as quotas para **Network (local)** e **Microsoft. Compute (local)**. Quando todos os três serviços tem quotas atribuídas, eles ver como o exemplo seguinte.

   ![Atribuições de quota concluída](media/azure-stack-create-plan/all-quotas-assigned.png)

9. Sob **Quotas**, escolha **OK**e, em **novo plano**, selecione **criar** para criar o plano.

    ![Criar o plano](media/azure-stack-create-plan/create.png)

10. Para ver o seu plano de novo, selecione **todos os recursos**, em seguida, procure o plano e selecione o seu nome. Se a sua lista de recursos é longa, utilize **pesquisa** para localizar o seu plano por nome.

   ![Rever o plano](media/azure-stack-create-plan/plan-overview.png)

## <a name="next-steps"></a>Passos Seguintes

* [Criar uma oferta](azure-stack-create-offer.md)
