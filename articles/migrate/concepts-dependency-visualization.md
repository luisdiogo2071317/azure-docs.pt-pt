---
title: Visualização de dependências no Azure Migrate | Documentos da Microsoft
description: Fornece uma visão geral dos cálculos de avaliação no serviço Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: raynew
ms.openlocfilehash: 5880c98b0f77b75dfb10969311408307b0c4afbd
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45603367"
---
# <a name="dependency-visualization"></a>Visualização de dependência

O [do Azure Migrate](migrate-overview.md) serviços avalia os grupos de máquinas no local para migração para o Azure. Agrupar máquinas, pode utilizar a visualização de dependência. Este artigo fornece informações sobre esta funcionalidade.


## <a name="overview"></a>Descrição geral

Visualização de dependências no Azure Migrate permite-lhe criar grupos para avaliar a migração com maior confiança. Usando a visualização de dependência pode ver as dependências de rede de máquinas específicas, ou num grupo de máquinas. Isso é útil para não garantir que nenhuma funcionalidade ou perdido (ou máquinas esquecidas) no processo de migração, quando as aplicações e cargas de trabalho executam em várias máquinas.  

## <a name="how-does-it-work"></a>Como funciona?

O Azure Migrate utiliza a [mapa de serviço](../operations-management-suite/operations-management-suite-service-map.md) solução em [do Log Analytics](../log-analytics/log-analytics-overview.md) para visualização de dependência.
- Quando cria um projeto de migração do Azure, uma área de trabalho do Log Analytics é criada na sua subscrição.
- O nome de área de trabalho é o nome que especificar para o projeto de migração, o prefixo **migrar-** e, opcionalmente, o sufixo com um número.
- Navegue para a área de trabalho do Log Analytics do **Essentials** secção do projeto **descrição geral** página.
- A área de trabalho criada é marcada com a chave **projeto de migração**e o valor **nome do projeto**. Pode usar esses para pesquisar no portal do Azure.  

    ![Área de trabalho do Log Analytics](./media/concepts-dependency-visualization/oms-workspace.png)

Para utilizar a visualização de dependência, terá de transferir e instalar agentes em cada máquina no local que pretende analisar.  

## <a name="do-i-need-to-pay-for-it"></a>É necessário para pagá-la?

O Azure Migrate está disponível sem custos adicionais. Uso dos recursos de visualização de dependência do Azure Migrate requer o mapa de serviço. A criação de um projeto do Azure Migrate, do Azure Migrate criará automaticamente uma nova área de trabalho do Log Analytics em seu nome.

> [!NOTE]
> A funcionalidade de visualização de dependência utiliza o mapa de serviço por meio de uma área de trabalho do Log Analytics. Desde a 28 de Fevereiro de 2018, com o anúncio da disponibilidade geral do Azure Migrate, a funcionalidade agora está disponível sem custos adicionais. Terá de criar um novo projeto para que utilize a área de trabalho de utilização gratuita. Áreas de trabalho existentes antes de availaibility geral são ainda chargable, por conseguinte, recomendamos que mover para um novo projeto.

1. Utilização de quaisquer soluções que não seja o mapa de serviço dentro desta área de trabalho do Log Analytics incorre em custos padrão do Log Analytics.
2. Para suportar cenários de migração sem custos adicionais, a solução mapa de serviço não será cobrado qualquer custo durante os primeiros 180 dias desde a criação do projeto do Azure Migrate, após o qual os custos padrão serão aplicada.
3. Apenas a área de trabalho criada como parte da criação de projeto, será gratuito para utilização.

Quando registra agentes à área de trabalho, utilize o ID e a chave fornecida pelo projeto na página de passos de agente de instalação. Não é possível utilizar uma área de trabalho existente e associá-la com o projeto do Azure Migrate.

Quando o projeto do Azure Migrate é eliminado, a área de trabalho não é eliminada, juntamente com ele. Publique a eliminação do projeto, a utilização de mapa de serviço não serão gratuita e cada nó será cobrada de acordo com o escalão pago de área de trabalho do Log Analytics.

Saiba mais sobre os preços do Azure Migrate [aqui](https://azure.microsoft.com/pricing/details/azure-migrate/).

## <a name="how-do-i-manage-the-workspace"></a>Como posso gerir a área de trabalho?

Pode utilizar a área de trabalho do Log Analytics fora do Azure Migrate. Não é eliminado se eliminar o projeto de migração no qual foi criado. Se já não precisar de área de trabalho, [eliminá-lo](../log-analytics/log-analytics-manage-access.md) manualmente.

Não elimine a área de trabalho criada pelo Azure Migrate, a menos que excluir o projeto de migração. Se o fizer, as dependências não funcionam conforme esperado.

## <a name="next-steps"></a>Passos Seguintes

[Agrupar máquinas através de dependências de máquina](how-to-create-group-machine-dependencies.md)
