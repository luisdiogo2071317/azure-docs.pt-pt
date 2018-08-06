---
title: Responder a eventos com Alertas do Log Analytics do Azure | Microsoft Docs
description: Este tutorial ajuda-o a compreender os alertas com o Log Analytics para identificar informações importantes na sua área de trabalho e notificá-lo proativamente de problemas ou invocar ações para tentar corrigi-las.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/30/2018
ms.author: magoedte
ms.custom: mvc
ms.component: na
ms.openlocfilehash: d81a41a0012d4e0be4e812d48074e7af1e92213a
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/01/2018
ms.locfileid: "39391151"
---
# <a name="respond-to-events-with-azure-monitor-alerts"></a>Responder a eventos com Alertas do Azure Monitor
São criadas regras de pesquisa de registos pelos Alertas do Azure para executar automaticamente consultas de registos especificados a intervalos regulares.  Se os resultados da pesquisa de registos corresponderem a critérios específicos, é criado um registo de alerta. A regra pode executar automaticamente uma ou mais ações através dos [Grupos de Ações](../monitoring-and-diagnostics/monitoring-action-groups.md).  Este tutorial é uma continuação do tutorial [Criar e partilhar dashboards de dados do Log Analytics](log-analytics-tutorial-dashboards.md).   

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma regra de alerta
> * Configurar um Grupo de Ações para enviar uma notificação de e-mail

Para concluir o exemplo neste tutorial, tem de ter uma máquina virtual existente [ligada à área de trabalho do Log Analytics](log-analytics-quick-collect-azurevm.md).

