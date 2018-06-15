---
title: As soluções de gestão no Azure | Microsoft Docs
description: Soluções de gestão no Azure são uma coleção de regras de aquisição lógica, visualização e os dados que fornecem métricas pivoted em torno de uma área de problema específico.  Este artigo fornece informações sobre como instalar e utilizar as soluções de gestão.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: f029dd6d-58ae-42c5-ad27-e6cc92352b3b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2018
ms.author: bwren
ms.openlocfilehash: 0df54d1758693bce5fb5fd74c3be9c4cfd7dccb6
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
ms.locfileid: "33885273"
---
# <a name="management-solutions-in-azure"></a>Soluções de gestão no Azure
As soluções de gestão tirar partido dos serviços do Azure para fornecer informações adicionais sobre a operação de uma determinada aplicação ou serviço. Este artigo fornece uma breve descrição geral das soluções de gestão no Azure e detalhes sobre como utilizar e instalá-los.

Soluções de gestão, normalmente, recolhem informações para análise de registos e fornecem pesquisas de registo e vistas para analisar os dados recolhidos. Estes podem também tirar partido de outros serviços, tais como a automatização do Azure para efetuar ações relacionadas com a aplicação ou serviço.

Pode adicionar as soluções de gestão à sua subscrição do Azure para quaisquer aplicações e serviços que utiliza. São normalmente disponíveis em nenhum dado custo mas recolher que invocam a custos de utilização. Além das soluções fornecidas pela Microsoft, parceiros e os clientes podem [criar soluções de gestão](../monitoring/monitoring-solutions-creating.md) a ser utilizada no seu próprio ambiente ou disponibilizada para clientes através da Comunidade.

## <a name="using-management-solutions"></a>Soluções de gestão
O **descrição geral** página para cada área de trabalho de análise de registos apresenta um mosaico para cada solução instalado na área de trabalho. Clique no mosaico para a solução abrir a vista que inclui uma análise mais detalhada dos dados recolhidos.

![Descrição geral](media/monitoring-solutions/overview.png)

As soluções de gestão podem conter vários tipos de recursos do Azure e pode ver todos os recursos incluídos com uma solução, tal como quaisquer outros recursos. Por exemplo, as pesquisas de registo incluídas na solução estão incluídas com **pesquisas guardadas** na área de trabalho. Pode utilizar as pesquisas quando efetuar a análise ad-hoc na análise de registos.

## <a name="list-installed-management-solutions"></a>Lista de soluções de gestão instalados 
Utilize o procedimento seguinte para listar as soluções de gestão instaladas na sua subscrição.

1. Inicie sessão no portal do Azure.
2. No painel esquerdo, selecione **todos os serviços**.
3. O desloque para baixo até **soluções** ou tipo *soluções* para o **filtro** caixa de diálogo.
4. Soluções instaladas em todas as áreas de trabalho são listadas. O nome da solução é seguido pelo nome da área de trabalho de análise de registos que está instalado na.
1. Utilize as caixas de lista pendente, na parte superior do ecrã, para filtrar por subscrição ou grupo de recursos.


![Lista todas as soluções](media/monitoring-solutions/list-solutions-all.png)

Clique no nome de uma solução para abrir a página de resumo. Esta página apresenta as vistas de análise de registos incluídas na solução e fornece diferentes opções para a solução em si e a área de trabalho. Ver a página de resumo para uma solução utilizando um dos procedimentos acima para soluções de lista e, em seguida, clique no nome da solução.

![Propriedades de solução](media/monitoring-solutions/solution-properties.png)


