---
title: Introdução ao Assistente do Azure | Documentos da Microsoft
description: Introdução ao Assistente do Azure.
services: advisor
documentationcenter: NA
author: manbeenkohli
manager: ''
ms.assetid: ''
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/10/2017
ms.author: makohli
ms.openlocfilehash: f6b6900425f86cf720e63cda8621ff438f0f9dea
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/09/2018
ms.locfileid: "42060661"
---
# <a name="get-started-with-azure-advisor"></a>Introdução ao Azure Advisor

Saiba como acessar o Advisor através do portal do Azure, obtenha recomendações e implemente recomendações.

## <a name="get-advisor-recommendations"></a>Receba recomendações do Assistente

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

2. No painel esquerdo, clique em **Advisor**.  Se não vir o Advisor no painel esquerdo, clique em **todos os serviços**.  No painel de menu de serviço, em **monitorização e gestão**, clique em **Advisor**.
 O dashboard do assistente é exibido.

   ![Assistente do acesso do Azure no portal do Azure](./media/advisor-get-started/advisor-portal-menu.png) 

4. O dashboard do assistente será apresentado um resumo das suas recomendações para todas as subscrições selecionadas.  Pode escolher a lista pendente de filtro de subscrições que pretende que as recomendações a apresentar para utilizar a subscrição.

5. Para obter recomendações para uma categoria específica, clique em um dos separadores: **elevada disponibilidade**, **Security**, **desempenho**, ou **custo**. 

  ![Dashboard do Assistente do Azure](./media/advisor-overview/advisor-dashboard.png)

## <a name="get-advisor-recommendation-details-and-implement-a-solution"></a>Obter os detalhes de recomendação do assistente e implementar uma solução

Pode selecionar uma recomendação no Advisor para ver detalhes adicionais – por exemplo, as ações de recomendação e obter recursos afetados – e para implementar a solução para a recomendação.  

