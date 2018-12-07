---
title: Soluções de gestão do Azure | Documentos da Microsoft
description: Soluções de gestão no Azure são um conjunto de dados, visualização e lógica de regras de aquisição que fornecem métricas articuladas em torno de uma área de problema específico.  Este artigo fornece informações sobre como instalar e utilizar soluções de gestão.
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
ms.date: 06/22/2018
ms.author: bwren
ms.openlocfilehash: 2bf82c19498087e21bc66bf3c813b8f23d50d621
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "52996503"
---
# <a name="management-solutions-in-azure"></a>Soluções de gestão do Azure
Soluções de gestão de tirar partido dos serviços do Azure para fornecer informações adicionais sobre o funcionamento de uma determinada aplicação ou serviço. Este artigo fornece uma breve descrição geral de soluções de gestão do Azure e os detalhes sobre como utilizar e instalá-los.

Soluções de gestão, normalmente, recolhem informações no Log Analytics e fornecem pesquisas de registos e modos de exibição para analisar os dados recolhidos. Eles também podem tirar partido outros serviços, como a automatização do Azure para efetuar ações relacionadas com a aplicação ou serviço.

Pode adicionar soluções de gestão para a sua subscrição do Azure para quaisquer aplicações e serviços que utilizar. Eles estão normalmente disponíveis em nenhum dado de custo mas recolher invoca os custos de utilização. Além das soluções fornecidas pela Microsoft, parceiros e clientes podem [criar soluções de gestão](solutions-creating.md) a ser utilizada no seu próprio ambiente ou disponibilizados aos clientes através da Comunidade.

## <a name="using-management-solutions"></a>Usando soluções de gestão
O **descrição geral** página para cada área de trabalho do Log Analytics apresenta um mosaico para cada solução instalado na área de trabalho. Clique no mosaico para a solução abrir a vista que inclui uma análise mais detalhada em seus dados recolhidos.

![Descrição geral](media/solutions/overview.png)

Soluções de gestão podem conter vários tipos de recursos do Azure e pode ver todos os recursos incluídos com uma solução, assim como qualquer outro recurso. Por exemplo, são incluídas com qualquer incluídas na solução de pesquisas de registos **pesquisas guardadas** na área de trabalho. Pode usar essas pesquisas ao executar a análise ad hoc no Log Analytics.

## <a name="list-installed-management-solutions"></a>Liste as soluções de gestão instalados 
Utilize o procedimento seguinte para listar as soluções de gestão instaladas na sua subscrição.

1. Inicie sessão no portal do Azure.
2. No painel esquerdo, selecione **todos os serviços**.
3. Qualquer um desloque para baixo até **soluções** ou tipo *soluções* para o **filtro** caixa de diálogo.
4. Soluções instaladas em todas as áreas de trabalho são listadas. O nome da solução é seguido do nome da área de trabalho do Log Analytics que está instalado na.
1. Utilize as caixas de lista pendente na parte superior do ecrã para filtrar por subscrição ou grupo de recursos.


![Listar todas as soluções](media/solutions/list-solutions-all.png)

Clique no nome de uma solução para abrir a página de resumida. Esta página apresenta todas as vistas do Log Analytics incluídas na solução e fornece opções diferentes para a solução em si e a sua área de trabalho. Ver a página de resumo para uma solução utilizando um dos procedimentos acima para soluções de lista e, em seguida, clique no nome da solução.

![Propriedades de solução](media/solutions/solution-properties.png)



