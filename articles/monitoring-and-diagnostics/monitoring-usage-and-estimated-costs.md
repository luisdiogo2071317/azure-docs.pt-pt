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
ms.openlocfilehash: ce295c449b01de4fa99df9198805a6b0727c0d18
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="monitoring-usage-and-estimated-costs"></a>Utilização de monitorização e os custos estimados

No hub Monitor do portal do Azure, o **utilização e os custos estimados** página foi concebida para ajudar a compreender a utilização de funcionalidades de monitorização, tais como principal [alertas, métricas, notificações](https://azure.microsoft.com/pricing/details/monitor/), [Iniciar análise](https://azure.microsoft.com/pricing/details/log-analytics/) e [Application Insights](https://azure.microsoft.com/pricing/details/application-insights/). Para clientes nos planos de preços disponíveis antes de Abril de 2018, este também inclui adquirida através das informações de utilização de análise de registos e a oferta de análise.

Nesta página, os utilizadores podem ver os respetivos utilização destes recursos nos últimos 31 dias, agregados por subscrição, com desagregação-ins para ver a tendência de utilização durante este período. Há uma grande quantidade de dados necessários para solicitar em conjunto e fazer esta estimativa, para que tenha ser patient como a página for carregada.
Eis um exemplo que mostra a utilização de monitorização e uma estimativa dos custos resultantes:

![Captura de ecrã portal custos estimados e utilização](./media/monitoring-usage-and-estimated-costs/001.png)

Ao clicar na hiperligação na coluna de utilização mensais abrirá um gráfico que mostra as tendências de utilização durante o último período de 31 dias:

![Incluído por nó captura de ecrã 671.47 GB](./media/monitoring-usage-and-estimated-costs/002.png)

Segue-se a outra utilização semelhante e de custo resumo, neste caso, para uma subscrição no novo Abril de 2018 baseada no consumo preço modelo. Observar a falta de qualquer faturação baseada no nó e esse ingestão de dados e a retenção de análise de registos e o Application Insights são reportados agora uma nova medição comuns.

![Captura de ecrã portal custos estimados e utilização](./media/monitoring-usage-and-estimated-costs/003.png)

## <a name="new-pricing-model"></a>Novo modelo de preços

Em Abril de 2018, foi lançado um novo modelo de preços monitorização.  Esta funcionalidades compatível com a nuvem, com base no consumo de preços. Paga apenas o que utiliza, sem compromissos com base no nó. Estão disponíveis detalhes do novo modelo de preços para [alertas, métricas, notificações](https://azure.microsoft.com/pricing/details/monitor/), [Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/) e [Application Insights](https://azure.microsoft.com/pricing/details/application-insights/).

Para a integração de clientes para análise de registos ou Application Insights após 2 de Abril de 2018, o novo modelo de preços é a única opção. Para os clientes que já estiver a utilizar estes serviços, mover para o novo modelo de preços é opcional.

## <a name="assessing-the-impact-of-the-new-pricing-model"></a>Avaliar o impacto do novo modelo de preços

O novo modelo de preços terá impactos diferentes para cada cliente com base na respetiva monitorização padrões de utilização. Para clientes que já utilizam análise de registos ou Application Insights antes 2 de Abril de 2018, o **utilização e o custo estimado** página no Monitor do Azure fornece uma forma de estimar qualquer alteração custos de se mover para o novo modelo de preços e fornece o Para mover uma subscrição para o novo modelo. Para a maioria dos clientes, o novo modelo de preços será vantajoso, mas para os clientes com padrões de utilização de especialmente elevados de dados ou em regiões de custo mais elevado, pode não ser o caso.

Para ver uma estimativa dos custos para as subscrições que selecionou no **utilização e os custos estimados** página, clique na faixa do azul perto da parte superior da página. É melhor fazer esta subscrição de um momento, uma vez que é que o nível no qual o novo modelo de preços pode ser adotado uma larga maioria.

![Captura de ecrã selecionar modelo de preços](./media/monitoring-usage-and-estimated-costs/004.png)

Agora verá uma versão semelhante desta página com uma faixa verde:

![Captura de ecrã selecionar modelo de preços](./media/monitoring-usage-and-estimated-costs/005.png)

Aqui, verá um conjunto diferente de medidores – medidores que correspondem para o novo modelo de preços. Medidores para ingestão de dados para a instância, tal como

1. Informações e Analytics\Overage por nó
2. Informações e Analytics\Included por nó
3. Dados da aplicação Insights\Basic excedidas
4. Dados da aplicação Insights\Included

são combinados um medidor de ingestão dados comuns nova denominada **partilhado ingestão de Services\Data** , uma vez que o novo modelo de preços não tem a elegibilidade de dados incluído por nó.

Outra alteração irá ver é que os dados ingeridos para análise de registos ou Application Insights no regiões com custos superiores serão apresentados com os novo medidores regionais para corretamente refletir, para a instância **"ingestão de dados (E.U.A. oeste Centro)**.

> [!NOTE]
> Os custos de estimado por subscrição não ter em conta na elegibilidade de nível por nó de conta da subscrição Operations Management Suite (OMS). . Consulte o seu representante de conta para um debate mais aprofundado de novo modelo de preços neste caso.

## <a name="new-pricing-model-and-operations-management-suite-subscription-entitlements"></a>Preços do novo modelo e a elegibilidade de subscrição do Operations Management Suite

Os clientes que já tenham adquirido E1 do Microsoft Operations Management Suite e E2 são elegíveis para elegibilidade de ingestão de dados por nó para [Log Analytics](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite) e [Application Insights](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-pricing#the-price-plans). Para receber estas elegibilidade para a análise de registos áreas de trabalho ou do Application Insights recursos numa determinada subscrição, que a subscrição do modelo de preços tem de permanecer na pré-Abril 2018 preços de modelo onde o análise de registos "por nó (OMS)" escalão de preço e o Application Insights "Empresarial" plano de preços estão disponíveis. Dependendo do número de nós do suite adquirido pela sua organização, mover algumas subscrições para o novo modelo de preços ainda podem ser vantajosos, mas é necessário consideração cuidada. 

## <a name="changes-when-moving-to-the-new-pricing-model"></a>Alterações ao mover para o novo modelo de preços

Mover uma subscrição para o novo modelo de preços irá alterar o escalão de preço para cada análise de registos para uma nova camada por GB e irá mover quaisquer (chamado "pergb2018" no Gestor de recursos do Azure). Movimentação também irá alterar quaisquer recursos do Application Insights no plano de Enterprise para o plano básico. Os efeitos das seguintes são apresentados na estimativa de custo descrita acima. 

## <a name="moving-to-the-new-pricing-model"></a>Mover para o novo modelo de preços

Se tiver optado por adotar o novo modelo de preços para uma subscrição, clique no **seleção do modelo de preços** opção na parte superior do **utilização e os custos estimados** página:

![Monitorizar a utilização de um custos estimados na nova preço modelo captura de ecrã](./media/monitoring-usage-and-estimated-costs/006.png)

Esta ação irá abrir o **seleção do modelo de preços** página, listagem cada uma das subscrições que estava a visualizar a página anterior:

![Captura de ecrã de seleção de modelo de preços](./media/monitoring-usage-and-estimated-costs/007.png)

Para mover uma subscrição para o novo modelo de preços, que apenas a caixa de verificação e clique em **guardar**.  Pode mover novamente para o modelo de preços mais antigo da mesma forma. Tenha em atenção que as permissões de proprietário ou contribuinte da subscrição são necessárias para alterar o modelo de preços.
