---
title: Tutorial - monitorizar registos do Azure Firewall
description: Neste tutorial, vai aprender a ativar e gerir registos do Azure Firewall.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.workload: infrastructure-services
ms.date: 7/11/2018
ms.author: victorh
ms.openlocfilehash: a4922fda80b957138a9929090f9d3c349348185d
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38991966"
---
# <a name="tutorial-monitor-azure-firewall-logs"></a>Tutorial: monitorizar registos do Azure Firewall

[!INCLUDE [firewall-preview-notice](../../includes/firewall-preview-notice.md)]

Os exemplos nos artigos do Azure Firewall partem do princípio de que já ativou a pré-visualização pública do Azure Firewall. Para obter mais informações, veja [Ativar a pré-visualização pública do Azure Firewall](public-preview.md).

Pode monitorizar os registos do Azure Firewall com registos de firewall. Também pode utilizar os registos de atividades para auditar operações nos recursos do Azure Firewall.

Pode aceder a alguns destes registos através do portal. Os registos podem ser enviados para o [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md), o Armazenamento e os Hubs de Eventos, e analisados no Log Analytics ou através de ferramentas diferentes, como o Excel e o Power BI.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Ativar o registo através do portal do Azure
> * Ativar registo com o PowerShell
> * Ver e analisar o registo de atividades
> * Ver e analisar os registos de regras de rede e de aplicação

## <a name="diagnostic-logs"></a>Registos de diagnósticos

 Os seguintes registos de diagnóstico estão disponíveis para o Azure Firewall:

* **Registo de regra de Aplicação**

   O registo de regra de Aplicação é guardado numa conta de armazenamento, transmitido em fluxo aos Hubs de eventos e/ou enviado para o Log Analytics, apenas se o tiver ativado para cada Firewall do Azure. Cada nova ligação que corresponde a uma das suas regras de aplicação configuradas resulta num registo da ligação aceite/negada. Os dados são registados no formato JSON, conforme mostrado no exemplo seguinte:

   ```
   Category: access logs are either application or network rule logs.
   Time: log timestamp.
   Properties: currently contains the full message. 
   note: this field will be parsed to specific fields in the future, while maintaining backward compatibility with the existing properties field.
   ```

   ```json
   {
    "category": "AzureFirewallApplicationRule",
    "time": "2018-04-16T23:45:04.8295030Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallApplicationRuleLog",
    "properties": {
        "msg": "HTTPS request from 10.1.0.5:55640 to mydestination.com:443. Action: Allow. Rule Collection: collection1000. Rule: rule1002"
    }
   }
   ```

* **Registo de regra de Rede**

   O registo de regra de Rede é guardado numa conta de armazenamento, transmitido em fluxo aos Hubs de eventos e/ou enviado para o Log Analytics, apenas se o tiver ativado para cada Firewall do Azure. Cada nova ligação que corresponde a uma das suas regras de rede configuradas resulta num registo da ligação aceite/negada. Os dados são registados no formato JSON, conforme mostrado no exemplo seguinte:

   ```
   Category: access logs are either application or network rule logs.
   Time: log timestamp.
   Properties: currently contains the full message. 
   note: this field will be parsed to specific fields in the future, while maintaining backward compatibility with the existing properties field.
   ```

   ```json
  {
    "category": "AzureFirewallNetworkRule",
    "time": "2018-06-14T23:44:11.0590400Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallNetworkRuleLog",
    "properties": {
        "msg": "TCP request from 111.35.136.173:12518 to 13.78.143.217:2323. Action: Deny"
    }
   }

   ```

Tem três opções para armazenar os registos:

* **Conta de armazenamento**: as contas de armazenamento são ideais para os registos quando estes são armazenados durante um período mais longo e revistos quando necessário.
* **Hubs de eventos**: os Hubs de eventos são uma excelente opção para integrar com outras informações de segurança e ferramentas de gestão de eventos (SEIM) para obter alertas sobre os seus recursos.
* **Log Analytics**: o Log Analytics é ideal para monitorização geral em tempo real da sua aplicação ou para observar tendências.

## <a name="activity-logs"></a>Registos de atividade

   As entradas de registos de atividades são recolhidas por predefinição e pode visualizá-las no portal do Azure.

   Pode utilizar os [registos de atividades do Azure](../azure-resource-manager/resource-group-audit.md) (anteriormente conhecidos como registos operacionais e registos de auditoria) para ver todas as operações que são submetidas para a sua subscrição do Azure.

## <a name="enable-diagnostic-logging-through-the-azure-portal"></a>Ativar o registo de diagnósticos através do portal do Azure

Pode demorar alguns minutos até que os dados sejam apresentados nos seus registos, depois de concluir este procedimento para ativar o registo de diagnósticos. Se não vir nada inicialmente, verifique novamente passado alguns minutos.

1. No portal do Azure, abra o grupo de recursos da firewall e clique na firewall.
2. Em **Monitorização**, clique em **Registos de diagnóstico**.

   No Azure Firewall, estão disponíveis dois registos específicos do serviço:

   * Registo de regra de aplicação
   * Registo de regra de rede

