---
title: Utilização de monitorização e os custos estimados no Monitor do Azure
description: Descrição geral do processo de utilizar a página de custos estimados e utilização de Monitor do Azure
author: dalekoetke
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/31/2018
ms.author: mbullwin
ms.reviewer: Dale.Koetke
ms.component: ''
ms.openlocfilehash: edfcc244105403ae33251777c560d4cc21dfe5cb
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35264287"
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
> Aqui uma nota importante se utilizar o Azure Resource Manager ou o PowerShell para implementar [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-template-workspace-configuration) ou [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-powershell) numa subscrição movido para o novo modelo de preços. Se especificar um camada/plano de preços que não seja "pergb2018" para análise de registos ou "Básico" para o Application Insights, vez a falhar a implementação devido à especificação de um inválido/plano, de escalão de preço será bem sucedida **mas irá utilizar apenas válido / plano de escalão de preço** (isto não é aplicável para o escalão gratuito de análise do registo em que é gerada uma mensagem de escalão de preço inválido).
>

## <a name="moving-to-the-new-pricing-model"></a>Mover para o novo modelo de preços

Se tiver optado por adotar o novo modelo de preços para uma subscrição, selecione o **seleção do modelo de preços** opção na parte superior do **utilização e os custos estimados** página:

![Monitorizar a utilização e os custos estimados no novo preço captura de ecrã do modelo](./media/monitoring-usage-and-estimated-costs/006.png)

O **seleção do modelo de preços** será aberta a página. Mostra uma lista de cada uma das subscrições visualizados na página anterior:

![Captura de ecrã de seleção de modelo de preços](./media/monitoring-usage-and-estimated-costs/007.png)

Para mover uma subscrição para o novo modelo de preços, basta selecionar a caixa e, em seguida, selecione **guardar**. Pode mover novamente para o modelo de preços mais antigo da mesma forma. Tenha em atenção que proprietário da subscrição ou são necessárias permissões de contribuinte para alterar o modelo de preços.

## <a name="automate-moving-to-the-new-pricing-model"></a>Automatizar a mover para o novo modelo de preços

Os scripts abaixo requerem o módulo do PowerShell do Azure. Para verificar se tem a versão mais recente, consulte [módulo Azure PowerShell instalar](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.1.0).

Assim que tiver a versão mais recente do Azure PowerShell, primeiro terá de executar ``Connect-AzureRmAccount``.

``` PowerShell
# To check if your subscription is eligible to adjust pricing models.
$ResourceID ="/subscriptions/<Subscription-ID-Here>/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action listmigrationdate `
 -Force
```

Um resultado de True em isGrandFatherableSubscription indica que o modelo de preços esta subscrição pode ser movido entre modelos de preços. A falta de um valor em optedInDate significa que esta subscrição está atualmente configurada para o modelo de preços antigo.

```
isGrandFatherableSubscription optedInDate
----------------------------- -----------
                         True            
```

A migração desta subscrição para o novo modelo de preços execute:

```PowerShell
$ResourceID ="/subscriptions/<Subscription-ID-Here>/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action migratetonewpricingmodel `
 -Force
```

Para confirmar que a alteração foi efetuada com êxito volte a executar:

```PowerShell
$ResourceID ="/subscriptions/<Subscription-ID-Here>/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action listmigrationdate `
 -Force
```

Se a migração foi bem sucedida, o resultado deve agora ter o seguinte aspeto:

```
isGrandFatherableSubscription optedInDate                      
----------------------------- -----------                      
                         True 2018-05-31T13:52:43.3592081+00:00
```

Agora, o optInDate contém um carimbo de quando esta subscrição incluídos no novo modelo de preços.

Se precisar de reverter para o modelo de preços antigo, executaria:

```PowerShell
 $ResourceID ="/subscriptions/<Subscription-ID-Here>/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action rollbacktolegacypricingmodel `
 -Force
```

Se, em seguida, voltar a executar o script anterior que tenha ``-Action listmigrationdate``, deverá ver um valor de optedInDate vazia que indica a subscrição foi devolvida para o modelo de preços de legado.

Se tiver várias subscrições, o que pretende migrar que estão alojadas no mesmo inquilino pode criar o seus próprios variante utilizando partes dos seguintes scripts:

```PowerShell
#Query tenant and create an array comprised of all of your tenants subscription ids
$TenantId = <Your-tenant-id>
$Tenant =Get-AzureRMSubscription -TenantId $TenantId
$Subscriptions = $Tenant.Id
```

Para verificar se todas as subscrições no seu inquilino são elegíveis para o novo modelo de preços, pode executar:

```PowerShell
Foreach ($id in $Subscriptions)
{
$ResourceID ="/subscriptions/$id/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action listmigrationdate `
 -Force
}
```

O script pode ser avançado adicionais através da criação de um script que gera três matrizes. Uma matriz irá ser composto por todos os id de subscrição que tenham ```isGrandFatherableSubscription``` definida como True e optedInDate não tem atualmente um valor. Uma segundo matriz de quaisquer subscrições atualmente no novo modelo de preços. E uma terceira matriz preenchida apenas com ids de subscrição no seu inquilino, que não são elegíveis para o novo modelo de preços:

```PowerShell
[System.Collections.ArrayList]$Eligible= @{}
[System.Collections.ArrayList]$NewPricingEnabled = @{}
[System.Collections.ArrayList]$NotEligible = @{}

Foreach ($id in $Subscriptions)
{
$ResourceID ="/subscriptions/$id/providers/microsoft.insights"
$Result= Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action listmigrationdate `
 -Force

     if ($Result.isGrandFatherableSubscription -eq $True -and [bool]$Result.optedInDate -eq $False)
     {
     $Eligible.Add($id)
     }

     elseif ($Result.isGrandFatherableSubscription -eq $True -and [bool]$Result.optedInDate -eq $True)
     {
     $NewPricingEnabled.Add($id)
     }

     elseif ($Result.isGrandFatherableSubscription -eq $False)
     {
     $NotEligible.add($id)
     }
}
```

> [!NOTE]
> Dependendo do número de subscrições, o script acima pode demorar algum tempo para executar. Devido à utilização do método .add() a janela do PowerShell irá eco valores incrementados itens são adicionadas para cada matriz.

Agora que tem as suas subscrições divididas em três matrizes cuidadosamente deve rever os resultados. Poderá pretender efetuar uma cópia de segurança do conteúdo das matrizes de modo a que pode facilmente reverter as alterações devem terá no futuro. Se decidiu, pretendesse converter todas as subscrições elegíveis que estão atualmente sobre o modelo de preços antigo para o novo modelo de preços que esta tarefa agora pode ser conseguida com:

```PowerShell
Foreach ($id in $Eligible)
{
$ResourceID ="/subscriptions/$id/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action migratetonewpricingmodel `
 -Force
}

```