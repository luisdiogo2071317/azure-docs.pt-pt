---
title: Responder a eventos com Alertas do Log Analytics do Azure | Microsoft Docs
description: "Este tutorial ajuda-o a compreender os alertas no Log Analytics para identificar informações importantes no seu repositório do OMS e notificá-lo proativamente de problemas ou invocar ações para tentar corrigi-las."
services: log-analytics
documentationcenter: log-analytics
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/20/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: fcfaa849f67ffcfa69672d116837e96d318c2124
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="respond-to-events-with-log-analytics-alerts"></a>Responder a eventos com Alertas do Log Analytics
Os Alertas no Log Analytics identificam informações importantes no seu repositório do Log Analytics. Estes são criados pelas regras de alerta que executam automaticamente pesquisas de registos em intervalos regulares, e se os resultados da pesquisa de registos corresponderem a critérios de específicos, então é criado um registo de alerta que pode ser configurado para efetuar uma resposta automática.  Este tutorial é uma continuação do tutorial [Criar e partilhar dashboards de dados do Log Analytics](log-analytics-tutorial-dashboards.md).   

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma regra de alerta
> * Configurar uma regra de alerta para enviar uma notificação de e-mail

Para concluir o exemplo neste tutorial, tem de ter uma máquina virtual existente [ligada à área de trabalho do Log Analytics](log-analytics-quick-collect-azurevm.md).  

## <a name="log-in-to-azure-portal"></a>Iniciar sessão no portal do Azure
Inicie sessão no portal do Azure em [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-alerts"></a>Criar alertas

Os alertas são criados por regras de alerta que executam automaticamente pesquisas de registos em intervalos regulares.  Pode criar alertas com base em métricas de desempenho específicas ou quando determinados eventos são criados, ausência de um evento ou um número de eventos é criado numa janela de tempo específica.  Por exemplo, os alertas podem ser utilizados para o notificar quando a utilização média da CPU excede um determinado limiar ou é gerado um evento quando um serviço específico do Windows ou Linux daemon não está em execução.   Se os resultados da pesquisa de registos corresponderem a critérios específicos, então é criado um registo de alerta. A regra pode, em seguida, executar automaticamente uma ou mais ações para notificá-lo do alerta proativamente ou invocar outro processo. 

No exemplo seguinte, cria uma regra de alerta de medida da métrica que cria um alerta para cada objeto de computador na consulta com um valor que excede um limiar de 90%.

1. No portal do Azure, clique em **All services** (Todos os serviços). Na lista de recursos, escreva **Log Analytics**. À medida que começa a escrever, a lista filtra com base na sua entrada. Selecione **Log Analytics**.
2. Inicie o portal do OMS ao selecionar o Portal do OMS e na página **Descrição geral**, selecione **Pesquisa de Registos**.  
3. Selecione **Favoritos** na parte superior do portal e no painel **Pesquisas Guardadas** à direita, selecione a consulta *VMs do Azure - Utilização do Processador*.  
4. Clique em **Alerta** na parte superior da página para abrir o ecrã **Adicionar Regra de Alerta**.  
5. Configure a regra de alerta com as seguintes informações:  
   a. Indique um **Nome** para o alerta, como a *Utilização excedida do processador da VM >90*  
   b. Para a **Janela de Tempo**, especifique um intervalo de tempo para a consulta, como *30*.  A consulta devolve apenas os registos que foram criados neste intervalo da hora atual.  
   c. O **Alerta da Frequência** especifica agora a frequência com que a consulta deve ser executada.  Neste exemplo, especifique *5* minutos, que ocorre dentro da nossa janela de tempo especificada.  
   d. Selecione **Medida da Métrica** e introduza *90* para **Valor Agregado** e introduza *3* para **Acionar alerta baseado no**   
   e. Em **Ações**, desative a notificação de e-mail.
6. Clique em **Guardar** para concluir a regra de alerta. Começa imediatamente a executar.<br><br> ![Exemplo de regra de alerta](media/log-analytics-tutorial-response/log-analytics-alert-01.png)

Os registos de alerta criados por regras de alertas no Log Analytics têm um Tipo de **Alerta** e um SourceSystem de **OMS**.<br><br> ![Exemplo de Eventos de alertas gerados](media/log-analytics-tutorial-response/log-analytics-alert-events-01.png)  

## <a name="alert-actions"></a>Ações de alerta
Pode realizar ações avançadas com alertas, como criar uma notificação de e-mail, inicie um [Runbook de automatização](../automation/automation-runbook-types.md), utilize um webhook para criar um registo de incidente no seu sistema de gestão de incidentes ITSM ou com a [solução do Conector de Gestão de Serviços de TI](log-analytics-itsmc-overview.md) como resposta quando os critérios de alerta são cumpridos.   

As ações de e-mail enviam um e-mail com os detalhes do alerta para um ou mais destinatários. Pode especificar o assunto do correio, mas o respetivo conteúdo é um formato standard criado pelo Log Analytics.  Vamos atualizar a regra de alerta criada anteriormente e configurá-la para o notificar por e-mail, em vez de monitorizar ativamente para o registo de alerta com uma pesquisa de registo.     

1. No portal do OMS, no menu superior selecione **Definições** e, em seguida, selecione **Alertas**.
2. Na lista de regras de alertas, clique no ícone do lápis junto ao alerta criado anteriormente.
3. Na secção **Ações**, ative as notificações por e-mail.
4. Escreva um **Assunto** no e-mail, como *Limiar de utilização do processador excedido >90*.
5. Adicione endereços de um ou mais destinatários de e-mail no campo **Destinatários**.  Se especificar mais de um endereço, separe os endereços com um ponto e vírgula (;).
6. Clique em **Guardar** para concluir a regra de alerta. Começa imediatamente a executar.<br><br> ![Regra de alerta com notificação por e-mail](media/log-analytics-tutorial-response/log-analytics-alert-02.png)

## <a name="next-steps"></a>Passos seguintes
Neste tutorial, aprendeu como as regras de alertas podem identificar e responder proativamente a um problema quando executam pesquisas de registo em intervalos agendados e corresponder a critérios específicos.  

Siga esta ligação para ver os exemplos de scripts do Log Analytics pré-criados.  

> [!div class="nextstepaction"]
> [Exemplos do script do Log Analytics](powershell-samples.md)