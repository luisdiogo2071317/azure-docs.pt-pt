---
title: Gerir o custo de dados do Log Analytics do Azure | Microsoft Docs
description: Saiba como alterar o plano de preços e gestão da política de volume e a retenção de dados para a sua área de trabalho de análise de registos no Azure.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/09/2018
ms.author: magoedte
ms.openlocfilehash: 9a360b41b24f4aca3c3aba29387ecd55faf881b7
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="manage-cost-by-controlling-data-volume-and-retention-in-log-analytics"></a>Gerir os custos ao controlar o volume de dados e a retenção na análise de registos
Análise de registos está concebido para escala e a recolha de suporte, a indexação e armazenar as quantidades enormes de dados por dia de uma origem na sua empresa ou implementado no Azure.  Apesar de este número pode ser um controlador primário para a sua organização, eficiência de custos é basicamente o controlador subjacente. Para esse fim importante compreender que o custo de uma área de trabalho do registo Analytisc não se encontra apenas com base no volume dos dados recolhidos, também está dependente do plano selecionado e o período de tempo que escolheu armazenar dados gerados a partir de origens de ligado.  

Neste artigo, vamos rever como pode proativamente monitorizar crescimento de volume e o armazenamento de dados e definir limites para controlar os custos associados. 

O custo dos dados pode ser considerável consoante os seguintes fatores: 

- Número de sistemas, os componentes de infraestrutura, recursos de nuvem, etc., que está a recolher 
- Tipo de dados criados pela origem, tais como filas de mensagens, os registos, eventos, dados relacionados com segurança ou métricas de desempenho 
- Volume de dados gerados destas origens e ingeridos à área de trabalho 
- Os dados períodos são mantidos na área de trabalho  
- Número de soluções de gestão ativado, a origem de dados e a frequência de recolha 

Consulte a documentação de cada solução como fornece uma estimativa da quantidade de dados recolhe.   

