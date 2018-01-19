---
title: Criar vistas para analisar os dados no Log Analytics do Azure | Microsoft Docs
description: "Estruturador de vistas no Log Analytics permite-lhe criar vistas personalizadas que são apresentadas no portal do Azure e contenham visualizações diferentes dos dados na área de trabalho de análise de registos. Este artigo contém uma descrição geral do estruturador de vistas e procedimentos para criar e editar vistas personalizadas."
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
ms.openlocfilehash: a84f40503c1b9778c496461ebbf6864f99bd1c4b
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="use-view-designer-to-create-custom-views-in-log-analytics"></a>Utilize o estruturador de vistas para criar vistas personalizadas no Log Analytics
O estruturador de vistas no [Log Analytics](log-analytics-overview.md) permite-lhe criar vistas personalizadas no portal do Azure que contêm diferentes visualizações de dados da sua área de trabalho de análise de registos. Este artigo contém uma descrição geral do estruturador de vistas e procedimentos para criar e editar vistas personalizadas.

Outros artigos disponíveis para o estruturador de vistas são:

* [Mosaico referência](log-analytics-view-designer-tiles.md) -referência das definições para cada um dos mosaicos disponíveis para utilização na sua vistas personalizadas.
* [Referência de parte de visualização](log-analytics-view-designer-parts.md) -referência das definições para cada um dos mosaicos disponíveis para utilização na sua vistas personalizadas.