3. Para iniciar a recolha de dados, clique em **Ativar os diagnósticos**.
4. A página **Definições de diagnóstico** fornece as definições para os registos de diagnóstico. 
5. Neste exemplo, o Log Analytics armazena os registos, por isso escreva **Análise do registo da firewall** como o nome.
6. Clique em **Enviar para o Log Analytics** para configurar a sua área de trabalho. Também pode utilizar os hubs de eventos e uma conta de armazenamento para guardar os registos de diagnóstico.
7. Em **Log Analytics**, clique em **Configurar**.
8. Na página Áreas de Trabalho do OMS, clique em **Criar Nova Área de Trabalho**.
9. Na página **Área de trabalho do Log Analytics**, escreva **firewall-oms** como o novo nome da **Área de trabalho do OMS**.
10. Selecione a sua subscrição, utilize o grupo de recursos de firewall existente (**Test-FW-RG**), selecione **E.U.A. Leste** como localização e selecione o escalão de preço **Gratuito**.
11. Clique em **OK**.
   ![Iniciar o processo de configuração][1]
12. Em **Registo**, clique em **AzureFirewallApplicationRule** e **AzureFirewallNetworkRule** para recolher registos para as regras de aplicação e de rede.
   ![Guardar as definições de diagnóstico][2]
13. Clique em **Guardar**.

## <a name="enable-logging-with-powershell"></a>Ativar registo com o PowerShell

O registo de atividades é ativado automaticamente para todos os recursos do Resource Manager. O registo de diagnósticos tem de estar ativado para iniciar a recolha dos dados disponíveis através desses registos.

Para ativar o registo de diagnósticos, utilize os seguintes passos:

1. Anote o ID de recurso da conta de armazenamento, onde os dados de registo são armazenados. Este valor é do formulário: */subscriptions/\<subscriptionId\>/resourceGroups/\<nome do grupo de recursos\>/providers/Microsoft.Storage/storageAccounts/\<nome da conta de armazenamento\>*.

   Pode utilizar qualquer conta de armazenamento na sua subscrição. Pode utilizar o portal do Azure para encontrar estas informações. As informações que estão localizadas na página do recurso **Propriedade**.

2. Anote o ID de recurso da Firewall para o qual o registo está ativado. Este valor é do formulário: */subscriptions/\<subscriptionId\>/resourceGroups/\<nome do grupo de recursos\>/providers/Microsoft.Network/azureFirewalls/\<Nome da firewall\>*.

   Pode utilizar o portal para encontrar estas informações.

3. Ative o registo de diagnósticos com o seguinte cmdlet do PowerShell:

    ```powershell
    Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/azureFirewalls/<Firewall name> `
   -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> `
   -Enabled $true     
    ```
    
> [!TIP] 
>Os registos de diagnóstico não necessitam de uma conta de armazenamento separada. A utilização do armazenamento para registo do acesso e do desempenho incorre em encargos de serviços.

## <a name="view-and-analyze-the-activity-log"></a>Ver e analisar o registo de atividades

Pode ver e analisar os dados de registo de atividades através de um dos seguintes métodos:

* **Ferramentas do Azure**: recuperar informações de registo de atividades através do Azure PowerShell, a CLI do Azure, a API REST do Azure ou o portal do Azure. As instruções passo-a-passo para cada método estão detalhadas no artigo [Operações de atividades com o Resource Manager](../azure-resource-manager/resource-group-audit.md).
* **Power BI**: se ainda não tiver uma conta do [Power BI](https://powerbi.microsoft.com/pricing), pode experimentá-lo gratuitamente. Ao utilizar o [pacote de conteúdos de Registos de Atividades do Azure para o Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/), pode analisar os seus dados com dashboards pré-configurados que podem ser utilizados tal como estão ou personalizados.

## <a name="view-and-analyze-the-network-and-application-rule-logs"></a>Ver e analisar os registos de regras de rede e de aplicação

O Azure [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) recolhe os ficheiros de registo de contadores e de eventos. Inclui visualizações e capacidades de pesquisa poderosas para analisar os seus registos.

Também pode ligar à sua conta de armazenamento e obter as entradas de registo JSON para os registos de acesso e desempenho. Depois de transferir os ficheiros JSON, pode convertê-los em CSV e visualizá-los no Excel, Power BI ou qualquer outra ferramenta de visualização de dados.

> [!TIP]
> Se estiver familiarizado com os conceitos básicos do Visual Studio para alterar os valores de constantes e variáveis em C#, pode utilizar as [ferramentas de conversor de registo](https://github.com/Azure-Samples/networking-dotnet-log-converter) disponíveis no GitHub.


## <a name="next-steps"></a>Passos seguintes

Agora que configurou a firewall para recolher registos, pode explorar o Log Analytics para ver os seus dados.

> [!div class="nextstepaction"]
> [Soluções de monitorização de rede no Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md)

[1]: ./media/tutorial-diagnostics/figure1.png
[2]: ./media/tutorial-diagnostics/figure2.png