Se no escalão de preço "livre", dados estão limitados a retenção de 7 dias. Camadas de "Por nó (OMS)" ou "Por GB (autónomo)", estão disponíveis dados recolhidos nos últimos 31 dias e retenção pode ser aumentada cópias de segurança para 2 anos. Se selecionar um período de retenção mais são aplicáveis encargos. O plano gratuito tem o limite de ingestão diário de 500 MB e, se achar que exceder consistentemente as quantidades permitidas volume, pode alterar a área de trabalho por GB ou camadas por nó para recolher dados além este limite. Pode alterar o tipo de plano em qualquer altura e para obter mais informações sobre preços, consulte [detalhes de preços](https://azure.microsoft.com/pricing/details/log-analytics/). 

> [!NOTE]
> Em Abril de 2018, iremos [introduzida](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/) um novo modelo de preços para monitorização do Azure. Este modelo adopts um modelo simples "pay as you go" entre o portefólio completado de monitorização de serviços. Saiba mais sobre o [novo modelo de preços](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs), como a [avaliar o impacto de mover para este modelo](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#assessing-the-impact-of-the-new-pricing-model) com base nos seus padrões de utilização, e [como optar ativamente por participar no modelo de nova](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#moving-to-the-new-pricing-model). 

Independentemente do preço modelo ou camada, gerir o volume de dados é fundmental para controlar os custos. Para além da opção e a configuração de solução específica, dentro de análise de registos, existem duas formas em que o volume de dados pode ser limitada e ajudar a controlam o custo, estes são diária retenção de dados e de extremidade.  

## <a name="review-estimated-cost"></a>Reveja o custo estimado
Torna de análise do registo facilitam a compreender o que são provável que os custos com base nos padrões de utilização recente.  Para tal, execute os seguintes passos.  

1. Inicie sessão no [Portal do Azure](http://portal.azure.com). 
2. No portal do Azure, clique em **All services** (Todos os serviços). Na lista de recursos, escreva **Log Analytics**. À medida que começa a escrever, a lista filtra com base na sua entrada. Selecione **Log Analytics**.<br><br> ![Portal do Azure](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
3. No painel de subscrições de análise de registos, selecione a sua área de trabalho e, em seguida, clique em **utilização e os custos estimados** do painel da esquerda.<br><br> ![Página de custos estimados e utilização](media/log-analytics-manage-cost-storage/usage-estimated-cost-dashboard-01.png)<br>

Aqui pode rever o volume de dados para o mês. Isto inclui todos os dados recebidos e mantidas na sua área de trabalho de análise de registos.  Clique em **detalhes de utilização** da parte superior da página para ver o dashboard de utilização com informações sobre tendências de volume de dados por origem, computadores e oferta. Para visualizar e definir um limite diário ou modificar o período de retenção, clique em **gestão de volumes de dados**.
 
Os encargos de análise do registo são adicionados à fatura do Azure. Pode ver os detalhes do seu Azure cobrar na secção de faturação do portal do Azure ou no [Azure Billing Portal](https://account.windowsazure.com/Subscriptions).  

## <a name="daily-cap"></a>Limite diário
Quando criar uma área de trabalho de análise de registos no portal do Azure e escolha o *livres* plano, está definido para um 500 MB por limite de dia. Não há nenhum limite para os planos de preços. Pode configurar um limite diário e limitar a ingestão de diariamente para a sua área de trabalho, mas utilizar cuidado, como o seu objetivo não deve ser para atingiu o limite diário.  Caso contrário nessa altura, perderá os dados para o resto do dia e a capacidade de observar as condições de estado de funcionamento de recursos que suportam serviços de TI é afetada.  A extremidade diária destina-se a ser utilizado como uma forma de gerir o aumento inesperado no volume de dados dos seus recursos geridos e permanecer dentro do seu limite ou quando pretender limitar simplesmente encargos não planeados para a sua área de trabalho.  

Quando for atingido o limite diário, interrompe a coleção de tipos de dados sujeito a faturação para o resto do dia.  É apresentada uma faixa de aviso na parte superior da página para a área de trabalho de análise de registos selecionada e um evento de operação é enviado para o *operação* tabela em **LogManagement** categoria. Retoma a recolha de dados depois do tempo de reposição definidos em *limite diário será definido em*. É recomendável definir uma regra de alerta com base em eventos esta operação, configurado para enviar notificações quando for alcançado o limite diário de dados. 

### <a name="identify-what-daily-data-limit-to-define"></a>Identificar o limite de dados diárias para definir 
Reveja [utilização de análise do registo e os custos estimados](log-analytics-usage.md) para compreender a tendência de ingestão de dados e o que é a extremidade de volume diária para definir. Devem ser considerada com cuidado, dado que não conseguirá monitorizar os recursos após ter sido atingido o limite. 

### <a name="manage-the-maximum-daily-data-volume"></a>Gerir o volume de dados diária máximo 
Os passos seguintes descrevem como configurar um limite para gerir o volume de dados que irá ingestão de análise de registos por dia.  

1. Na área de trabalho, selecione **utilização e os custos estimados** no painel esquerdo.
2. No **utilização e os custos estimados** página para a área de trabalho selecionada, clique em **gestão de volumes de dados** da parte superior da página. 
5. Limite diário é **OFF** por predefinição – clique em **ON** para ativá-la e, em seguida, definir o limite de volume de dados no diário GB.<br><br> ![Análise de registos configurar o limite de dados](media/log-analytics-manage-cost-storage/set-daily-volume-cap-01.png)

### <a name="alert-when-limit-reached"></a>Quando atingido o limite de alertas
Enquanto que apresentam um visual rectângulos no portal do Azure quando é cumprido o limiar de limite de dados, este comportamento não necessariamente são alinhados com como gerir problemas operacionais que necessite de atenção imediata.  Para receber uma notificação de alerta, pode criar uma nova regra de alerta no Monitor do Azure.  Para obter mais informações, consulte [como criar, ver e gerir alertas](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md).      

Para começar, seguem-se as definições recomendadas para o alerta:

* Destino: Selecione o recurso de análise de registos
* Critérios de: 
   * Nome de sinal: pesquisa de registo personalizado
   * Consulta de pesquisa: operação | em que o detalhe tem 'OverQuota'
   * Com base na: número de resultados
   * Condição: Superior a
   * Limiar: 0
   * Período: 5 (minutos)
   * Frequência: 5 (minutos)
* Nome da regra de alerta: foi atingido o limite de dados diária
* Gravidade: Aviso (gravidade 1)

Depois do alerta está definido e o limite é atingido, o alerta é acionado e efetua a resposta definida num grupo de ação. Pode notificar a equipa através de mensagens de e-mail e texto ou automatizar ações utilizando webhooks, runbooks de automatização ou [integrar uma solução ITSM externa](log-analytics-itsmc-overview.md#create-itsm-work-items-from-azure-alerts). 

## <a name="change-the-data-retention-period"></a>Alterar o período de retenção de dados 
Os passos seguintes descrevem como configurar o registo quanto dados são mantidos na sua área de trabalho.
 
1. Na área de trabalho, selecione **utilização e os custos estimados** no painel esquerdo.
2. No **utilização e os custos estimados** página, clique em **gestão de volumes de dados** da parte superior da página.
5. No painel, mova o controlo de deslize para aumentar ou reduzir o número de dias e, em seguida, clique em **OK**.  Se tiver o *livre* camada, não será possível modificar o período de retenção de dados e tem de atualizar para o escalão pago para controlar esta definição.<br><br> ![Alteração da definição de retenção de dados de área de trabalho](media/log-analytics-manage-cost-storage/manage-cost-change-retention-01.png)

## <a name="troubleshooting"></a>Resolução de problemas
**Pergunta**: como resolver se a análise de registos já não está a recolher dados? 
**Resposta**: Se estiver a livre no escalão de preço e ter enviado a mais do que 500 MB de dados dentro de um dia, deixa de recolha de dados para o resto do dia. Atingir o limite diário é um motivo comum que Log Analytics interrompe a recolha de dados ou dados parecem estar em falta.  
Análise de registos cria um evento do tipo operação quando é iniciado e deixa de recolha de dados.  
Execute a seguinte consulta em pesquisa para verificar se está a atingir o limite diário e dados em falta: operação | onde OperationCategory = = 'Status de recolha de dados'   
Quando parar a recolha de dados, o OperationStatus indica um aviso. Quando inicia a recolha de dados, o OperationStatus é concluída com êxito.  
A tabela seguinte descreve as razões que deixa de recolha de dados e uma ação sugerida para retomar a recolha de dados:  

|Interrompe a recolha de razão| Solução| 
|-----------------------|---------|
|Atingiu o limite diário de dados gratuitos<sup>1</sup>|Aguarde até ao dia seguinte para a coleção reiniciar automaticamente ou altere para um escalão de preço pago.|
|Atingiu o limite diário definidos na gestão de volumes de dados|Aguarde até ao dia seguinte para a coleção reiniciar automaticamente, ou aumentar o limite de volume de dados diária descrito [gerir o volume de dados diária máximo](#manage-the-maximum-daily-volume)|
|Subscrição do Azure está num estado suspenso devido a:<br> Avaliação gratuita terminada<br> Passagem do Azure expirou<br> Limite de gastos mensalmente atingido (por exemplo, numa subscrição MSDN ou Visual Studio)|Converter uma subscrição paga<br> Remover o limite ou aguarde que o limite repõe|

<sup>1</sup> se a sua área de trabalho é no escalão de preço gratuito, está limitado a enviar 500 MB de dados por dia para o serviço. Quando atingir o limite diário, deixa de recolha de dados até ao dia seguinte. Dados enviados enquanto a recolha de dados está parada não estão indexados e não estão disponíveis para pesquisa. Quando é retomada a recolha de dados, processamento ocorre apenas para novos dados enviados. 

Análise de registos utiliza a hora UTC. O tempo de reposição varia entre áreas de trabalho para impedir que todas as áreas de trabalho limitado início, a ingestão de dados ao mesmo tempo. Se a área de trabalho atingir o limite diário, processamento retoma depois do tempo de reposição definidos no **limite diário será definido em**.<br><br> ![Análise de registos limita fuso horário UTC](media/log-analytics-manage-cost-storage/data-volume-mgmt-limit-utc.png)

**Pergunta**: como podem posso ser notificado quando parar a recolha de dados? 
**Resposta**: utilizar os passos descritos no *extremidade de dados diária criar* alerta para ser notificado quando parar a recolha de dados e siga os passos utilizam os passos descritos no adicionam ações de regras de alerta configurar um e-mail, o webhook ou o runbook ação de regra de alerta. 

## <a name="next-steps"></a>Passos Seguintes  

Para determinar a quantidade de dados é recolhido, as origens de estão a enviar e os diferentes tipos de dados enviados para o ajudar a gerir consumo e o custo, consulte [analisar a utilização de dados na análise de registos](log-analytics-usage.md).
