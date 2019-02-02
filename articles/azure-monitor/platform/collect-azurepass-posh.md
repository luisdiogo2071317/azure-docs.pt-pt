---
title: Recolher métricas de recursos de PaaS do Azure com o Log Analytics | Documentos da Microsoft
description: Saiba como ativar a recolha de métricas de recursos de PaaS do Azure com o PowerShell para a retenção e análises do Log Analytics.
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
ms.date: 10/23/2018
ms.author: magoedte
ms.openlocfilehash: 08fdbc80c0f7dd1093020d64f4fc6f04fb7a88ae
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/02/2019
ms.locfileid: "55657047"
---
# <a name="configure-collection-of-azure-paas-resource-metrics-with-log-analytics"></a>Configurar a recolha de métricas de recursos de PaaS do Azure com o Log Analytics

Plataforma do Azure como um recursos de serviço (PaaS), como o SQL do Azure e o Web Sites (aplicações Web), pode emitir dados de métricas de desempenho nativamente para o Log Analytics. Este script permite aos utilizadores ativar métricas de registo de recursos de PaaS já implementados num grupo de recursos específico ou numa subscrição completa. 

Atualmente, não é possível ativar métricas de registo para PaaS recursos através do portal do Azure. Por conseguinte, tem de utilizar um script do PowerShell. Esta capacidade de registo de métricas nativo, juntamente com a monitorização do Log Analytics, permitem-lhe monitorizar recursos do Azure em escala. 

## <a name="prerequisites"></a>Pré-requisitos
Certifique-se de que tem os seguintes módulos do Azure Resource Manager instalados no seu computador antes de continuar:

- AzureRM.Insights
- AzureRM.OperationalInsights
- AzureRM.Resources
- AzureRM.profile

>[!NOTE]
>Recomendamos que todos os seus módulos do Azure Resource Manager são a mesma versão para garantir a compatibilidade, quando executa comandos do Azure Resource Manager no PowerShell.
>
Para instalar a versão mais recente dos módulos do Azure Resource Manager no seu computador, consulte [instalar e configurar o Azure PowerShell](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps?view=azurermps-4.4.1).  

## <a name="enable-azure-diagnostics"></a>Ativar o diagnóstico do Azure  
Configurar os diagnósticos do Azure para os recursos de PaaS é conseguido ao executar o script, **Enable-AzureRMDiagnostics.ps1**, que está disponível a partir do [galeria do PowerShell](https://www.powershellgallery.com/packages/Enable-AzureRMDiagnostics/2.52).  O script suporta os seguintes cenários:
  
* Especificar um recurso relacionado a um ou mais grupos de recursos numa subscrição  
* Especificar um recurso relacionado a um grupo de recursos específico numa subscrição  
* Reconfigurar um recurso para reencaminhar para uma área de trabalho diferente

São suportados os únicos recursos que suportam a coleta de métricas com os diagnósticos do Azure e enviam diretamente ao Log Analytics.  Para obter uma lista detalhada, consulte [Azure recolher registos do serviço e as métricas de utilização do Log Analytics](collect-azure-metrics-logs.md) 

Execute os seguintes passos para transferir e executar o script.

1.  No ecrã Iniciar do Windows, escreva **PowerShell** e com o botão direito do PowerShell resultados da pesquisa.  Selecione no menu **executar como administrador**.   
2. Guardar a **Enable-AzureRMDiagnostics.ps1** ficheiro de script localmente ao executar o seguinte comando e fornecendo um caminho para armazenar o script.    

    ```
    PS C:\> save-script -Name Enable-AzureRMDiagnostics -Path "C:\users\<username>\desktop\temp"
    ```

3. Execute `Connect-AzureRmAccount` para criar uma ligação com o Azure.   
4. Execute o seguinte script `.\Enable-AzureRmDiagnostics.ps1` sem quaisquer parâmetros para ativar a recolha de dados de um recurso específico na sua subscrição ou com o parâmetro `-ResourceGroup <myResourceGroup>` para especificar um recurso no grupo de recursos específico.   
5. Selecione a subscrição adequada na lista se tiver mais do que uma, ao introduzir o valor correto.<br><br> ![Selecione a subscrição devolvida pelo script](./media/collect-azurepass-posh/script-select-subscription.png)<br> Caso contrário, seleciona automaticamente a subscrição única disponível.
6. Em seguida, o script devolve uma lista de áreas de trabalho do Log Analytics registado na subscrição.  Selecione a adequada na lista.<br><br> ![Selecione a área de trabalho devolvida pelo script](./media/collect-azurepass-posh/script-select-workspace.png)<br> 
7. Selecione o recurso do Azure que gostaria de ativar a recolha de. Por exemplo, se digitar 5, ativa a recolha de dados para bases de dados do SQL Azure.<br><br> ![Tipo de recurso selecione devolvido pelo script](./media/collect-azurepass-posh/script-select-resource.png)<br>
   Pode selecionar apenas os recursos que suportam a coleta de métricas com o diagnóstico do Azure e o envio de diretamente para um Log Analytics.  O script irá mostrar um valor de **True** sob a **métricas** coluna para a lista de recursos que Deteta na sua subscrição ou grupo de recursos especificado.    
8. São-lhe pedido para confirmar a sua seleção.  Introduza **Y** ativar o registo de métricas para todos os recursos para o âmbito definido, o que, no nosso exemplo, são todas as bases de dados do SQL na subscrição selecionada.  

O script será executado em todos os recursos correspondentes aos critérios selecionados e ativar a recolha de métricas para os mesmos. Depois de terminar, verá uma mensagem a indicar a configuração está concluída.  

Logo após a conclusão, começará a ver os dados do recurso de PaaS do Azure no seu repositório do Log Analytics.  Um registo com o tipo `AzureMetrics` é criado e analisar estes registos são suportados pela [análise de SQL do Azure](../insights/azure-sql.md) e [análise de aplicações Web do Azure](../insights/azure-web-apps-analytics.md) soluções de gestão.   

## <a name="update-a-resource-to-send-data-to-another-workspace"></a>Atualizar um recurso para enviar dados para outra área de trabalho
Se tiver um recurso que já está a enviar dados para uma área de trabalho do Log Analytics e, posteriormente, decidir reconfigurá-lo para fazer referência a outra área de trabalho, pode executar o script com o `-Update` parâmetro.  

**Exemplo:** 
`PS C:\users\<username>\Desktop\temp> .\Enable-AzureRMDiagnostics.ps1 -Update`

Será solicitado para responder as mesmas informações que, quando executou o script para executar a configuração inicial.  

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [pesquisas de registos](../log-query/log-query-overview.md) para analisar os dados recolhidos a partir de origens de dados e soluções. 

* Uso [campos personalizados](custom-fields.md)(para analisar os registos de eventos em campos individuais.

* Revisão [criar e partilhar dashboards de dados do Log Analytics](../learn/tutorial-logs-dashboards.md) compreender como visualizar o seu registo de procura de formas significativas para a organização.
