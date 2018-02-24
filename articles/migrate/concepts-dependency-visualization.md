---
title: "Visualização de dependência no Azure migrar | Microsoft Docs"
description: "Fornece uma descrição geral de cálculos de avaliação no serviço Azure migrar."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 2/21/2018
ms.author: raynew
ms.openlocfilehash: 886977764517f1fec89eee77fc3263d30ff9ab31
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/23/2018
---
# <a name="dependency-visualization"></a>Visualização de dependência

O [Azure migrar](migrate-overview.md) serviços avalia os grupos de computadores no local para a migração para o Azure. Para agrupar máquinas, pode utilizar a visualização de dependência. Este artigo fornece informações sobre esta funcionalidade.


## <a name="overview"></a>Descrição geral

Visualização de dependência na migrar do Azure permite-lhe criar grupos para avaliar a migração com maior confiança. Utilizar a visualização de dependência pode ver as dependências de rede de máquinas específicas ou através de um grupo de computadores. Isto é útil para se certificar de que nenhuma funcionalidade ou perdido (ou máquinas esquecidas) no processo de migração, quando as aplicações e cargas de trabalho executadas em várias máquinas.  

## <a name="how-does-it-work"></a>Como funciona?

Azure migrar utiliza o [mapa de serviço](../operations-management-suite/operations-management-suite-service-map.md) solução [Log Analytics](../log-analytics/log-analytics-overview.md) para visualização de dependência.
- Quando cria um projeto de migração do Azure, é criada uma área de trabalho de análise de registos do OMS na sua subscrição.
- O nome da área de trabalho é o nome que especificar para o projeto de migração, o prefixo **migrar-**e, opcionalmente, o sufixo com um número. 
- Navegue para a área de trabalho de análise de registos do **Essentials** secção do projeto **descrição geral** página.
- A área de trabalho criada é etiquetada com a chave **MigrateProject**e o valor **nome do projeto**. Pode utilizá-las para procurar no portal do Azure.  

    ![Área de trabalho do Log Analytics](./media/concepts-dependency-visualization/oms-workspace.png)

Para utilizar a visualização de dependência, terá de transferir e instalar agentes em cada máquina no local que pretende analisar.  

## <a name="do-i-need-to-pay-for-it"></a>É necessário pagar para o mesmo?

Saiba mais sobre os preços do Azure Migrate [aqui](https://azure.microsoft.com/pricing/details/azure-migrate/). 

## <a name="how-do-i-manage-the-workspace"></a>Como gerir a área de trabalho?

Pode utilizar a área de trabalho do Log Analytics fora do Azure migrar. Não é eliminada se eliminar o projeto de migração no qual foi criado. Se já não necessita de área de trabalho, [eliminá-la](../log-analytics/log-analytics-manage-access.md) manualmente.

Não elimine a área de trabalho criada pelo Azure migrar, a menos que elimine o projeto de migração. Se o fizer, dependências não funcionam conforme esperado.

## <a name="next-steps"></a>Passos Seguintes

[Máquinas de grupo utilizando dependências de máquina](how-to-create-group-machine-dependencies.md)