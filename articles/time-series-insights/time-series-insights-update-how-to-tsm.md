---
title: Modelação de dados na Azure Time Series Insights | Documentos da Microsoft
description: Noções básicas sobre a modelação de dados na Azure Time Series Insights
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 11/30/2018
ms.openlocfilehash: 926d1a35cb10d50ddeacbed5476e2dcf14d0999d
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856633"
---
# <a name="data-modeling-in-azure-time-series-insights"></a>Modelação de dados na Azure Time Series Insights

Este documento descreve como trabalhar com **modelos de série de tempo** seguindo o Azure Time Series Insights (pré-visualização). Ele detalha os vários cenários de dados comuns.

Leitura a [Azure TSI (pré-visualização) Explorer](./time-series-insights-update-explorer.md) artigo, para saber mais sobre como navegar na pré-visualização privada.

## <a name="types"></a>Tipos

### <a name="how-to-create-a-single-type"></a>Como criar um único tipo

1. Comece por cabeçalho para o painel de Seletor de modelo TSM e seleciona os tipos a partir do menu. Em seguida, fechar o painel para se concentrar em tipos TSM:

    ![portal_one][1]

1. Clique em **Adicionar**.
1. Todos os detalhes relativos a tipos de entrada e clique em **criar**. Se o fizer, deve criar tipos no ambiente:

    ![portal_two][2]

### <a name="how-to-bulk-upload-one-or-more-types"></a>Como em massa carregar um ou mais tipos

1. Clique em **carregar JSON**.
1. Escolha esse ficheiro que contém a carga de tipo.
1. Clique em **carregar**

    ![portal_three][3]

### <a name="how-to-edit-a-single-type"></a>Como editar um único tipo

* Selecione o tipo e clique em **editar** botão. Efetue as alterações necessárias e clique em **guardar**:

    ![portal_four][4]

### <a name="how-to-delete-a-type"></a>Como eliminar um tipo

* Selecione o tipo e clique em **eliminar** botão. Se não existem instâncias estão associadas aos tipos, vão ser eliminada:

    ![portal_five][5]

## <a name="hierarchies"></a>Hierarquias

### <a name="how-to-create-a-single-hierarchy"></a>Como criar uma única hierarquia

1. Comece por cabeçalho para o painel de Seletor de modelo TSM e selecione hierarquias no menu. Em seguida, fechar o painel para se concentrar em tipos TSM:

    ![portal_six][6]

1. Clique em **adicionar**

    ![portal_seven][7]

1. Clique em **nível adicionar** no painel da direita:

    ![portal_eight][8]

1. Introduza os detalhes de hierarquia e clique em **criar**:

    ![portal_nine][9]

### <a name="how-to-bulk-upload-one-or-more-hierarchies"></a>Como em massa carregar um ou mais hierarquias

1. Clique em **carregar JSON**.
1. Escolha o ficheiro que contém o payload de hierarquia.
1. Clique em **carregar**:

    ![portal_ten][10]

### <a name="how-to-edit-a-single-hierarchy"></a>Como editar uma única hierarquia

* Selecione a hierarquia e clique nas **editar** botão. Efetue as alterações necessárias e clique em **guardar**:

    ![portal_eleven][11]

### <a name="how-to-delete-a-hierarchy"></a>Como eliminar uma hierarquia

* Selecione a hierarquia e clique em **eliminar** botão. Se não existem instâncias estão associadas à hierarquia, este será eliminado.

    ![portal_twelve][12]

## <a name="instances"></a>Instâncias

### <a name="how-to-create-a-single-instance"></a>Como criar uma única instância

1. Comece por cabeçalho para o painel de Seletor de modelo TSM e selecione as instâncias no menu. Em seguida, fechar o painel para se concentrar em tipos TSM:

    ![portal_thirteen][13]

1. Clique em **adicionar**:

    ![portal_fourteen][14]

1. Introduza os detalhes de instância, selecione a associação de tipo e a hierarquia e clique em **criar**.

### <a name="how-to-bulk-upload-one-or-more-instances"></a>Como em massa carregar uma ou mais instâncias

1. Clique em **carregar JSON**.
1. Escolha o ficheiro que contém a carga de instâncias:

    ![portal_fifteen][15]

1. Clique em **carregar**.

### <a name="how-to-edit-a-single-instance"></a>Como editar uma única instância

* Selecione a instância e clique nas **editar** botão. Efetue as alterações necessárias e clique em **guardar**:

    ![portal_sixteen][16]

### <a name="how-to-delete-an-instance"></a>Como eliminar uma instância

* Selecione a instância e clique em **eliminar** botão. Se não existem eventos estão associados às instâncias, serão eliminado.

## <a name="next-steps"></a>Passos Seguintes

Leia [modelos de série de tempo](./time-series-insights-update-tsm.md) para obter mais informações sobre **modelos de série de tempo**.

Ver o Azure TSI (pré-visualização) Explorer [artigo](./time-series-insights-update-explorer.md) para saber mais sobre a pré-visualização.

<!-- Images -->
[1]: media/v2-update-how-to-tsm/portal_one.png
[2]: media/v2-update-how-to-tsm/portal_two.png
[3]: media/v2-update-how-to-tsm/portal_three.png
[4]: media/v2-update-how-to-tsm/portal_four.png
[5]: media/v2-update-how-to-tsm/portal_five.png
[6]: media/v2-update-how-to-tsm/portal_six.png
[7]: media/v2-update-how-to-tsm/portal_seven.png
[8]: media/v2-update-how-to-tsm/portal_eight.png
[9]: media/v2-update-how-to-tsm/portal_nine.png
[10]: media/v2-update-how-to-tsm/portal_ten.png
[11]: media/v2-update-how-to-tsm/portal_eleven.png
[12]: media/v2-update-how-to-tsm/portal_twelve.png
[13]: media/v2-update-how-to-tsm/portal_thirteen.png
[14]: media/v2-update-how-to-tsm/portal_fourteen.png
[15]: media/v2-update-how-to-tsm/portal_fifteen.png
[16]: media/v2-update-how-to-tsm/portal_sixteen.png