1. Inicie sessão para o [portal do Azure](https://portal.azure.com)e, em seguida, abra [Advisor](https://aka.ms/azureadvisordashboard).

2. Selecione uma categoria de recomendação para apresentar a lista de recomendações dentro dessa categoria, ou selecione o **todos os** separador para ver todas as suas recomendações.

3. Clique numa recomendação que deseja examinar detalhadamente.

4. Reveja as informações sobre a recomendação e os recursos que a recomendação aplica-se a.

5. Clique nas **ação recomendada** para implementar a recomendação.

## <a name="filter-advisor-recommendations"></a>Recomendações do Assistente de filtro

Pode filtrar recomendações para descer até o que é mais importante para si.  Pode filtrar por subscrição, tipo de recurso ou o estado da recomendação.  

1. Inicie sessão para o [portal do Azure](https://portal.azure.com)e, em seguida, abra [Advisor](https://aka.ms/azureadvisordashboard).

2.  Utilize as listas pendentes no dashboard do Advisor para filtrar por subscrição, o tipo de recurso ou o estado da recomendação.

    ![Critérios de filtro de pesquisa do Advisor](./media/advisor-get-started/advisor-filters.png)

## <a name="postpone-or-dismiss-advisor-recommendations"></a>Adiar ou ignorar as recomendações do Assistente

1. Inicie sessão para o [portal do Azure](https://portal.azure.com)e, em seguida, abra [Advisor](https://aka.ms/azureadvisordashboard).

2. Navegue para a recomendação de que pretende adiar ou ignorar.

3. Clique a recomendação.

4. Clique em **adiar**. 

5. Especifique um ' Adiar ' período de tempo ou selecione **Never** para dispensar a recomendação.

## <a name="exclude-subscriptions-or-resource-groups-from-advisor"></a>Excluir da Advisor subscrições ou grupos de recursos

Poderá ter grupos de recursos ou subscrições para que não pretende receber recomendações do assistente, como recursos de "teste".  Pode configurar o Assistente para gerar apenas recomendações para as subscrições específicas e grupos de recursos.

> [!NOTE]
> Para incluir ou excluir uma subscrição ou grupo de recursos do assistente, tem de ser uma proprietário da subscrição.  Se não tiver as permissões necessárias para uma subscrição ou grupo de recursos, a opção para incluir ou exclui-lo está desativada na interface do usuário.

1. Inicie sessão para o [portal do Azure](https://portal.azure.com)e, em seguida, abra [Advisor](https://aka.ms/azureadvisordashboard).

2. Clique em **configurar** na barra de ação.

3. Desmarcar quaisquer subscrições ou grupos de recursos que não pretende receber recomendações do Assistente para.

    ![O Assistente configurar o exemplo de recursos](./media/advisor-get-started/advisor-configure-resources.png)

4. Clique nas **aplicar** botão.

## <a name="configure-the-average-cpu-utilization-rule-for-the-low-usage-virtual-machine-recommendation"></a>Configurar a regra de utilização de CPU média para a recomendação de máquina virtual de baixa utilização

O assistente monitoriza a utilização da máquina virtual durante 14 dias e, em seguida, identifica as máquinas virtuais de baixa utilização. Máquinas virtuais cuja utilização média da CPU é 5 por cento ou menos e utilização de rede é de 7 MB ou menos para quatro ou mais dias são considerados máquinas de virtuais de baixa utilização.

Se gostaria de ser mais agressivo na identificação de máquinas virtuais de baixa utilização, pode ajustar a regra de utilização de CPU média numa base por subscrição.  A regra de utilização média da CPU pode ser definida como 5%, 10%, % de 15 ou 20%.

> [!NOTE]
> Para ajustar a regra de utilização de CPU média para identificar máquinas de virtuais de baixa utilização, tem de ser uma subscrição *proprietário*.  Se não tiver as permissões necessárias para uma subscrição ou grupo de recursos, a opção para incluir ou exclui-lo será desativada na interface do usuário. 

1. Inicie sessão para o [portal do Azure](https://portal.azure.com)e, em seguida, abra [Advisor](https://aka.ms/azureadvisordashboard).

2. Clique em **configurar** na barra de ação.

3. Clique nas **regras** separador.

4. Selecione as subscrições que pretende ajustar a regra de utilização de CPU média para e, em seguida, clique em **editar**.

5. Selecione o valor de utilização de CPU média desejado e clique em **aplicar**.

6. Clique em **atualizar recomendações** para atualizar as suas recomendações existentes para utilizar a nova regra de utilização média da CPU. 

   ![O Assistente configurar o exemplo de regras de recomendação](./media/advisor-get-started/advisor-configure-rules.png)

## <a name="download-your-advisor-recommendations"></a>Transferir as recomendações do Assistente

O assistente permite-lhe transferir um resumo das suas recomendações.  Pode transferir as suas recomendações como um ficheiro PDF ou um ficheiro CSV.  Transferir as suas recomendações permite-lhe facilmente compartilhar com seus colegas ou realizar sua própria análise sobre os dados de recomendação.

1. Inicie sessão para o [portal do Azure](https://portal.azure.com)e, em seguida, abra [Advisor](https://aka.ms/azureadvisordashboard).

2. Clique em **transferir como CSV** ou **transferir como PDF** na barra de ação.

A opção de download respeita quaisquer filtros que tiver aplicado para o dashboard do assistente.  Se selecionar a opção de transferência enquanto vê uma categoria de recomendação específica ou a recomendação, o resumo transferido só inclui informações para essa categoria ou a recomendação. 

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre o Advisor, consulte:
* [Introdução ao Assistente do Azure](advisor-overview.md)
* [Recomendações de elevada disponibilidade do Assistente](advisor-high-availability-recommendations.md)
* [Recomendações de segurança do Advisor](advisor-security-recommendations.md)
-  [Recomendações de desempenho do Assistente](advisor-performance-recommendations.md)
* [Recomendações de custos do Assistente](advisor-performance-recommendations.md)
