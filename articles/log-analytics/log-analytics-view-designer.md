---
title: Criar vistas para analisar dados no Log Analytics do Azure | Documentos da Microsoft
description: Ao utilizar o estruturador de vistas no Log Analytics, pode criar vistas personalizadas que são apresentadas no portal do Azure e contêm uma variedade de visualizações de dados na área de trabalho do Log Analytics. Este artigo contém uma descrição geral do estruturador de vistas e apresenta os procedimentos para criação e edição de exibições personalizadas.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ce41dc30-e568-43c1-97fa-81e5997c946a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 77c7d9a549b10a56faaf6226062d0e6212ce19c0
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/20/2018
ms.locfileid: "42057398"
---
# <a name="create-custom-views-by-using-view-designer-in-log-analytics"></a>Criar vistas personalizadas com o estruturador de vistas no Log Analytics
Ao utilizar o estruturador de vistas no [do Azure Log Analytics](log-analytics-overview.md), pode criar diversas exibições personalizadas no portal do Azure pode ajudá-lo a visualizar os dados na sua área de trabalho do Log Analytics. Este artigo apresenta uma visão geral do estruturador de vistas e procedimentos para criação e edição de exibições personalizadas.

Para obter mais informações sobre o estruturador de vistas, consulte:

* [Referência de mosaico](log-analytics-view-designer-tiles.md): Fornece um guia de referência para as definições para cada um dos mosaicos disponíveis nas suas vistas personalizadas.
* [Referência de parte da visualização](log-analytics-view-designer-parts.md): Fornece um guia de referência para as definições para as partes de visualização que estão disponíveis em suas vistas personalizadas.


## <a name="concepts"></a>Conceitos
Modos de exibição são apresentados no **descrição geral** página da sua área de trabalho do Log Analytics no portal do Azure. Os mosaicos em cada modo de exibição personalizado são apresentados por ordem alfabética e os mosaicos para as soluções são instalados a mesma área de trabalho.

![Página de descrição geral](media/log-analytics-view-designer/overview-page.png)

As vistas que criar com o estruturador de vistas contêm os elementos que são descritos na tabela a seguir:

| Parte | Descrição |
|:--- |:--- |
| Mosaicos | São apresentadas na sua área de trabalho do Log Analytics **descrição geral** página. Cada mosaico mostra um resumo visual do modo de exibição personalizado, que ele representa. Cada tipo de mosaico fornece uma visualização diferente de seus registros. Selecionar um mosaico para apresentar uma vista personalizada. |
| Vista personalizada | Apresentado quando seleciona um mosaico. Cada vista contém um ou mais partes de visualização. |
| Partes de visualização | Apresentar uma visualização de dados na área de trabalho do Log Analytics com base num ou vários [pesquisas de registos](log-analytics-log-searches.md). A maioria das partes incluem um cabeçalho, que fornece uma visualização de alto nível, e uma lista, que apresenta os principais resultados. Cada tipo de parte fornece uma visualização diferente dos registos na área de trabalho do Log Analytics. Selecionar elementos na parte para efetuar uma pesquisa de registos que fornece registos detalhados. |


## <a name="work-with-an-existing-view"></a>Trabalhar com uma vista existente
Vistas que foram criadas com o estruturador de vistas apresentam as seguintes opções:

![Menu de descrição geral](media/log-analytics-view-designer/overview-menu.png)

As opções são descritas na tabela a seguir:

| Opção | Descrição |
|:--|:--|
| Atualizar   | Atualiza a exibição com os dados mais recentes. | 
| Análise | Abre o [portal da análise avançada](log-analytics-log-search-portals.md) para analisar dados com consultas de registo. |
| Editar       | Abre a vista no estruturador de vista para editar o seu conteúdo e configuração.  |
| Clone      | Cria uma nova vista e abre-a no estruturador de vistas. O nome do novo modo de exibição é o mesmo que o nome original, mas com *cópia* acrescentado. |
| Intervalo de datas | Defina o filtro de intervalo de data e hora para os dados que estão incluídos na vista. |
| +          | Defina um filtro personalizado que está definido para a vista. |


## <a name="create-a-new-view"></a>Criar uma nova vista
Pode criar uma nova vista no estruturador de vistas, selecionando **estruturador de vistas** no menu da sua área de trabalho do Log Analytics.

![Ver o mosaico do Designer](media/log-analytics-view-designer/view-designer-tile.png)


## <a name="work-with-view-designer"></a>Trabalhar com o estruturador de vistas
Utilize o estruturador de vistas para criar novas vistas ou editar as já existentes. 

Estruturador de vistas tem três painéis: 
* **Design**: contém a vista personalizada que está a criar ou a editar. 
* **Controles**: contém os mosaicos e partes que adicionar à **Design** painel. 
* **Propriedades**: apresenta as propriedades dos mosaicos ou partes selecionados.

![Estruturador de Vista](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-the-view-tile"></a>Configurar o mosaico de vista
Uma vista personalizada pode ter apenas um único mosaico. Para ver o mosaico atual ou selecione um alternativo, selecione o **mosaico** separador a **controle** painel. O **propriedades** painel apresenta as propriedades do mosaico atual. 

Pode configurar as propriedades de mosaico, de acordo com as informações a [referência de mosaico](log-analytics-view-designer-tiles.md) e, em seguida, clique em **aplicar** para guardar as alterações.

### <a name="configure-the-visualization-parts"></a>Configurar as partes de visualização
Uma vista pode incluir qualquer número de partes de visualização. Para adicionar partes a uma vista, selecione o **vista** separador e, em seguida, selecione uma parte da visualização. O **propriedades** painel apresenta as propriedades da parte selecionada. 

Pode configurar as propriedades de exibição, de acordo com as informações a [referência de parte da visualização](log-analytics-view-designer-parts.md) e, em seguida, clique em **aplicar** para guardar as alterações.

As vistas têm apenas uma linha de partes de visualização. Pode reorganizar as partes existentes ao arrastá-los para uma nova localização.

Pode remover uma parte da visualização do vista ao selecionar o **X** na parte superior direita da parte.


### <a name="menu-options"></a>Opções de menu
As opções para trabalhar com modos de exibição no modo de edição são descritas na tabela seguinte.

![Menu Editar](media/log-analytics-view-designer/edit-menu.png)

| Opção | Descrição |
|:--|:--|
| Guardar        | Guarda as alterações e fecha o modo de exibição. |
| Cancelar      | Elimina as suas alterações e fecha o modo de exibição. |
| Eliminar Vista | Elimina a vista. |
| Exportar      | Exporta a exibição para um [modelo Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) que pode importar para outra área de trabalho. O nome do ficheiro é o nome da vista e tem um *omsview* extensão. |
| Importar      | Importa os *omsview* ficheiro que exportou a partir da outra área de trabalho. Esta ação substitui a configuração da vista existente. |
| Clone       | Cria uma nova vista e abre-a no estruturador de vistas. O nome do novo modo de exibição é o mesmo que o nome original, mas com *cópia* acrescentado. |

## <a name="next-steps"></a>Passos Seguintes
* Adicione [mosaicos](log-analytics-view-designer-tiles.md) à sua vista personalizada.
* Adicione [partes de visualização](log-analytics-view-designer-parts.md) à sua vista personalizada.