>[!NOTE]
> Se a sua área de trabalho tiver sido atualizada para o [idioma de consulta de análise de registos nova](log-analytics-log-search-upgrade.md), e consultas em todas as vistas têm de ser escritas [novo idioma de consulta](https://go.microsoft.com/fwlink/?linkid=856078).  Todas as vistas que foram criadas para a área de trabalho foi atualizada será automtically convertido.

## <a name="concepts"></a>Conceitos
As vistas são apresentadas no **descrição geral** página da sua área de trabalho de análise de registos no portal do Azure.  O mosaico para cada vista personalizada será apresentado por ordem alfabética com os mosaicos para as soluções de instalado o mesmo espaço de trabalho.

![Página de descrição geral](media/log-analytics-view-designer/overview-page.png)

As vistas criadas com o estruturador de vistas contém os elementos na seguinte tabela.

| Parte | Descrição |
|:--- |:--- |
| Mosaico |Apresentado na página de descrição geral para a sua área de trabalho de análise de registos.  Inclui um visual resumir as informações contidas na vista personalizada.  Diferentes tipos de mosaico fornecem visualizações diferentes dos registos.  Clique no mosaico para abrir a vista personalizada. |
| Vista personalizada |Apresentada quando o utilizador clica no mosaico.  Contém um ou mais peças de visualização. |
| Partes de visualização |Visualização de dados na área de trabalho de análise de registos com base num ou mais [pesquisas de registo](log-analytics-log-searches.md).  A maioria das partes irão incluir um cabeçalho que fornece uma visualização de alto nível e uma lista dos resultados da parte superiores.  Tipos de outra parte fornecem visualizações diferentes dos registos na área de trabalho de análise de registos.  Clique nos elementos na parte para efetuar uma pesquisa de registo que fornece registos detalhados. |


## <a name="work-with-an-existing-view"></a>Trabalhar com uma vista existente
Quando abre uma vista que foi criada com o estruturador de vistas, terá um menu com as opções na seguinte tabela.

![Menu de descrição geral](media/log-analytics-view-designer/overview-menu.png)


| Opção | Descrição |
|:--|:--|
| Atualizar   | Atualize a vista com os dados mais recentes. | 
| Análise | Abra o [portal da análise avançadas](log-analytics-log-search-portals.md#advanced-analytics-portal) para analisar dados com o registo pesquisas. ( log-Analytics-log-Search-portals.md#Advanced-Analytics-Portal). |
| Filtro    | Defina um filtro de tempo dos dados incluídos na vista. |
| Editar      | Abra a vista no estruturador de vistas para editar o respetivo conteúdo e a configuração.   |
| Clone     | Criar uma nova vista e abri-lo no estruturador de vistas.  A nova vista tem o mesmo nome que o original com "copiar" anexado ao fim do mesmo. |


## <a name="create-a-new-view"></a>Criar uma nova vista
Criar uma nova vista na **estruturador de vistas** clicando no mosaico do estruturador de vistas, na página de descrição geral da sua área de trabalho de análise de registos no portal do Azure.

![Mosaico de vista de Designer](media/log-analytics-view-designer/view-designer-tile.png)


## <a name="working-with-view-designer"></a>Trabalhar com o estruturador de vistas
Irá funcionar com o estruturador de vistas, se estiver a criar uma nova vista ou editar uma existente.  

O estruturador de vistas tem três painéis.  O **Design** painel contém a vista personalizada que está a criar ou editar.  Adicionar mosaicos e partes do que o **controlo** painel para o **Design** painel.  O **propriedades** painel irá apresentar as propriedades para o mosaico ou peça selecionada.

![Estruturador de Vista](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-view-tile"></a>Configurar o mosaico de vista
Uma vista personalizada pode ter apenas um único mosaico.  Selecione o **mosaico** separador o **controlo** painel para ver o mosaico atual ou selecione um alternativo.  O **propriedades** painel irá apresentar as propriedades para o mosaico atual.  Configurar as propriedades de mosaico, de acordo com as informações detalhadas a [mosaico referência](log-analytics-view-designer-tiles.md) e clique em **aplicar** ao guardar as alterações.

### <a name="configure-visualization-parts"></a>Configurar partes de visualização
Uma vista pode incluir qualquer número de partes de visualização.  Selecione o **vista** separador e, em seguida, uma parte de visualização para adicionar à vista.  O **propriedades** painel irá apresentar as propriedades para a parte selecionada.  Configurar as propriedades de vista, de acordo com as informações detalhadas a [referência de parte de visualização](log-analytics-view-designer-parts.md) e clique em **aplicar** ao guardar as alterações.

Vistas tem apenas uma linha de partes de visualização.  Reorganizar partes existentes numa vista, clicando e arrastando-los para uma nova localização.

Pode remover uma parte de visualização da vista clicando a **X** botão no canto superior direito da peça.


### <a name="menu-options"></a>Opções de menu
Quando estiver a trabalhar com uma vista em modo de edição, terá das opções de menu na seguinte tabela.

![Editar menu](media/log-analytics-view-designer/edit-menu.png)

| Opção | Descrição |
|:--|:--|
| Guardar        | Guardar as alterações e fechar a vista. |
| Cancelar      | Rejeitar as alterações e fechar a vista. |
| Eliminar vista | Elimine a vista. |
| Exportar      | Exportar a vista para um [modelo do Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) que pode importar para outra área de trabalho.  O nome do ficheiro será o nome da vista com a extensão *omsview*. |
| Importar      | Importar um *omsview* ficheiro que exportou a partir da área de trabalho de outra.  Isto irá substituir a configuração da vista existente. |
| Clone       | Criar uma nova vista e abri-lo no estruturador de vistas.  A nova vista tem o mesmo nome que o original com "copiar" anexado ao fim do mesmo. |
| Publicar     | Exportar a vista para um ficheiro JSON que pode ser inserido num [Mangagement solução](../operations-management-suite/operations-management-suite-solutions-resources-views.md).  O nome do ficheiro será o nome da vista com a extensão *json*. Um segundo ficheiro é criado com a extensão *resjson* que inclui os valores para recursos definidos no ficheiro de JSON.

## <a name="next-steps"></a>Passos Seguintes
* Adicionar [mosaicos](log-analytics-view-designer-tiles.md) à vista personalizada.
* Adicionar [partes de visualização](log-analytics-view-designer-parts.md) à vista personalizada.