## <a name="find-management-solutions"></a>Encontre soluções de gestão
Pode procurar e instalar as soluções de gestão disponíveis da Microsoft e de parceiros no [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace). Efetuar uma [procurar *soluções de gestão* ](https://azuremarketplace.microsoft.com/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions) para filtrar para soluções de gestão e, em seguida, clique em qualquer item para obter mais detalhes.

![Marketplace](media/monitoring-solutions/marketplace.png)

## <a name="install-a-management-solution"></a>Instalar uma solução de gestão

### <a name="install-a-management-solution-from-the-azure-marketplace"></a>Instalar uma solução de gestão do Azure Marketplace
Pode utilizar qualquer um dos seguintes métodos para localizar e iniciar a instalação de uma solução de gestão.

- Clique em **obtê-lo agora** numa solução de gestão no [Azure Marketplace](#find-management-solutions).
- Do [lista de soluções para a sua subscrição](#list-installed-management-solutions), clique em **adicionar**. À direita do **soluções de gestão**, clique em **mais**. Localize a solução de gestão que pretende e clique em **criar**.
- No portal do Azure, selecione **crie um recurso** > **monitorização + gestão** > **ver todos os**. À direita do **soluções de gestão**, clique em **mais**. Localize a solução de gestão que pretende e clique em **criar**.

Quando é iniciado o processo de instalação, está a pedido para fornecer configuração necessárias que varia de cada solução. Todos eles tem de selecionar uma área de trabalho de análise de registos onde será instalada a solução e onde serão recolhidos os respetivos dados. Também poderá ter de [especificar uma conta de automatização](#log-analytics-workspace-and-automation-account) se for necessário, a solução.

### <a name="install-a-solution-from-the-community"></a>Instalar uma solução da Comunidade
Os membros da Comunidade podem submeter soluções de gestão de modelos de início rápido do Azure. Pode instalar estas soluções diretamente ou transferi-los modelos para instalação posterior.

1. Siga o processo descrito no [área de trabalho de análise de registos e a conta de automatização](#log-analytics-workspace-and-automation-account) para ligar uma área de trabalho e a conta.
2. Aceda a [modelos de início rápido do Azure](https://azure.microsoft.com/documentation/templates/). 
3. Procurar uma solução que lhe interessa.
4. Selecione a solução na lista de resultados para ver os detalhes.
5. Clique em de **implementar no Azure** botão.
6. É-lhe pedido que fornecem informações tais como o grupo de recursos e a localização para além de valores para os parâmetros na solução.
7. Clique em **Compra** para instalar a solução.


## <a name="log-analytics-workspace-and-automation-account"></a>Área de trabalho de análise de registo e a conta de automatização
Todas as soluções de gestão requerem um [área de trabalho de análise de registos](../log-analytics/log-analytics-manage-access.md) para armazenar os dados recolhidos pela solução de e para alojar a vistas e pesquisas de registo. Algumas soluções também requerem um [conta de automatização](../automation/automation-security-overview.md#automation-account-overview) para conter os runbooks e recursos relacionados. A área de trabalho e a conta tem de cumprir os seguintes requisitos.

* Cada instalação de uma solução só pode utilizar uma área de trabalho de análise de registos e uma conta de automatização. Pode instalar a solução em separado para várias áreas de trabalho.
* Se uma solução requer uma conta de automatização, em seguida, a área de trabalho de análise de registos e a conta de automatização tem ser ligadas a um do outro. Uma área de trabalho do Log Analytics só pode ser associada a uma conta de automatização e uma conta de automatização só pode ser associada a uma área de trabalho de análise de registos.
* Estar ligado, a área de trabalho de análise de registos e a conta de automatização devem estar no mesmo grupo de recursos e região. A exceção é a região EUA Leste uma área de trabalho e a conta de automatização nos EUA Leste 2.

### <a name="creating-a-link-between-a-log-analytics-workspace-and-automation-account"></a>Criar uma ligação entre uma área de trabalho de análise de registos e a conta de automatização
Como especificar a área de trabalho de análise de registos e a conta de automatização depende o método de instalação para a sua solução.

* Quando instala uma solução através do Azure Marketplace, lhe for pedido para uma área de trabalho e a conta de automatização. É criada a ligação entre eles se estes já não estão ligados.
* Para soluções fora do Azure Marketplace, tem de ligar a área de trabalho de análise de registos e a conta de automatização antes de instalar a solução. Pode fazê-lo a selecionar qualquer solução no Azure Marketplace e selecionando a área de trabalho de análise de registos e a conta de automatização. Não tem de instalar, na verdade, a solução porque a ligação for criada assim que a área de trabalho de análise de registos e a conta de automatização estão selecionadas. Depois da ligação for criada, em seguida, pode utilizar essa área de trabalho de análise de registos e a conta de automatização para qualquer solução.

### <a name="verifying-the-link-between-a-log-analytics-workspace-and-automation-account"></a>Verificar a ligação entre uma área de trabalho de análise de registos e a conta de automatização
Pode verificar a ligação entre uma área de trabalho de análise de registos e uma conta de automatização utilizando o procedimento seguinte.

1. Selecione a conta de automatização no portal do Azure.
1. Desloque-se para o **recursos relacionados** secção do menu.
1. Se o **área de trabalho** definição estiver ativada, em seguida, esta conta está associada a uma área de trabalho de análise de registos. Pode clicar em **área de trabalho** para ver os detalhes da área de trabalho.

## <a name="remove-a-management-solution"></a>Remover uma solução de gestão
Para remover uma solução de instalados, localizá-la no [lista de soluções instaladas](#list-installed-management-solutions). Clique no nome da solução para abrir a página de resumo e, em seguida, clique em **eliminar**.




## <a name="next-steps"></a>Passos Seguintes
* Obter um [lista de soluções de gestão da Microsoft](monitoring-solutions-inventory.md).
* Saiba como [criar consultas](../log-analytics/log-analytics-log-searches.md) para analisar os dados recolhidos por soluções de gestão.

