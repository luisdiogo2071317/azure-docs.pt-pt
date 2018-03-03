---
title: "Visualização de dependência no Azure migrar | Microsoft Docs"
description: "Fornece uma descrição geral de cálculos de avaliação no serviço Azure migrar."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 2/21/2018
ms.author: raynew
ms.openlocfilehash: bcbb2ace6686e4052149a5dde1ed837a16c36bad
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/02/2018
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

O Azure Migrate está disponível sem custos adicionais. Utilize as funcionalidades de visualização de dependência no Azure migrar necessitam de mapa de serviço. Durante a criação de um projeto migrar do Azure, Azure migrar irá criar automaticamente uma nova área de trabalho de análise de registos em seu nome.

> [!NOTE]
> A funcionalidade de visualização de dependência utiliza o mapa de serviço através de uma área de trabalho de análise de registos. Desde 28 de Fevereiro de 2018, com o anúncio de disponibilidade geral do Azure migrar, a funcionalidade está agora disponível, sem encargos adicionais. Terá de criar um novo projeto para tornar a utilização de área de trabalho de utilização livre. Áreas de trabalho existentes antes de availaibility geral são ainda chargable, por conseguinte, recomendamos que mover para um novo projeto.

1. Utilização de qualquer soluções que não seja o mapa de serviço dentro desta área de trabalho de análise de registos resultará em encargos Log Analytics padrão. 
2. Para suportar cenários de migração sem custos adicionais, a solução de mapa de serviço não será cobrado qualquer custo para o primeiro período de 180 dias da criação do projeto do Azure migrar, após o qual são aplicáveis tarifas padrão.
3. Apenas a área de trabalho criada como parte da criação de projeto, será gratuita para utilização.

Quando registar os agentes para a área de trabalho, utilize o ID e a chave fornecida pelo projeto na página de passos de agente de instalação. Não é possível utilizar uma área de trabalho existente e associá-lo com o projeto do Azure migrar.

Quando o projeto do Azure migrar é eliminado, a área de trabalho não é eliminada, juntamente com o mesmo. Após a eliminação de projeto, a utilização de mapa de serviço não será gratuita e cada nó será cobrada de acordo com a camada paga da área de trabalho de análise de registos.

Saiba mais sobre os preços do Azure Migrate [aqui](https://azure.microsoft.com/pricing/details/azure-migrate/). 

## <a name="how-do-i-manage-the-workspace"></a>Como gerir a área de trabalho?

Pode utilizar a área de trabalho do Log Analytics fora do Azure migrar. Não é eliminada se eliminar o projeto de migração no qual foi criado. Se já não necessita de área de trabalho, [eliminá-la](../log-analytics/log-analytics-manage-access.md) manualmente.

Não elimine a área de trabalho criada pelo Azure migrar, a menos que elimine o projeto de migração. Se o fizer, dependências não funcionam conforme esperado.

## <a name="next-steps"></a>Passos Seguintes

[Máquinas de grupo utilizando dependências de máquina](how-to-create-group-machine-dependencies.md)