---
title: Ver registos de atividades do Azure para monitorizar os recursos | Documentos da Microsoft
description: Utilize os registos de atividades para ações de utilizador de revisão e erros. Mostra o Azure PowerShell Portal, da CLI do Azure e REST.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: fcdb3125-13ce-4c3b-9087-f514c5e41e73
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/08/2018
ms.author: tomfitz
ms.openlocfilehash: 09f7fba2b8ae3b3ccc8710ffe9302d02d311c74c
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/10/2018
ms.locfileid: "51514337"
---
# <a name="view-activity-logs-to-audit-actions-on-resources"></a>Ver registos de atividades para auditar as ações em recursos

Através dos registos de atividades, pode determinar:

* as operações que foram executadas nos recursos na sua subscrição
* quem iniciou a operação (embora operações iniciadas por um serviço de back-end não devolver um utilizador como o autor da chamada)
* Quando ocorreu a operação
* O estado da operação
* Os valores de outras propriedades que podem ajudar a operação de pesquisa

O registo de atividade contém todas as operações de escrita (PUT, POST, DELETE) efetuadas nos seus recursos. Não inclui operações de leitura (GET). Para obter uma lista de ações de recursos, consulte [operações de fornecedor de recursos do Azure Resource Manager](../role-based-access-control/resource-provider-operations.md). Pode utilizar os registos de auditoria para encontrar um erro ao resolver problemas ou para monitorizar a forma como um utilizador na sua organização alterou um recurso.

Os registos de atividades são mantidos durante 90 dias. Pode consultar qualquer intervalo de datas, desde que a data de início não seja superior a 90 dias no passado.

