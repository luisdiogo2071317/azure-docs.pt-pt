---
title: Funcionalidades de pedido de dados de cliente das informações de séries de tempo do Azure
description: Resumo das funcionalidades de pedido de dados de cliente.
author: ashannon7
ms.author: dobett
manager: timlt
ms.date: 05/17/2018
ms.topic: conceptual
ms.service: time-series-insights
services: time-series-insights
ms.openlocfilehash: 5714782d4fe44ce4521dd604055e925937a7328d
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295283"
---
# <a name="summary-of-customer-data-request-features"></a>Resumo das funcionalidades de pedido de dados de cliente

As informações de séries de tempo do Azure é um serviço em nuvem gerido com componentes de armazenamento, a análise e visualização que torna mais fácil de inserção, armazenar, explorar e analisar billions de eventos em simultâneo.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

Para ver, exportar e eliminar os dados pessoais que poderão estar sujeitas a um pedido de assunto de dados, um administrador de inquilino de informações de séries de tempo do Azure pode utilizar o portal do Azure ou as APIs REST. No portal do Azure para pedidos de assunto de dados do serviço, fornece um método menos complexo para realizar estas operações que preferir a maior parte dos utilizadores.

## <a name="identifying-customer-data"></a>Identificar dados de cliente

As informações de séries de tempo do Azure considera de dados pessoais dados associados a administradores e utilizadores das informações de séries de tempo. Informações de séries de tempo armazena o ID de objeto do Azure Active Directory dos utilizadores com acesso ao ambiente. O portal do Azure apresenta endereços de e-mail do utilizador, mas estes endereços de e-mail não são armazenados no Insights de séries de tempo, estes são dinamicamente pesquisados utilizando o Azure Active Directory-ID de objeto no Azure Active Directory.

## <a name="deleting-customer-data"></a>Eliminar dados de cliente

Um administrador inquilino pode eliminar os dados de cliente no portal do Azure.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

No entanto, antes de eliminar dados de cliente através do portal, deve remover as políticas de acesso do utilizador do ambiente de informações de séries de tempo dentro do portal do Azure. Para obter mais informações, consulte [conceder acesso a dados para um ambiente de informações de séries de tempo utilizando o portal do Azure](time-series-insights-data-access.md).

Também pode efetuar operações de eliminação nas políticas de acesso utilizando a API REST. Para obter mais informações, consulte [eliminar políticas de acesso -](https://docs.microsoft.com/rest/api/time-series-insights-management/accesspolicies/delete).

Informações de séries de tempo é integrada com o painel de política no portal do Azure. Informações de séries de tempo e o painel política permitem-lhe ver, exportar e eliminar dados de utilizador armazenados no serviço. Elimine qualquer ação tomada no painel de política de resultados do portais do Azure na eliminação de dados de utilizador dentro das informações de séries de tempo. Por exemplo, se um utilizador tiver uma consulta guardada pessoal, essa consulta é permanentemente eliminada do Explorador de informações de séries de tempo. Se o utilizador tem uma consulta guardada partilhada, a consulta persistir, mas as informações do utilizador são eliminadas permanentemente. A nota seguinte contém instruções sobre como realizar estas tarefas.

## <a name="exporting-customer-data"></a>Exportar dados de cliente

Da mesma forma para eliminar dados, um administrador inquilino pode ver e exportar dados armazenados no Insights de séries de tempo do painel da política no portal do Azure.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

Se for um administrador inquilino, pode ver as políticas de acesso de dados no ambiente Insights de séries de tempo no portal do Azure. Para obter mais informações, consulte [conceder acesso a dados para um ambiente de informações de séries de tempo utilizando o portal do Azure](time-series-insights-data-access.md).

Também é possível efetuar operações de exportação em políticas de acesso com a operação de "lista pelo ambiente" na API REST fornecido. Para obter mais informações, consulte [políticas de acesso - lista pelo ambiente](https://docs.microsoft.com/rest/api/time-series-insights-management/accesspolicies/listbyenvironment).

## <a name="to-delete-data-stored-within-time-series-insights"></a>Para eliminar os dados armazenados num Insights de séries de tempo

Dados pessoais podem tornar a caminho para o armazenamento de informações de séries de tempo, outro cenário do utilizador e os dados de administrador. Se considerar os dados armazenados na série de tempo informações como dados pessoais, pode exportar e eliminar os dados utilizando os seguintes passos:

**Ver e exportar dados**

Para ver e exportar dados armazenados na Insights de séries de tempo, terá de procurar dados. Pode utilizar o Explorador de informações de séries de tempo ou a consulta de informações de séries de tempo APIs para ver e exportar dados. Para ver e exportar dados com o Explorador de informações de séries de tempo, primeiro procurar para localizar os dados de utilizador em causa. Depois de procurar, faça duplo clique no gráfico e selecione **explorar eventos**. Grelha de eventos aparece e apresenta as opções para exportar os dados como CSV e JSON.

Para obter mais informações, consulte [Explorador de informações de séries de tempo de Azure](time-series-insights-explorer.md).

**Eliminar dados**

Atualmente, informações de séries de tempo não suporta granular eliminação de dados. No entanto, informações de séries de tempo fornece a capacidade de remover dados de cliente armazenados dentro das informações de séries de tempo ao configurar as políticas de retenção. Pode ajustar o período de retenção de todo o ambiente de informações de séries de tempo para qualquer número de dias para suportar os requisitos de eliminação.

Para obter mais informações, consulte [configurar a retenção de informações de séries de tempo](time-series-insights-how-to-configure-retention.md).