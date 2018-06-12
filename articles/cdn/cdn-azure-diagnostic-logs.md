---
title: Os registos de diagnóstico do Azure | Microsoft Docs
description: Cliente pode ativar a análise de registos para a CDN do Azure.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2018
ms.author: v-deasim
ms.openlocfilehash: 98a7fc5c4607115811e17a7cf6acd4e867663833
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35261309"
---
# <a name="azure-diagnostic-logs"></a>Registos de diagnóstico do Azure

Com os registos de diagnóstico do Azure, pode ver a análise de núcleo e guardá-las para um ou mais destinos, incluindo:

 - Conta de armazenamento do Azure
 - Azure Event Hubs
 - [Área de trabalho de análise de registo](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started)
 
Esta funcionalidade está disponível nos pontos finais da CDN para todos os escalões de preços. 

Registos de diagnóstico do Azure permitem-lhe exportar métricas de utilização básica do ponto final de CDN para uma variedade de origens de modo a que pode aceder aos mesmos de forma personalizada. Por exemplo, pode efetuar os seguintes tipos de exportação de dados:

- Exporte dados para armazenamento de BLOBs, exportar para CSV e gerar gráficos no Excel.
- Exportar dados para Event Hubs e estar relacionados com a dados a partir de outros serviços do Azure.
- Exportar dados para análise de registos e ver dados no seu próprio espaço de trabalho de análise de registos

O diagrama seguinte mostra uma vista de análise de núcleo CDN típica de dados.

![Portal – registos de diagnóstico](./media/cdn-diagnostics-log/01_OMS-workspace.png)

*Figura 1 - vista de análise de núcleo CDN*

Para mais informações sobre os registos de diagnóstico, consulte [os registos de diagnóstico](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).

## <a name="enable-logging-with-the-azure-portal"></a>Ativar o registo com o portal do Azure

Siga estes ativar passos registo com a análise de núcleo CDN:

