---
title: Gerir o custo de dados no Log Analytics do Azure | Documentos da Microsoft
description: Saiba como alterar o plano de preços e gerir a política de volume e retenção de dados para a área de trabalho do Log Analytics no Azure.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: 350c009f77bc37ae2201b1d5c3b701e89301b7aa
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/01/2018
ms.locfileid: "52721702"
---
# <a name="manage-cost-by-controlling-data-volume-and-retention-in-log-analytics"></a>Gerir os custos ao controlar o volume de dados e a retenção do Log Analytics

> [!NOTE]
> Este artigo descreve como controlar os custos do Log Analytics ao definir o período de retenção de dados.  Veja os artigos seguintes para obter informações relacionadas.
> - [Analisar a utilização de dados do Log Analytics](log-analytics-manage-cost-storage.md) descreve como analisar e alerta sobre a utilização de dados.
> - [Monitorizar a utilização e custos estimados](../monitoring-and-diagnostics/monitoring-usage-and-estimated-costs.md) descreve como ver a utilização e custos estimados no Azure de várias funcionalidades para diferentes modelos de preços de monitorização. Também descreve como alterar o modelo de preços.

O log Analytics é criado para dimensionamento e suporte a recolher, indexação e armazenamento de grandes quantidades de dados por dia a partir de qualquer origem na sua empresa ou implementado no Azure.  Embora isso possa ser um controlador primário para a sua organização, relação custo-benefício é, por fim, o driver subjacente. Para esse fim, é importante compreender que o custo de uma área de trabalho do Log Analytics não está apenas com base no volume de dados recolhidos, também é dependente do plano selecionado e o tempo que optar por armazenar os dados gerados a partir de origens ligadas.  

Neste artigo, vamos rever como pode proativamente monitorizar crescimento de volume e o armazenamento de dados e definir limites para controlar os custos associados. 

O custo de dados pode ser considerável consoante os seguintes fatores: 

- Número de sistemas, componentes de infraestrutura, recursos de nuvem, etc. que recolhemos de 
- Tipo de dados criados a origem, tais como filas de mensagens, os registos, eventos, dados relacionados com a segurança ou métricas de desempenho 
- Volume de dados gerados por estas origens e ingeridos à área de trabalho 
- Os dados de período são mantidos na área de trabalho  
- Número de soluções de gestão ativado, a origem de dados e a frequência de coleção 

> [!NOTE]
> Consulte a documentação para cada solução pois ele fornece uma estimativa da quantidade de dados que recolhe.   

Os clientes com um Enterprise Agreement assinado antes de 1 de Julho de 2018 ou que já criou uma área de trabalho do Log Analytics numa subscrição, ainda terá acesso para o *gratuito* plano. Se a sua subscrição não está vinculada a uma inscrição EA já existente, o *gratuito* escalão não está disponível quando criar uma área de trabalho numa nova subscrição depois de 2 de Abril de 2018.  Dados estão limitados a retenção de 7 dias para o *gratuito* escalão.  Para o *autónomo* ou *pago* camada, os dados recolhidos estão disponíveis nos últimos 31 dias. O *gratuito* escalão tem o limite de ingestão de diário de 500 MB e, se encontrar exceder consistentemente as quantidades permitidas volume, pode alterar sua área de trabalho para um plano pago para recolher dados para lá deste limite. 

