---
title: Criar vistas para analisar os dados no Log Analytics do Azure | Microsoft Docs
description: "Ao utilizar o estruturador de vistas da análise de registos, pode criar vistas personalizadas que são apresentadas no portal do Azure e contêm uma variedade de visualizações de dados na área de trabalho de análise de registos. Este artigo contém uma descrição geral do estruturador de vistas e apresenta os procedimentos para criar e editar vistas personalizadas."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: ce41dc30-e568-43c1-97fa-81e5997c946a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2018
ms.author: bwren
ms.openlocfilehash: 08d0e557f03f771901c9ac92fb080e74e5966452
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="create-custom-views-by-using-view-designer-in-log-analytics"></a>Criar vistas personalizadas utilizando o estruturador de vistas do Log Analytics
Ao utilizar o estruturador de vistas no [Log Analytics do Azure](log-analytics-overview.md), pode criar uma variedade de vistas personalizadas no portal do Azure que pode ajudar a visualizar dados da sua área de trabalho de análise de registos. Este artigo apresenta uma descrição geral do estruturador de vistas e procedimentos para criar e editar vistas personalizadas.

Para obter mais informações sobre o estruturador de vistas, consulte:

* [Mosaico referência](log-analytics-view-designer-tiles.md): Fornece um guia de referência para as definições para todos os mosaicos disponíveis no seu vistas personalizadas.
* [Referência de parte de visualização](log-analytics-view-designer-parts.md): Fornece um guia de referência para as definições para as peças de visualização estão disponíveis no seu vistas personalizadas.