## <a name="sign-in-to-azure-portal"></a>Iniciar sessão no portal do Azure
Inicie sessão no portal do Azure em [https://portal.azure.com](https://portal.azure.com).

## <a name="create-alerts"></a>Criar alertas
Os alertas são criados por regras de alerta no Azure Monitor e podem executar automaticamente consultas guardadas ou pesquisa de registos personalizadas em intervalos regulares.  Pode criar alertas com base em métricas de desempenho específicas ou quando determinados eventos são criados, ausência de um evento ou um número de eventos é criado numa janela de tempo específica.  Por exemplo, os alertas podem ser utilizados para notificar quando a utilização da CPU excede determinado limiar, quando é detetada uma atualização em falta ou quando é gerado um evento depois da deteção que determinado serviço Windows ou Linux daemon não está a ser executado.  Se os resultados da pesquisa de registos corresponderem a critérios específicos, será criado um alerta. A regra pode, em seguida, executar automaticamente uma ou mais ações, como notificá-lo do alerta proativamente ou invocar outro processo.

No exemplo seguinte, o utilizador cria uma regra de alerta de medição métrica com base na consulta *Azure VMs – Utilização do Processador* guardada em [Visualizar tutorial de dados](log-analytics-tutorial-dashboards.md). É criado um alerta para cada máquina virtual que excede um limiar de 90%.

1. No portal do Azure, clique em **All services** (Todos os serviços). Na lista de recursos, escreva **Monitorizar**. À medida que começa a escrever, a lista filtra com base na sua entrada. Selecione **Monitorizar**.
2. No painel da esquerda, selecione **Alertas** e, em seguida, clique em **Nova Regra de Alerta** na parte superior da página para criar um novo alerta.

    ![Criar nova regra de alerta](./media/log-analytics-tutorial-response/alert-rule-02.png)

3. Para o primeiro passo, na secção **Criar Alerta**, vai selecionar a sua área de trabalho do Log Analytics como o recurso, pois é um sinal de alerta baseado em registo.  Filtre os resultados ao escolher a **Subscrição** específica na lista pendente, se tiver mais do que uma, que contém a área de trabalho VM e Log Analytics criada anteriormente.  Filtre o **Tipo de Recurso** ao selecionar **Log Analytics** na lista pendente.  Por fim, selecione o **Recurso** **DefaultLAWorkspace** e, em seguida, clique em **Concluído**.

    ![Tarefa Criar alerta passo 1](./media/log-analytics-tutorial-response/alert-rule-03.png)

4. Na secção **Critérios de Alerta**, clique em **Adicionar Critérios** para definir a consulta e, em seguida, especifique a lógica que a regra de alerta segue. No painel **Configurar lógica de sinal**, selecione **Pesquisa de registos personalizados** como nome do sinal e introduza a sua consulta na **Consulta de pesquisa**.

    Por exemplo:
    ```
    Perf
    | where CounterName == "% Processor Time" and ObjectName == "Processor" and InstanceName == "_Total"
    | summarize AggregatedValue=avg(CounterValue) by bin(TimeGenerated, 1m)
    ```

    O painel atualiza para apresentar as definições de configuração do alerta.  Na parte superior, são apresentados os resultados dos últimos 30 minutos do sinal selecionado.

5. Configure o alerta com as seguintes informações:  
   a. Na lista pendente **Com base em*, selecione **Medição métrica**.  Uma medição métrica cria um alerta para cada objeto na consulta com um valor que excede o nosso limiar especificado.  
   b. Para **Condição**, selecione **Maior que** e introduza **90** para **Limiar**.  
   c. Na secção Acionar Alerta Com Base Em, selecione **Falhas de segurança consecutivas** e na lista pendente selecione **Maior que** e introduza 3.  
   d. Na secção Avaliação baseada em, aceite as predefinições. A regra é executada a cada cinco minutos e devolve registos que foram criados no intervalo da hora atual.  
6. Clique em **Concluído** para concluir a regra do alerta.

    ![Configurar sinal do alerta](./media/log-analytics-tutorial-response/alert-signal-logic-02.png)

7. Agora, passando ao segundo passo, indique o nome do alerta no campo **Nome da regra de alerta**, como **Percentagem da CPU maior que 90%**.  Especifique uma **Descrição** detalhando especificidades do alerta e selecione **Crítico (Grav 0)** como o valor de **Gravidade** nas opções fornecidas.

    ![Configurar detalhes do alerta](./media/log-analytics-tutorial-response/alert-signal-logic-04.png)

8. Para ativar imediatamente a regra de alerta na criação, aceite o valor predefinido de **Ativar regra após criação**.  
9. Para o terceiro e último passo, especifica um **Grupo de Ações**, que garante que as mesmas ações são feitas sempre que um alerta é acionado e pode ser utilizado para cada regra que definir.  Configure um novo grupo de ações com as seguintes informações:  
   a. Selecione **Novo grupo de ações** e o painel **Adicionar grupo de ações** é apresentado.  
   b. Em **Nome do grupo de ações**, especifique um nome como **Operações de TI – Notificar** e um **Nome abreviado** como **itops-n**.  
   c. Verifique se os valores predefinidos de **Subscrição** e **Grupo de recursos** estão corretos. Caso contrário, selecione os valores corretos na lista pendente.  
   d. Na secção Ações, especifique um nome para a ação, como **Enviar E-mail** e em **Tipo de Ação** selecione **E-mail/SMS/Push/Voz** na lista pendente. O painel de propriedades **E-mail/SMS/Push/Voz** abre à direita para facultar informações adicionais.  
   e. No painel **E-mail/SMS/Push/Voz**, ative **E-mail** e indique um endereço de e-mail SMTP válido para entregar a mensagem.  
   f. Clique em **OK** para guardar as alterações.  
       ![Criar novo grupo de ações](./media/log-analytics-tutorial-response/action-group-properties-01.png)

10. Clique em **OK** para concluir o grupo de ações.
11. Clique em **Criar regra de alerta** para concluir a regra de alerta. Começa imediatamente a executar.

    ![Concluir a criação da nova regra de alerta](./media/log-analytics-tutorial-response/alert-rule-01.png)

## <a name="view-your-alerts-in-azure-portal"></a>Veja os seus alertas no portal do Azure
Agora que criou um alerta, pode ver os alertas do Azure num painel único e gerir todas as regras de alerta nas subscrições do Azure. Lista todas as regras de alerta (ativadas ou desativadas) e podem ser ordenadas com base nos recursos de destino, grupos de recursos, nome da regra ou estado. Há um resumo agregado de todos os alertas acionados e o total de regras de alerta configuradas/ativadas.

![Página de estado dos Alertas do Azure](./media/log-analytics-tutorial-response/azure-alerts-02.png)

Quando o alerta é acionado, a tabela reflete a condição e quantas vezes ocorreu no intervalo de tempo selecionado (a predefinição é as últimas seis horas).  Deve haver um e-mail correspondente na sua caixa de correio semelhante ao seguinte exemplo mostrando a máquina virtual inválida e os principais resultados que correspondem à consulta de pesquisa neste caso.

![Exemplo de ação de e-mail de alerta](./media/log-analytics-tutorial-response/azure-alert-email-notification-01.png)

## <a name="next-steps"></a>Passos seguintes
Neste tutorial, aprendeu como as regras de alertas podem identificar e responder proativamente a um problema quando executam pesquisas de registo em intervalos agendados e corresponder a critérios específicos.

Siga esta ligação para ver os exemplos de scripts do Log Analytics pré-criados.

> [!div class="nextstepaction"]
> [Exemplos do script do Log Analytics](powershell-samples.md)