> [!NOTE]
> Encargos se optar por selecionar um período de retenção mais longo para o escalão pago. Pode alterar o tipo de plano em qualquer altura para obter mais informações sobre os preços, consulte [os detalhes dos preços](https://azure.microsoft.com/pricing/details/log-analytics/). 

Existem duas formas em que o volume de dados pode ser limitada e ajudar a controlam o custo, estes são diária cap e retenção de dados.  

## <a name="review-estimated-cost"></a>Reveja o custo estimado
Log Analytics torna isso fácil de entender o que são provável que os custos basear-se nos padrões de utilização recente.  Para tal, execute os seguintes passos.  

1. Inicie sessão no [Portal do Azure](http://portal.azure.com). 
2. No portal do Azure, clique em **All services** (Todos os serviços). Na lista de recursos, escreva **Log Analytics**. À medida que começa a escrever, a lista filtra com base na sua entrada. Selecione **Log Analytics**.<br><br> ![Portal do Azure](media/log-analytics-manage-cost-storage/azure-portal-01.png)<br><br>  
3. No painel de subscrições do Log Analytics, selecione a sua área de trabalho e, em seguida, clique em **utilização e custos estimados** no painel esquerdo.<br><br> ![Utilização e a página de custos estimados](media/log-analytics-manage-cost-storage/usage-estimated-cost-dashboard-01.png)<br>

A partir daqui pode rever o volume dos seus dados para o mês. Isto inclui todos os dados recebidos e mantidas na sua área de trabalho do Log Analytics.  Clique em **detalhes de utilização** da parte superior da página para ver o dashboard de utilização com informações sobre as tendências de volume de dados de origem, computadores e oferta. Para ver e definir um máximo diário ou para modificar o período de retenção, clique em **gestão de volumes de dados**.
 
Custos de análise de registo são adicionados à sua fatura do Azure. Pode ver detalhes do seu do Azure são faturadas na secção de faturação do portal do Azure ou no [Azure Billing Portal](https://account.windowsazure.com/Subscriptions).  

## <a name="daily-cap"></a>Extremidade diária
Quando criar uma área de trabalho do Log Analytics a partir do portal do Azure, escolha o *gratuito* plano, ele é definido como um 500 MB por limite de dias. Não existe nenhum limite para os planos de preços. Pode configurar um máximo diário e limitar a ingestão diária da sua área de trabalho, mas usar cuidados de saúde, como o seu objetivo não deve ser atingir o limite diário.  Caso contrário, perde os dados para o resto do dia, que pode afetar outros serviços do Azure e soluções cuja funcionalidade pode depender de dados atualizados estarem disponíveis na área de trabalho.  Como resultado, a sua capacidade de observar e receber alertas quando as condições de estado de funcionamento de recursos que suportam serviços de TI são afetadas.  O limite diário destina-se para ser utilizado como uma forma de gerir o aumento inesperado no volume de dados a partir de recursos geridos e mantenha-se no seu limite, ou quando quiser simplesmente a limitar Cobranças não planeadas à sua área de trabalho.  

Quando for atingido o limite diário, a coleção de tipos de dados cobrar deixa para o resto do dia. É apresentada uma faixa de aviso na parte superior da página para a área de trabalho do Log Analytics selecionada e um evento de operação é enviado para o *operação* tabela sob **LogManagement** categoria. Recolha de dados retoma após a hora de reposição definidas em *limite diário terá o valor*. É recomendável definir uma regra de alerta com base em eventos esta operação, configurado para ser notificado quando for atingido o limite diário de dados. 

### <a name="identify-what-daily-data-limit-to-define"></a>Identificar o limite diário de dados para definir 
Revisão [utilização do Log Analytics e custos estimados](log-analytics-usage.md) para compreender as tendências de ingestão de dados e o que é o limite de volume diário para definir. Deve ser considerado com cuidado, uma vez que não será possível monitorizar os seus recursos após ter sido atingido o limite. 

### <a name="manage-the-maximum-daily-data-volume"></a>Gerir o volume de dados máximo diário 
Os passos seguintes descrevem como configurar um limite para gerir o volume de dados do Log Analytics será ingerir por dia.  

1. Na área de trabalho, selecione **Utilização e custos estimados**, no painel do lado esquerdo.
2. Sobre o **utilização e custos estimados** para a área de trabalho selecionada, clique em **gestão de volumes de dados** da parte superior da página. 
5. Limite diário é **OFF** por predefinição – clique em **ON** para ativá-la e, em seguida, defina o limite de volume de dados em GB/dia.<br><br> ![Configurar o limite de dados do log Analytics](media/log-analytics-manage-cost-storage/set-daily-volume-cap-01.png)

### <a name="alert-when-limit-reached"></a>Alertar quando atingido o limite
Embora, apresentamos uma indicação visual no portal do Azure quando o limiar de limite de dados for cumprido, esse comportamento não necessariamente alinhados com como gerir os problemas operacionais que requerem atenção imediata.  Para receber uma notificação de alerta, pode criar uma nova regra de alerta no Azure Monitor.  Para obter mais informações, consulte [como criar, ver e gerir alertas](../monitoring-and-diagnostics/alert-metric.md).      

Para começar, seguem-se as definições recomendadas para o alerta:

* Destino: Selecione o recurso do Log Analytics
* Critérios: 
   * Nome do sinal: pesquisa de registos personalizado
   * Consulta de pesquisa: operação | em que detalhes tem 'OverQuota'
   * Com base em: número de resultados
   * Condição: Maior do que
   * Limiar: 0
   * Período: 5 (minutos)
   * Frequência: 5 (minutos)
* Nome da regra de alerta: limite diário de dados foi atingido
* Gravidade: Aviso (gravidade 1)

Assim que o alerta é definido e o limite for atingido, um alerta é acionado e executa a resposta definida no grupo de ação. Pode notificar a sua equipa através de e-mail e mensagens de texto, ou automatize ações através de webhooks, runbooks de automatização ou [integrar com uma solução ITSM externa](log-analytics-itsmc-overview.md#create-itsm-work-items-from-azure-alerts). 

## <a name="change-the-data-retention-period"></a>Alterar o período de retenção de dados 
Os passos seguintes descrevem como configurar o registo quanto dados são mantidos na sua área de trabalho.
 
1. Na área de trabalho, selecione **Utilização e custos estimados**, no painel do lado esquerdo.
2. Na página **Utilização e custos estimados**, clique em **Gestão de volumes de dados**, na parte superior.
5. No painel, mova o controlo de deslize para aumentar ou diminuir o número de dias e, em seguida, clique em **OK**.  Se estiver a utilizar o *gratuita* escalão, não será capaz de modificar o período de retenção de dados e terá de atualizar para o escalão pago para poder controlar esta definição.<br><br> ![Alteração da definição de retenção de dados de área de trabalho](media/log-analytics-manage-cost-storage/manage-cost-change-retention-01.png)

## <a name="troubleshooting"></a>Resolução de problemas
**Pergunta**: como posso resolver problemas se o Log Analytics já não está a recolher dados? 
**Resposta**: se são gratuito no escalão de preço e ter enviado a mais de 500 MB de dados num dia, deixa de recolha de dados para o resto do dia. Atingir o limite diário é um motivo comum que o Log Analytics interrompe a recolha de dados ou dados parecem estar em falta.  
O log Analytics cria um evento do tipo operação quando a recolha de dados é iniciada e interrompida.  
Execute a seguinte consulta na pesquisa para verificar se estiver a atingir o limite diário e dados em falta: operação | onde OperationCategory = = "Estado de recolha de dados"   
Quando parar a recolha de dados, o OperationStatus indica um aviso. Quando inicia a recolha de dados, o OperationStatus é concluído com êxito.  
A tabela seguinte descreve as razões que pára de recolha de dados e uma ação sugerida para retomar a recolha de dados:  

|Deixa de coleção de razão| Solução| 
|-----------------------|---------|
|Foi atingido o limite diário de dados gratuitos<sup>1</sup>|Aguarde até ao dia seguinte para a coleção reiniciar automaticamente ou alterar para um escalão de preço pago.|
|Foi atingido o limite diário que definiu na gestão de volumes de dados|Aguarde até ao dia seguinte para a coleção reiniciar automaticamente ou aumentar o limite de volume de dados diário descrito [gerir o volume de dados máximo diário](#manage-the-maximum-daily-volume)|
|Subscrição do Azure está num estado suspenso devido a:<br> Versão de avaliação gratuita terminada<br> Passagem do Azure expirou<br> Atingido o limite de gastos mensal (por exemplo, numa assinatura do MSDN ou o Visual Studio)|Converter numa subscrição paga<br> Remover o limite, ou aguarde até que o limite é zerado|

<sup>1</sup> se sua área de trabalho no escalão de preço gratuito, está limitado ao envio de 500 MB de dados por dia para o serviço. Quando atingir o limite diário, deixa de recolha de dados até ao dia seguinte. Dados enviados, enquanto a recolha de dados está parada não indexados e não estão disponíveis para pesquisa. Quando a retoma a recolha de dados, o processamento ocorre apenas para novos dados enviados. 

O log Analytics utiliza hora UTC. O tempo de reposição varia entre as áreas de trabalho para impedir que todos os de áreas de trabalho de limite de início, a ingestão de dados ao mesmo tempo. Se a área de trabalho atinge o limite diário, o processamento continua após a hora de reposição definido no **limite diário terá o valor**.<br><br> ![Fuso horário UTC de limitar o log Analytics](media/log-analytics-manage-cost-storage/data-volume-mgmt-limit-utc.png)

**Pergunta**: como posso ser notificado quando parar a recolha de dados? 
**Resposta**: Utilize os passos descritos no *criar limite de dados diário* alerta para ser notificado quando parar a recolha de dados e siga os passos utilizam os passos descritos em adicionam ações a regras de alerta de configurar um e-mail, webhook ou runbook ação para a regra de alerta. 

## <a name="next-steps"></a>Passos Seguintes  

Para determinar a quantidade de dados é recolhido, quais fontes estão a enviar e os diferentes tipos de dados enviados para o ajudar a gerenciar o consumo e o custo, veja [analisar a utilização de dados do Log Analytics](log-analytics-usage.md).
