---
title: Monitorização de utilização e os custos estimados no Monitor do Azure | Microsoft Docs
description: Descrição geral do processo de utilizar a página de custos estimados e utilização de Monitor do Azure
author: dalekoetke
manager: carmonmills
editor: mrbullwinkle
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/09/2018
ms.author: Dale.Koetke;mbullwin
ms.openlocfilehash: 08991565d56ffbf7d798944f108a1b86e4463c58
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="monitoring-usage-and-estimated-costs"></a>Utilização de monitorização e os custos estimados

No hub Monitor do portal do Azure, o **utilização e os custos estimados** página explica a utilização de funcionalidades de monitorização, tais como principal [alertas, métricas, notificações](https://azure.microsoft.com/pricing/details/monitor/), [Log Analytics do Azure ](https://azure.microsoft.com/pricing/details/log-analytics/), e [Azure Application Insights](https://azure.microsoft.com/pricing/details/application-insights/). Para clientes nos planos de preços disponíveis antes de Abril de 2018, este também inclui comprada através das informações de utilização de análise de registos e a oferta de análise.

Nesta página, os utilizadores podem ver os respetivos utilização de recursos nos últimos 31 dias, agregados por subscrição. Exercício-ins Mostrar tendências de utilização durante o período de 31 dias. Uma grande quantidade de dados tem uma combinação de para esta estimativa, por isso tenha ser patient como a página for carregada.

Este exemplo mostra a utilização de monitorização e uma estimativa dos custos resultantes:

![Captura de ecrã portal custos estimados e utilização](./media/monitoring-usage-and-estimated-costs/001.png)

Selecione a hiperligação na coluna de utilização mensais para abrir um gráfico que mostra as tendências de utilização durante o último período de 31 dias:

![Incluído por nó barra captura de ecrã do gráfico](./media/monitoring-usage-and-estimated-costs/002.png)

Segue-se a outra utilização semelhante e o resumo de custo. Este exemplo mostra uma subscrição no novo Abril de 2018 baseada no consumo preço modelo. Tenha em atenção a falta de qualquer faturação baseada no nó. Ingestão de dados e o período de retenção para análise de registos e o Application Insights agora são reportados num medidor comuns de novo.

![Utilização e os custos estimados captura de ecrã portal - de 2018 de Abril de preços](./media/monitoring-usage-and-estimated-costs/003.png)

## <a name="new-pricing-model"></a>Novo modelo de preços

Em Abril de 2018, um [monitorização novo modelo de preços foi lançada](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/).  Esta funcionalidades compatível com a nuvem, com base no consumo de preços. Paga apenas o que utiliza, sem compromissos com base no nó. Estão disponíveis detalhes do novo modelo de preços para [alertas, métricas, notificações](https://azure.microsoft.com/pricing/details/monitor/), [Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/) e [Application Insights](https://azure.microsoft.com/pricing/details/application-insights/). 

Para a integração de clientes para análise de registos ou Application Insights após 2 de Abril de 2018, o novo modelo de preços é a única opção. Para os clientes que já utilizam estes serviços, mover para o novo modelo de preços é opcional.

## <a name="assessing-the-impact-of-the-new-pricing-model"></a>Avaliar o impacto do novo modelo de preços
O novo modelo de preços terá impactos diferentes em cada cliente com base na respetiva monitorização padrões de utilização. Para os clientes que estava a utilizar o análise de registos ou Application Insights antes de 2 de Abril de 2018, o **utilização e o custo estimado** página no Azure Monitor calcula a partir de qualquer alteração custos de se poderem ser movido para o novo modelo de preços. Fornece a forma de mover uma subscrição para o novo modelo. Para a maioria dos clientes, o novo modelo de preços será vantajoso. Para clientes com padrões de utilização de especialmente elevados de dados ou em regiões de custo mais elevado, não pode ser o caso.

Para ver uma estimativa dos custos para as subscrições que escolheu no **utilização e os custos estimados** página, selecione a faixa azul perto da parte superior da página. É melhor efetuar esta subscrição de um momento, uma vez que é o nível no qual o novo modelo de preços pode ser adotado uma larga maioria.

![Monitorizar a utilização e os custos estimados no novo preço captura de ecrã do modelo](./media/monitoring-usage-and-estimated-costs/004.png)

A nova página mostra uma versão da página anterior com uma faixa verde semelhante:

![Monitorizar a utilização e os custos estimados atual preço captura de ecrã do modelo](./media/monitoring-usage-and-estimated-costs/005.png)

A página também mostra um conjunto diferente de medidores que correspondem para o novo modelo de preços. Esta lista é um exemplo:

- Informações e Analytics\Overage por nó
- Informações e Analytics\Included por nó
- Dados da aplicação Insights\Basic excedidas
- Dados da aplicação Insights\Included

O novo modelo de preços não tem as alocações de dados de incluídas com base no nó. Por conseguinte, estes medidores de ingestão de dados são combinados um medidor de ingestão dados comuns nova denominada **partilhado ingestão de Services\Data**. 

Há outra alteração aos dados ingeridos para análise de registos ou Application Insights no regiões com custos superiores. Dados para estas regiões de custo de alta serão apresentados com os novo medidores regionais. Um exemplo é **ingestão de dados (dos EUA oeste Central)**.

> [!NOTE]
> O por subscrição estimar os custos de não ter em conta no nível de conta por elegibilidade do nó da subscrição Operations Management Suite (OMS). Consulte neste caso, o seu representante de conta para um debate mais aprofundado de novo modelo de preços.

## <a name="new-pricing-model-and-operations-management-suite-subscription-entitlements"></a>Preços do novo modelo e a elegibilidade de subscrição do Operations Management Suite

Os clientes que já tenham adquirido E1 do Microsoft Operations Management Suite e E2 são elegíveis para elegibilidade de ingestão de dados por nó para [Log Analytics](https://www.microsoft.com/cloud-platform/operations-management-suite) e [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-pricing#the-price-plans). Para receber a elegibilidade para áreas de trabalho de análise de registos ou recursos do Application Insights numa determinada subscrição: 

- Modelo de preços da subscrição tem de permanecer no modelo de pre-Abril 2018.
- Áreas de trabalho de análise de registo devem utilizar o "por nó (OMS)" escalão de preço.
- Recursos do Application Insights devem utilizar o plano de preços "empresarial".

Dependendo do número de nós do suite adquirido pela sua organização, mover algumas subscrições para o novo modelo de preços pode ser vantajosos, mas é necessário consideração cuidada. Em geral, é recomendado simplesmente para se manter no modelo de 2018 de Abril de pré-instalação, conforme descrito acima.

> [!WARNING]
> Se a sua organização tiver comprado o E1 do Microsoft Operations Management Suite e E2,-normalmente, é melhor manter as suas subscrições no modelo de preços do pre-Abril 2018. 
>

## <a name="changes-when-youre-moving-to-the-new-pricing-model"></a>Alterações quando estiver a mover para o novo modelo de preços

O novo modelo de preços simplifica a análise de registos e o Application Insights preços opções para apenas um único escalão (ou estiver a planear). Mover uma subscrição para o novo modelo será de preços:

- Altere o escalão de preço para cada análise de registos para uma nova camada por GB (denominada "pergb2018" no Gestor de recursos do Azure)
- Quaisquer recursos do Application Insights no plano de Enterprise é alterado para o plano básico.

A estimativa de custo mostra os efeitos destas alterações.

> [!WARNING]
> Aqui uma nota importante se utilizar o Azure Resource Manager ou o PowerShell para implementar [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-template-workspace-configuration) ou [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-powershell) numa subscrição movido para o novo modelo de preços. Se especificar um camada/plano de preços que não seja "pergb2018" para análise de registos ou "Básico" para o Application Insights, vez a falhar a implementação devido à especificação de um inválido/plano, de escalão de preço será bem sucedida **mas irá utilizar apenas válido / plano de escalão de preço**. 
>

## <a name="moving-to-the-new-pricing-model"></a>Mover para o novo modelo de preços

Se tiver optado por adotar o novo modelo de preços para uma subscrição, selecione o **seleção do modelo de preços** opção na parte superior do **utilização e os custos estimados** página:

![Monitorizar a utilização e os custos estimados no novo preço captura de ecrã do modelo](./media/monitoring-usage-and-estimated-costs/006.png)

O **seleção do modelo de preços** será aberta a página. Mostra uma lista de cada uma das subscrições visualizados na página anterior:

![Captura de ecrã de seleção de modelo de preços](./media/monitoring-usage-and-estimated-costs/007.png)

Para mover uma subscrição para o novo modelo de preços, basta selecionar a caixa e, em seguida, selecione **guardar**. Pode mover novamente para o modelo de preços mais antigo da mesma forma. Tenha em atenção que proprietário da subscrição ou são necessárias permissões de contribuinte para alterar o modelo de preços.