Inicie sessão no [portal do Azure](http://portal.azure.com). Se não já tiver ativado a CDN para o fluxo de trabalho, [criar um perfil de CDN do Azure e o ponto final](cdn-create-new-endpoint.md) antes de continuar.

1. No portal do Azure, navegue para **perfil da CDN**.

2. No portal do Azure, procure um perfil da CDN ou selecione um dashboard. Em seguida, selecione o ponto final da CDN para a qual pretende ativar registos de diagnóstico.

    ![Portal – registos de diagnóstico](./media/cdn-diagnostics-log/02_Browse-to-Diagnostics-logs.png)

3. Selecione **registos de diagnóstico** na secção monitorização.

   O **registos de diagnóstico** é apresentada a página.

    ![Portal – registos de diagnóstico](./media/cdn-diagnostics-log/03_Diagnostics-logs-options.png)

### <a name="enable-logging-with-azure-storage"></a>Ativar o registo com o Storage do Azure

Para utilizar uma conta do storage para armazenar os registos, siga estes passos:
    
1. Para **nome**, introduza um nome para as suas definições de registo de diagnóstico.
 
2. Selecione **arquivo para uma conta de armazenamento**, em seguida, selecione **CoreAnalytics**. 

2. Para **retenção (dias)**, escolha o número de dias de retenção. Uma retenção de zero dias armazena os registos indefinidamente. 

    ![Portal – registos de diagnóstico](./media/cdn-diagnostics-log/04_Diagnostics-logs-storage.png) 

3. Selecione **conta de armazenamento**.

    O **Selecione uma conta de armazenamento** é apresentada a página.

4. Selecione uma conta de armazenamento na lista pendente, em seguida, selecione **OK**.

    ![Portal – registos de diagnóstico](./media/cdn-diagnostics-log/cdn-select-storage-account.png)

5. Depois de terminar de efetuar as definições de registo de diagnóstico, selecione **guardar**.

### <a name="logging-with-log-analytics"></a>Registo de análise do registo

Para utilizar a análise de registos para armazenar os registos, siga estes passos:

1. Do **registos de diagnóstico** página, selecione **enviar ao Log Analytics**. 

    ![Portal – registos de diagnóstico](./media/cdn-diagnostics-log/05_Ready-to-Configure.png)    

2. Selecione **configurar** para configurar o registo de análise de registos. 

   O **áreas de trabalho do OMS** é apresentada a página.

    ![Portal – registos de diagnóstico](./media/cdn-diagnostics-log/06_Choose-workspace.png)

3. Selecione **criar nova área de trabalho**.

    O **área de trabalho OMS** é apresentada a página.

    ![Portal – registos de diagnóstico](./media/cdn-diagnostics-log/07_Create-new.png)

4. Para **área de trabalho OMS**, introduza um nome de área de trabalho do OMS. O nome de área de trabalho do OMS tem de ser exclusivos e conter apenas letras, números e hífenes; não são permitidos espaços e carateres de sublinhado. 

5. Para **subscrição**, selecione uma subscrição existente na lista pendente. 

6. Para **grupo de recursos**, crie um novo grupo de recursos ou selecione um existente.

7. Para **localização**, selecione uma localização da lista.

8. Selecione **afixar ao dashboard** se pretender guardar a configuração de registo ao dashboard. 

9. Selecione **OK** para concluir a configuração.

    ![Portal – registos de diagnóstico](./media/cdn-diagnostics-log/08_Workspace-resource.png)

10. Depois de criar a sua área de trabalho, está a devolvido para o **registos de diagnóstico** página. Confirme o nome da sua área de trabalho de análise de registos nova.

    ![Portal – registos de diagnóstico](./media/cdn-diagnostics-log/09_Return-to-logging.png)

11. Selecione **CoreAnalytics**, em seguida, selecione **guardar**.

12. Para ver a nova área de trabalho de análise de registos, selecione **principal análise** da sua página de ponto final da CDN.

    ![Portal – registos de diagnóstico](./media/cdn-diagnostics-log/11_OMS-dashboard.png) 

    A área de trabalho de análise de registos está agora pronta para iniciar a sessão de dados. Para consumir dados, tem de utilizar um [solução de análise do registo](#consuming-diagnostics-logs-from-a-log-analytics-workspace), abrangidas neste artigo.

Para obter mais informações sobre os atrasos de dados de registo, consulte [atrasos de dados de registo](#log-data-delays).

## <a name="enable-logging-with-powershell"></a>Ativar o registo com o PowerShell

O exemplo seguinte mostra como ativar os registos de diagnóstico através de Cmdlets do PowerShell do Azure.

### <a name="enabling-diagnostic-logs-in-a-storage-account"></a>Ativar o diagnóstico inicia sessão numa conta do storage

1. Iniciar sessão e selecionar uma subscrição:

    Ligar-AzureRmAccount 

    SELECT-AzureSubscription - SubscriptionId 

2. Para ativar os registos de diagnóstico numa conta de armazenamento, introduza este comando:

    ```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Cdn/profiles/{profileName}/endpoints/{endpointName}" -StorageAccountId "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicStorage/storageAccounts/{storageAccountName}" -Enabled $true -Categories CoreAnalytics
    ```

3. Para ativar registos de diagnóstico numa área de trabalho de análise de registos, introduza este comando:

    ```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/`{subscriptionId}<subscriptionId>
    .<subscriptionName>" -WorkspaceId "/subscriptions/<workspaceId>.<workspaceName>" -Enabled $true -Categories CoreAnalytics 
    ```

## <a name="consuming-diagnostics-logs-from-azure-storage"></a>Consumir os registos de diagnóstico do armazenamento do Azure
Esta secção descreve o esquema da análise de núcleo CDN, como está organizada dentro de uma conta de armazenamento do Azure e fornece o código de exemplo para transferir os registos de um ficheiro CSV.

### <a name="using-microsoft-azure-storage-explorer"></a>Utilizar o Explorador de armazenamento do Microsoft Azure
Antes de poder aceder os dados de análise de núcleo de uma conta de armazenamento do Azure, primeiro precisa de uma ferramenta para aceder ao conteúdo numa conta do storage. Apesar de existirem várias ferramentas disponíveis no mercado, que recomendamos é o Explorador de armazenamento do Microsoft Azure. Para transferir a ferramenta, consulte [Explorador de armazenamento do Azure](http://storageexplorer.com/). Depois de transferir e instalar o software, configure-a para utilizar a mesma conta de armazenamento do Azure que foi configurada como um destino para os registos de diagnóstico da CDN.

1.  Abra **Explorador de armazenamento do Microsoft Azure**
2.  Localizar a conta de armazenamento
3.  Expanda o **contentores de BLOBs** nó sob esta conta de armazenamento.
4.  Selecione o contentor com o nome *insights-registos-coreanalytics*.
5.  Resulta Mostrar cópias de segurança no painel da direita, começando pelo primeiro nível, como *resourceId =*. Continuar a selecionar cada nível até encontrar o ficheiro *PT1H.json*. Consulte o seguinte *o formato de caminho do Blob* nota para obter uma explicação sobre o caminho.
6.  Cada blob *PT1H.json* ficheiro representa os registos de análise para uma hora para um ponto final de CDN específico ou o domínio personalizado.
7.  O esquema do conteúdo deste ficheiro JSON está descrito no esquema secção dos registos de análise do núcleo.


> [!NOTE]
> **Formato de caminho do blob**
> 
> Registos de análise do Core são gerados a cada hora e os dados são recolhidos e armazenados dentro de um único Azure blob como um payload JSON. Porque a ferramenta Explorador de armazenamento interpreta '/' como um separador de diretório e mostra a hierarquia, o caminho para o blob do Azure é apresentado como se houver uma estrutura hierárquica e representa o nome do blob. O nome do blob segue a seguinte convenção de nomenclatura: 
    
    resourceId=/SUBSCRIPTIONS/{Subscription Id}/RESOURCEGROUPS/{Resource Group Name}/PROVIDERS/MICROSOFT.CDN/PROFILES/{Profile Name}/ENDPOINTS/{Endpoint Name}/ y={Year}/m={Month}/d={Day}/h={Hour}/m={Minutes}/PT1H.json

**Descrição de campos:**

|Valor|Descrição|
|-------|---------|
|ID da subscrição    |ID de subscrição do Azure no formato Guid.|
|Nome do Grupo de Recursos |Nome do grupo de recursos aos quais pertencem os recursos da CDN.|
|Nome do perfil |Nome do perfil CDN|
|Nome do ponto final |Nome do ponto final de CDN|
|Ano|  Representação de quatro dígitos do ano, por exemplo, 2017|
|Mês| Representação de dois dígitos do número de meses. 01 = Janeiro... 12 = Dezembro|
|Dia|   Representação de dois dígitos do dia do mês|
|PT1H.json| Armazenar os dados de análise real do ficheiro JSON|

### <a name="exporting-the-core-analytics-data-to-a-csv-file"></a>Exportar os dados de análise de núcleo para um ficheiro CSV

Para facilitar a análise de núcleo de acesso, código de exemplo para uma ferramenta é fornecido. Esta ferramenta permite transferir os ficheiros JSON para um formato de ficheiro flat separados por vírgulas, que pode ser utilizado para criar gráficos ou outras agregações.

Eis como pode utilizar a ferramenta:

1.  Visite a ligação do github: [https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv ](https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv)
2.  Transferir o código.
3.  Siga as instruções para compilar e configurar.
4.  Execute a ferramenta.
5.  O ficheiro CSV resultante mostra os dados de análise numa hierarquia simples simple.

## <a name="consuming-diagnostics-logs-from-a-log-analytics-workspace"></a>Consumir os registos de diagnóstico de uma área de trabalho de análise de registos
Análise de registos é um serviço Azure que monitoriza a sua nuvem e no local ambientes para manter a respetiva disponibilidade e desempenho. Recolhe dados gerados por recursos nos seus ambientes na cloud e no local e de outras ferramentas de monitorização, para disponibilizar análises relativas a várias origens. 

Para utilizar a análise de registos, terá [ativar o registo](#enable-logging-with-azure-storage) à área de trabalho do Log Analytics do Azure, que é abordado anteriormente neste artigo.

### <a name="using-the-log-analytics-workspace"></a>Utilizar a área de trabalho de análise de registos

 O diagrama seguinte mostra a arquitetura de entradas e saídas do repositório:

![Área de trabalho do Log Analytics](./media/cdn-diagnostics-log/12_Repo-overview.png)

*Figura 3 - repositório de análise do registo*

Pode visualizar os dados de diversas formas utilizando soluções de gestão. Pode obter soluções de gestão do [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions).

Pode instalar as soluções de gestão do Azure marketplace, selecionando o **obtê-lo agora** ligação na parte inferior de cada solução.

### <a name="add-a-log-analytics-cdn-management-solution"></a>Adicionar uma solução de gestão de CDN de análise do registo

Siga estes passos para adicionar uma solução de gestão de análise do registo:

1.   Inicie sessão no portal do Azure com a sua subscrição do Azure e aceda ao dashboard.
    ![Dashboard do Azure](./media/cdn-diagnostics-log/13_Azure-dashboard.png)

2. No **novo** página **Marketplace**, selecione **monitorização + gestão**.

    ![Marketplace](./media/cdn-diagnostics-log/14_Marketplace.png)

3. No **monitorização + gestão** página, selecione **ver todos os**.

    ![Ver tudo](./media/cdn-diagnostics-log/15_See-all.png)

4. Procure CDN na caixa de pesquisa.

    ![Ver tudo](./media/cdn-diagnostics-log/16_Search-for.png)

5. Selecione **análise de núcleo da CDN do Azure**. 

    ![Ver tudo](./media/cdn-diagnostics-log/17_Core-analytics.png)

6. Depois de selecionar **criar**, é-lhe pedido para criar uma área de trabalho de análise de registos nova ou utilize uma já existente. 

    ![Ver tudo](./media/cdn-diagnostics-log/18_Adding-solution.png)

7. Selecione a área de trabalho criado antes de. Em seguida, terá de adicionar uma conta de automatização.

    ![Ver tudo](./media/cdn-diagnostics-log/19_Add-automation.png)

8. O ecrã seguinte mostra a forma de conta de automatização, terá de preencher enviados. 

    ![Ver tudo](./media/cdn-diagnostics-log/20_Automation.png)

9. Assim que tiver criado a conta de automatização, está pronto para adicionar a sua solução. Selecione o botão **Criar**.

    ![Ver tudo](./media/cdn-diagnostics-log/21_Ready.png)

10. A solução foi adicionada à sua área de trabalho. Regressar ao dashboard do portal do Azure.

    ![Ver tudo](./media/cdn-diagnostics-log/22_Dashboard.png)

    Selecione a área de trabalho de análise de registos que criou para aceder à sua área de trabalho. 

11. Selecione o **Portal do OMS** mosaico para obter a sua solução de novo.

    ![Ver tudo](./media/cdn-diagnostics-log/23_workspace.png)

12. O portal deverá agora parecer-semelhante do ecrã seguinte:

    ![Ver tudo](./media/cdn-diagnostics-log/24_OMS-solution.png)

    Selecione um dos mosaicos para ver várias vistas sobre os seus dados.

    ![Ver tudo](./media/cdn-diagnostics-log/25_Interior-view.png)

    Pode se deslocar para a esquerda ou direita para ver mais mosaicos que representa vistas individuais para os dados. 

    Selecione um dos mosaicos para ver mais detalhes sobre os dados.

     ![Ver tudo](./media/cdn-diagnostics-log/26_Further-detail.png)

### <a name="offers-and-pricing-tiers"></a>Ofertas e escalões de preços

Pode ver ofertas e escalões de preços para soluções de gestão [aqui](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers).

### <a name="customizing-views"></a>Personalizar vistas

Pode personalizar a vista sobre os seus dados através da utilização de **estruturador de vistas**. Para começar a estruturar, aceda à sua área de trabalho de análise de registos e selecione o **estruturador de vistas** mosaico.

![Estruturador de Vista](./media/cdn-diagnostics-log/27_Designer.png)

Arrastar e largar os tipos de gráficos e preencha os dados detalhes pretende analisar.

![Estruturador de Vista](./media/cdn-diagnostics-log/28_Designer.png)

    
## <a name="log-data-delays"></a>Atrasos de dados de registo

A tabela seguinte mostra registo atrasos de dados para **CDN do Azure Standard da Microsoft**, **CDN do Azure Standard da Akamai**, e **CDN do Azure Standard/Premium da Verizon**.

Atrasos de dados de registo do Microsoft | Atrasos de dados de registo da Verizon | Atrasos de dados de registo Akamai
--- | --- | ---
Atrasada pelo 1 hora. | Um atraso por 1 hora e pode demorar até 2 horas para iniciar a apresentação após a conclusão da propagação de ponto final. | Um atraso por 24 horas; Se foi criada há mais de 24 horas, que demora até 2 horas para iniciar a apresentação. Se tiver sido recentemente criado, pode demorar até 25 horas para os registos para iniciar a apresentação.

## <a name="diagnostic-log-types-for-cdn-core-analytics"></a>Tipos de registo de diagnóstico para análise de núcleo CDN

A Microsoft oferece atualmente core registos de análise apenas, que contêm as métricas que mostra as estatísticas de resposta HTTP e estatísticas de saída visto da CDN POPs/margens.

### <a name="core-analytics-metrics-details"></a>Detalhes de métricas de análise de núcleo
A tabela seguinte mostra uma lista das métricas disponíveis nas principais registos de análise para **CDN do Azure Standard da Microsoft**, **CDN do Azure Standard da Akamai**, e **Azure CDN padrão/Premium da Verizon**. Nem todas as métricas estão disponíveis de todos os fornecedores, apesar destas diferenças são mínimas. A tabela apresenta também se uma métrica fornecida está disponível a partir de um fornecedor. As métricas estão disponíveis para apenas esses pontos finais da CDN que tenham tráfego nos mesmos.


|Métrica                     | Descrição | Microsoft | Verizon | Akamai |
|---------------------------|-------------|-----------|---------|--------|
| RequestCountTotal         | Número total de pedidos de pedido durante este período. | Sim | Sim |Sim |
| RequestCountHttpStatus2xx | Contagem de todos os pedidos que resultaram num código de HTTP 2xx (por exemplo, 200, 202). | Sim | Sim |Sim |
| RequestCountHttpStatus3xx | Contagem de todos os pedidos que resultaram num código de HTTP 3xx (por exemplo, 300, 302). | Sim | Sim |Sim |
| RequestCountHttpStatus4xx | Contagem de todos os pedidos que resultaram num código de HTTP 4xx (por exemplo, 400, 404). | Sim | Sim |Sim |
| RequestCountHttpStatus5xx | Contagem de todos os pedidos que resultaram num código de HTTP 5xx (por exemplo, 500, 504). | Sim | Sim |Sim |
| RequestCountHttpStatusOthers | Contagem de todos os outros códigos HTTP (fora 2xx-5xx). | Sim | Sim |Sim |
| RequestCountHttpStatus200 | Contagem de todos os pedidos que resultaram numa resposta de código HTTP 200. | Sim | Não  |Sim |
| RequestCountHttpStatus206 | Contagem de todos os pedidos que resultaram numa resposta código 206 HTTP. | Sim | Não  |Sim |
| RequestCountHttpStatus302 | Contagem de todos os pedidos que resultaram numa resposta de código HTTP 302. | Sim | Não  |Sim |
| RequestCountHttpStatus304 | Contagem de todos os pedidos que resultaram numa resposta código 304 HTTP. | Sim | Não  |Sim |
| RequestCountHttpStatus404 | Contagem de todos os pedidos que resultaram numa resposta de código HTTP 404. | Sim | Não  |Sim |
| RequestCountCacheHit | Atingiu o número de todos os pedidos que resultaram numa Cache. O recurso tiver sido servido diretamente a partir do POP ao cliente. | Sim | Sim | Não  |
| RequestCountCacheMiss | Contagem de todos os pedidos que resultaram numa falha de acerto na Cache. Uma falha de acerto na Cache significa que o elemento não foi encontrado no POP mais próximo para o cliente e, por conseguinte, foi obtido da origem. | Sim | Sim | Não |
| RequestCountCacheNoCache | Contagem de todos os pedidos para um recurso que impedido de ser colocadas em cache devido a uma configuração de utilizador na extremidade. | Sim | Sim | Não |
| RequestCountCacheUncacheable | Contagem de todos os pedidos ativos que são impedidos de ser colocadas em cache, o elemento Cache-Control e cabeçalhos de expira, que indicam que este deve não ser colocadas em cache num POP ou pelo cliente HTTP. | Sim | Sim | Não |
| RequestCountCacheOthers | Contagem de todos os pedidos com o estado de cache não abrangido por acima. | Não | Sim | Não  |
| EgressTotal | Transferência de dados de saída em GB | Sim |Sim |Sim |
| EgressHttpStatus2xx | Dados de saída transferência * para respostas com códigos de estado HTTP 2xx em GB. | Sim | Sim | Não  |
| EgressHttpStatus3xx | Transferem de dados de saída para as respostas com códigos de estado HTTP 3xx em GB. | Sim | Sim | Não  |
| EgressHttpStatus4xx | Transferem de dados de saída para as respostas com códigos de estado HTTP 4xx em GB. | Sim | Sim | Não  |
| EgressHttpStatus5xx | Transferem de dados de saída para as respostas com códigos de estado HTTP 5xx em GB. | Sim | Sim | Não |
| EgressHttpStatusOthers | Transferem de dados de saída para as respostas com outros códigos de estado HTTP em GB. | Sim | Sim | Não  |
| EgressCacheHit | Saída transferência de dados para as respostas que foram fornecidas diretamente a partir da cache do CDN na CDN POPs/margens. | Sim | Sim | Não |
| EgressCacheMiss. | Transferem de dados de saída para as respostas que não foram encontradas no servidor mais próximo POP e obtidas a partir do servidor de origem. | Sim | Sim | Não |
| EgressCacheNoCache | Transferem de dados de saída para recursos que são impedidos de ser colocadas em cache devido a uma configuração de utilizador na extremidade. | Sim | Sim | Não |
| EgressCacheUncacheable | Transferem de dados de saída para recursos que são impedidos de ser colocadas em cache por Cache-Control e/ou cabeçalhos expira o elemento. Indica que este deve não ser colocadas em cache num POP ou pelo cliente HTTP. | Sim | Sim | Não |
| EgressCacheOthers | Transferências de dados de saída para obter outros cenários de cache. | Não | Sim | Não |

* Transferência de dados saída refere-se ao tráfego entregue dos servidores POP do CDN ao cliente.


### <a name="schema-of-the-core-analytics-logs"></a>Esquema dos registos de análise de núcleo 

Todos os registos são armazenados no formato JSON e cada entrada tem campos de cadeia, de acordo com o esquema seguinte:

```json
    "records": [
        {
            "time": "2017-04-27T01:00:00",
            "resourceId": "<ARM Resource Id of the CDN Endpoint>",
            "operationName": "Microsoft.Cdn/profiles/endpoints/contentDelivery",
            "category": "CoreAnalytics",
            "properties": {
                "DomainName": "<Name of the domain for which the statistics is reported>",
                "RequestCountTotal": integer value,
                "RequestCountHttpStatus2xx": integer value,
                "RequestCountHttpStatus3xx": integer value,
                "RequestCountHttpStatus4xx": integer value,
                "RequestCountHttpStatus5xx": integer value,
                "RequestCountHttpStatusOthers": integer value,
                "RequestCountHttpStatus200": integer value,
                "RequestCountHttpStatus206": integer value,
                "RequestCountHttpStatus302": integer value,
                "RequestCountHttpStatus304": integer value,
                "RequestCountHttpStatus404": integer value,
                "RequestCountCacheHit": integer value,
                "RequestCountCacheMiss": integer value,
                "RequestCountCacheNoCache": integer value,
                "RequestCountCacheUncacheable": integer value,
                "RequestCountCacheOthers": integer value,
                "EgressTotal": double value,
                "EgressHttpStatus2xx": double value,
                "EgressHttpStatus3xx": double value,
                "EgressHttpStatus4xx": double value,
                "EgressHttpStatus5xx": double value,
                "EgressHttpStatusOthers": double value,
                "EgressCacheHit": double value,
                "EgressCacheMiss": double value,
                "EgressCacheNoCache": double value,
                "EgressCacheUncacheable": double value,
                "EgressCacheOthers": double value,
            }
        }

    ]
}
```

Onde *tempo* representa a hora de início do limite de hora para o qual as estatísticas é comunicada. Quando uma métrica não é suportada por um fornecedor CDN, em vez de um valor de duplo ou número inteiro, há um valor nulo. Este valor nulo indica a ausência de uma métrica e é diferente do que um valor de 0. Não há um conjunto destas métricas por domínio configurado no ponto final.

Propriedades de exemplo:

```json
{
     "DomainName": "manlingakamaitest2.azureedge.net",
     "RequestCountTotal": 480,
     "RequestCountHttpStatus2xx": 480,
     "RequestCountHttpStatus3xx": 0,
     "RequestCountHttpStatus4xx": 0,
     "RequestCountHttpStatus5xx": 0,
     "RequestCountHttpStatusOthers": 0,
     "RequestCountHttpStatus200": 480,
     "RequestCountHttpStatus206": 0,
     "RequestCountHttpStatus302": 0,
     "RequestCountHttpStatus304": 0,
     "RequestCountHttpStatus404": 0,
     "RequestCountCacheHit": null,
     "RequestCountCacheMiss": null,
     "RequestCountCacheNoCache": null,
     "RequestCountCacheUncacheable": null,
     "RequestCountCacheOthers": null,
     "EgressTotal": 0.09,
     "EgressHttpStatus2xx": null,
     "EgressHttpStatus3xx": null,
     "EgressHttpStatus4xx": null,
     "EgressHttpStatus5xx": null,
     "EgressHttpStatusOthers": null,
     "EgressCacheHit": null,
     "EgressCacheMiss": null,
     "EgressCacheNoCache": null,
     "EgressCacheUncacheable": null,
     "EgressCacheOthers": null
}

```

## <a name="additional-resources"></a>Recursos adicionais

* [Registos de diagnóstico do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)
* [Análise de núcleo através do portal suplementar CDN do Azure](https://docs.microsoft.com/azure/cdn/cdn-analyze-usage-patterns)
* [Log Analytics do Azure](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)
* [Análise de registos do Azure REST API](https://docs.microsoft.com/rest/api/loganalytics)