Pode recuperar informações de registos de atividade através do portal, PowerShell, CLI do Azure, API de REST de informações, ou [biblioteca de .NET de Insights](https://www.nuget.org/packages/Microsoft.Azure.Insights/).

## <a name="portal"></a>Portal

1. Para ver os registos de atividade através do portal, selecione **Monitor**.

    ![Selecione os registos de atividades](./media/resource-group-audit/select-monitor.png)

   Ou, para filtrar automaticamente o registo de atividades para um determinado recurso ou grupo de recursos, selecione **registo de atividades**. Tenha em atenção que o registo de atividades automaticamente é filtrado pelo recurso selecionado.

    ![Filtrar por recurso](./media/resource-group-audit/filtered-by-resource.png)
2. Na **registo de atividades**, verá um resumo das operações recentes.

    ![Mostrar ações](./media/resource-group-audit/audit-summary.png)
3. Para restringir o número de operações, apresentada, selecione as condições diferentes. Por exemplo, a imagem seguinte mostra os **Timespan** e **evento iniciado por** campos alterados para ver as ações executadas por um usuário específico ou aplicação para o mês passado. Selecione **aplicar** para ver os resultados da consulta.

    ![Opções de filtro de conjunto](./media/resource-group-audit/set-filter.png)

4. Se tiver de executar a consulta novamente mais tarde, selecione **guardar** e dê um nome de consulta.

    ![Guardar consulta](./media/resource-group-audit/save-query.png)
5. Para executar rapidamente uma consulta, pode selecionar uma das consultas incorporadas, tais como implementações falhadas.

    ![Selecione a consulta](./media/resource-group-audit/select-quick-query.png)

   A consulta selecionada define automaticamente os valores de filtro obrigatório.

    ![ver erros de implementação](./media/resource-group-audit/view-failed-deployment.png)

6. Selecione uma das operações para ver um resumo do evento.

    ![operação de vista](./media/resource-group-audit/view-operation.png)  

## <a name="powershell"></a>PowerShell

1. Para obter as entradas de registo, execute o **Get-AzureRmLog** comando. Fornecer parâmetros adicionais para filtrar a lista de entradas. Se não especificar uma hora de início e fim, são devolvidas entradas para a última hora. Por exemplo, para obter as operações de um grupo de recursos durante a última hora executam:

  ```azurepowershell-interactive
  Get-AzureRmLog -ResourceGroup ExampleGroup
  ```

    O exemplo seguinte mostra como utilizar o registo de atividade para operações de pesquisa executadas durante um período de tempo especificado. As datas de início e de fim são especificadas num formato de data.

  ```azurepowershell-interactive
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00
  ```

    Em alternativa, pode utilizar funções de data para especificar o intervalo de datas, tais como os últimos 14 dias.

  ```azurepowershell-interactive
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14)
  ```

2. Consoante a hora de início que especificar, os comandos anteriores podem devolver uma longa lista de operações para o grupo de recursos. Pode filtrar os resultados para o que está procurando, fornecendo os critérios de pesquisa. Por exemplo, se estiver a tentar pesquisar como uma aplicação web foi parada, pode executar o seguinte comando:

  ```azurepowershell-interactive
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14) | Where-Object OperationName -eq Microsoft.Web/sites/stop/action
  ```

    Que para este exemplo mostra que foi efetuada uma ação de paragem por someone@contoso.com.

  ```powershell
  Authorization     :
  Scope     : /subscriptions/xxxxx/resourcegroups/ExampleGroup/providers/Microsoft.Web/sites/ExampleSite
  Action    : Microsoft.Web/sites/stop/action
  Role      : Subscription Admin
  Condition :
  Caller            : someone@contoso.com
  CorrelationId     : 84beae59-92aa-4662-a6fc-b6fecc0ff8da
  EventSource       : Administrative
  EventTimestamp    : 8/28/2015 4:08:18 PM
  OperationName     : Microsoft.Web/sites/stop/action
  ResourceGroupName : ExampleGroup
  ResourceId        : /subscriptions/xxxxx/resourcegroups/ExampleGroup/providers/Microsoft.Web/sites/ExampleSite
  Status            : Succeeded
  SubscriptionId    : xxxxx
  SubStatus         : OK
  ```

3. Pode consultar as ações executadas por um utilizador em particular, mesmo para um grupo de recursos que já não existe.

  ```azurepowershell-interactive
  Get-AzureRmLog -ResourceGroup deletedgroup -StartTime (Get-Date).AddDays(-14) -Caller someone@contoso.com
  ```

4. Pode filtrar operações falhadas.

  ```azurepowershell-interactive
  Get-AzureRmLog -ResourceGroup ExampleGroup -Status Failed
  ```

5. Que possa dedicar a um erro ao observar a mensagem de estado para essa entrada.

  ```azurepowershell-interactive
  ((Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput).Properties[1].Content["statusMessage"] | ConvertFrom-Json).error
  ```

    Que retorna:

        code           message
        ----           -------
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP.

## <a name="azure-cli"></a>CLI do Azure

Para obter as entradas de registo, execute o [lista de registo de atividade do monitor de az](/cli/azure/monitor/activity-log#az-monitor-activity-log-list) comando.

  ```azurecli
  az monitor activity-log list --resource-group <group name>
  ```

## <a name="rest-api"></a>API REST

As operações REST para trabalhar com o registo de atividade fazem parte dos [API do REST Insights](https://msdn.microsoft.com/library/azure/dn931943.aspx). Para obter eventos de registo de atividade, veja [listar os eventos de gestão numa subscrição](https://msdn.microsoft.com/library/azure/dn931934.aspx).

## <a name="next-steps"></a>Passos Seguintes

* Registos de atividades do Azure podem ser utilizados com o Power BI para obter mais informações sobre as ações na sua subscrição. Ver [ver e analisar registos de atividades do Azure no Power BI e muito mais](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/).
* Para saber mais sobre como definir políticas de segurança, veja [controlo de acesso baseado em funções do Azure](../role-based-access-control/role-assignments-portal.md).
* Para saber mais sobre os comandos para ver as operações de implementação, consulte [ver as operações de implementação](resource-manager-deployment-operations.md).
* Para saber como impedir eliminações num recurso para todos os utilizadores, veja [bloquear recursos com o Azure Resource Manager](resource-group-lock-resources.md).
* Para ver a lista de operações disponíveis para cada fornecedor do Microsoft Azure Resource Manager, consulte [operações de fornecedor de recursos do Azure Resource Manager](../role-based-access-control/resource-provider-operations.md)