## <a name="install-a-management-solution"></a>Instalar uma solução de gestão
Soluções de gestão da Microsoft e parceiros estão disponíveis a partir da [do Azure Marketplace](https://azuremarketplace.microsoft.com). Pode procurar as soluções disponíveis e instalá-los utilizando o procedimento seguinte.

1. Partir do [lista de soluções para a sua subscrição](#list-installed-management-solutions), clique em **Add**. 
1. À direita da **soluções de gestão**, clique em **mais**. 
1. Localize a solução de gestão que pretende e ler sua descrição.
1. Clique em **criar** para iniciar o processo de instalação.
1. Quando o processo de instalação é iniciado, lhe for pedido para fornecer configuração necessárias que varia para cada solução. Todos eles irão requerer que selecione uma área de trabalho do Log Analytics onde a solução será instalada e onde serão recolhidos os seus dados. 

![Instalar uma solução](media/solutions/install-solution.png)

### <a name="install-a-solution-from-the-community"></a>Instalar uma solução da Comunidade
Membros da Comunidade podem submeter soluções de gestão para modelos de início rápido do Azure. Pode instalar essas soluções diretamente ou transferi-los modelos para posterior instalação.

1. Siga o processo descrito em [área de trabalho do Log Analytics e a conta de automatização](#log-analytics-workspace-and-automation-account) para ligar uma área de trabalho e a conta.
2. Aceda a [modelos de início rápido do Azure](https://azure.microsoft.com/documentation/templates/). 
3. Procurar por uma solução que está interessado.
4. Selecione a solução dos resultados para ver os detalhes.
5. Clique nas **implementar no Azure** botão.
6. Lhe for pedido para fornecer informações tais como o grupo de recursos e a localização, além de valores de quaisquer parâmetros na solução.
7. Clique em **Compra** para instalar a solução.


## <a name="log-analytics-workspace-and-automation-account"></a>Área de trabalho do log Analytics e a conta de automatização
Todas as soluções de gestão exigem uma [área de trabalho do Log Analytics](../../azure-monitor/platform/manage-access.md) para armazenar os dados coletados pela solução e para alojar as suas vistas e pesquisas de registos. Algumas soluções também exigem um [conta de automatização](../../automation/automation-security-overview.md#automation-account-overview) para conter runbooks e recursos relacionados. A área de trabalho e a conta tem de cumprir os seguintes requisitos.

* Cada instalação de uma solução só pode utilizar uma área de trabalho do Log Analytics e uma conta de automatização. Pode instalar a solução em separado em várias áreas de trabalho.
* Se uma solução requer uma conta de automatização, em seguida, a área de trabalho do Log Analytics e a conta de automatização devem ser vinculados a um do outro. Uma área de trabalho do Log Analytics só pode ser ligada a uma conta de automatização e uma conta de automatização só pode ser ligada a uma área de trabalho do Log Analytics.
* Para ser ligada a área de trabalho do Log Analytics e a conta de automatização tem de ser no mesmo grupo de recursos e região. A exceção é uma área de trabalho na região E.U.A. leste e conta de automatização na região E.U.A. Leste 2.

### <a name="creating-a-link-between-a-log-analytics-workspace-and-automation-account"></a>Criar uma ligação entre uma área de trabalho do Log Analytics e uma conta de automatização
Como especificar a área de trabalho do Log Analytics e a conta de automatização depende do método de instalação para a sua solução.

* Quando instala uma solução no Azure Marketplace, lhe for pedido para a área de trabalho e conta de automatização. A ligação entre eles é criada se eles já não estão ligados.
* Para soluções de fora do Azure Marketplace, tem de associar a área de trabalho do Log Analytics e a conta de automatização antes de instalar a solução. Pode fazê-lo ao selecionar qualquer solução no Azure Marketplace e ao selecionar a área de trabalho do Log Analytics e a conta de automatização. Não tem de instalar, na verdade, a solução porque a ligação for criada assim que a área de trabalho do Log Analytics e a conta de automatização estão selecionadas. Depois de criar a ligação, em seguida, pode utilizar essa área de trabalho do Log Analytics e a conta de automatização para qualquer solução.

### <a name="verifying-the-link-between-a-log-analytics-workspace-and-automation-account"></a>A verificar a ligação entre uma área de trabalho do Log Analytics e uma conta de automatização
Pode verificar a ligação entre uma área de trabalho do Log Analytics e uma conta de automatização utilizando o procedimento seguinte.

1. Selecione a conta de automatização no portal do Azure.
1. Desloque-se para o **recursos relacionados** secção do menu.
1. Se o **área de trabalho** está habilitada, em seguida, esta conta está ligada a uma área de trabalho do Log Analytics. Pode clicar em **área de trabalho** para ver os detalhes da área de trabalho.

## <a name="remove-a-management-solution"></a>Remover uma solução de gestão
Para remover uma solução de instalados, localizá-la na [lista de soluções instaladas](#list-installed-management-solutions). Clique no nome da solução para abrir a página de resumida e, em seguida, clique em **eliminar**.




## <a name="next-steps"></a>Passos Seguintes
* Obter um [lista de soluções de gestão da Microsoft](solutions-inventory.md).
* Saiba como [criar consultas](../../azure-monitor/log-query/log-query-overview.md) para analisar os dados recolhidos por soluções de gestão.