>[!NOTE]
> Se a sua área de trabalho tiver sido atualizada para o [idioma de consulta de análise de registos nova](log-analytics-log-search-upgrade.md), consultas em todas as vistas têm de ser escritas [novo idioma de consulta](https://go.microsoft.com/fwlink/?linkid=856078). Todas as vistas que foram criadas para a área de trabalho foi atualizada são convertidas automaticamente.

## <a name="concepts"></a>Conceitos
As vistas são apresentadas no **descrição geral** página da sua área de trabalho de análise de registos no portal do Azure. Os mosaicos em cada vista personalizada são apresentados por ordem alfabética, e os mosaicos para as soluções estão instalados mesmo espaço de trabalho.

![Página de descrição geral](media/log-analytics-view-designer/overview-page.png)

As vistas que criar com o estruturador de vistas contém os elementos que estão descritos na tabela seguinte:

| Parte | Descrição |
|:--- |:--- |
| Mosaicos | São apresentados na sua área de trabalho de análise de registos **descrição geral** página. Cada mosaico mostra um resumo visual da vista personalizada representa. Cada tipo de mosaico fornece uma visualização diferente dos seus registos. Selecione um mosaico para apresentar uma vista personalizada. |
| Vista personalizada | Apresentado quando seleciona um mosaico. Cada vista contém uma ou mais peças de visualização. |
| Partes de visualização | Apresentar uma visualização de dados na área de trabalho de análise de registos com base num ou vários [pesquisas de registo](log-analytics-log-searches.md). A maioria das partes incluem um cabeçalho, que proporciona uma visualização de alto nível, e obter uma lista, que apresenta os resultados superiores. Cada tipo de parte fornece uma visualização diferente dos registos na área de trabalho de análise de registos. Selecione elementos na parte para efetuar uma pesquisa de registo que fornece registos detalhados. |


## <a name="work-with-an-existing-view"></a>Trabalhar com uma vista existente
Vistas que foram criadas com o estruturador de vistas apresentam as seguintes opções:

![Menu de descrição geral](media/log-analytics-view-designer/overview-menu.png)

As opções são descritas na tabela seguinte:

| Opção | Descrição |
|:--|:--|
| Atualizar   | Atualiza a vista com os dados mais recentes. | 
| Análise | Abre o [portal da análise avançadas](log-analytics-log-search-portals.md#advanced-analytics-portal) para analisar dados com pesquisas de registo. |
| Filtro    | Define um filtro de tempo para os dados que estão incluídos na vista. |
| Editar      | Abre a vista no estruturador de vistas para editar o respetivo conteúdo e a configuração.  |
| Clone     | Cria uma nova vista e abre-o no estruturador de vistas. O nome da nova vista é o mesmo que o nome original, mas com *cópia* anexado ao mesmo. |


## <a name="create-a-new-view"></a>Criar uma nova vista
Pode criar uma nova vista no estruturador de vistas, selecionando o **estruturador de vistas** mosaico a **descrição geral** página da sua área de trabalho de análise de registos.

![Mosaico de vista de Designer](media/log-analytics-view-designer/view-designer-tile.png)


## <a name="work-with-view-designer"></a>Trabalhar com o estruturador de vistas
Utilizar o estruturador de vistas para criar novas vistas ou editar relações existentes. 

Estruturador de vistas tem três painéis: 
* **Design**: contém a vista personalizada que está a criar ou editar. 
* **Controlos**: contém os mosaicos e partes que adiciona à **Design** painel. 
* **Propriedades**: apresenta as propriedades dos mosaicos ou partes selecionadas.

![Estruturador de Vista](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-the-view-tile"></a>Configurar o mosaico de vista
Uma vista personalizada pode ter apenas um único mosaico. Para ver o mosaico atual ou selecione um alternativo, selecione o **mosaico** separador o **controlo** painel. O **propriedades** painel apresenta as propriedades do mosaico atual. 

Pode configurar as propriedades de mosaico, de acordo com as informações de [mosaico referência](log-analytics-view-designer-tiles.md) e, em seguida, clique em **aplicar** para guardar as alterações.

### <a name="configure-the-visualization-parts"></a>Configurar as partes de visualização
Uma vista pode incluir qualquer número de partes de visualização. Para adicionar partes de uma vista, selecione o **vista** separador e, em seguida, selecione uma parte de visualização. O **propriedades** painel apresenta as propriedades da peça selecionada. 

Pode configurar as propriedades de vista, de acordo com as informações de [referência de parte de visualização](log-analytics-view-designer-parts.md) e, em seguida, clique em **aplicar** para guardar as alterações.

Vistas tem apenas uma linha de partes de visualização. Pode reorganizar as partes existentes, arrastando-los para uma nova localização.

Pode remover uma parte de visualização da vista selecionando o **X** na parte superior direito da peça.


### <a name="menu-options"></a>Opções de menu
As opções para trabalhar com vistas no modo de edição são descritas na seguinte tabela.

![Editar menu](media/log-analytics-view-designer/edit-menu.png)

| Opção | Descrição |
|:--|:--|
| Guardar        | Guarda as alterações e fecha a vista. |
| Cancelar      | Perderá as alterações e fecha a vista. |
| Eliminar vista | Elimina a vista. |
| Exportar      | Exporta a vista para um [modelo Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) que pode importar para outra área de trabalho. O nome do ficheiro é o nome da vista e tem um *omsview* extensão. |
| Importar      | Importa o *omsview* ficheiro que exportou a partir da área de trabalho de outra. Esta ação substitui a configuração da vista existente. |
| Clone       | Cria uma nova vista e abre-o no estruturador de vistas. O nome da nova vista é o mesmo que o nome original, mas com *cópia* anexado ao mesmo. |
| Publicar     | Exporta a vista para um ficheiro JSON que pode inserir um [solução de gestão](../operations-management-suite/operations-management-suite-solutions-resources-views.md). O nome de ficheiro é o mesmo que o nome da vista, mas com um *json* extensão. Um segundo ficheiro, é criado com uma *resjson* extensão, inclui os valores para os recursos que estão definidos no ficheiro de JSON.

## <a name="next-steps"></a>Passos Seguintes
* Adicionar [mosaicos](log-analytics-view-designer-tiles.md) à vista personalizada.
* Adicionar [partes de visualização](log-analytics-view-designer-parts.md) à vista personalizada.
