---
title: Monitorizar mensagens B2B com o Azure Log Analytics - Azure Logic Apps | Documentos da Microsoft
description: Controlar a comunicação de B2B para contas de integração e o Azure Logic Apps com o Azure Log Analytics
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.date: 06/19/2018
ms.openlocfilehash: 5bf5385824eb9b711a2fee547c29d24d7ef5a01d
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/28/2018
ms.locfileid: "43125773"
---
# <a name="track-b2b-communication-with-azure-log-analytics"></a>Controlar B2B comunicação com o Azure Log Analytics

Após configurar a comunicação de B2B entre dois processos de negócios ou aplicativos através da sua conta de integração, a executar essas entidades podem trocar mensagens entre si. Para verificar se estas mensagens são processadas corretamente, pode controlar o AS2, X12, e mensagens de EDIFACT com [do Azure Log Analytics](../log-analytics/log-analytics-overview.md). Por exemplo, pode utilizar estas capacidades de controle baseado na web para mensagens de controlo:

* Contagem de mensagens e o Estado
* Estado de agradecimentos
* Correlacionar mensagens com os agradecimentos
* Descrições de erro detalhadas para falhas
* Capacidades de pesquisa

## <a name="requirements"></a>Requisitos

* Uma aplicação de lógica que está configurada com o registo de diagnósticos. Saiba mais [como criar uma aplicação lógica](quickstart-create-first-logic-app-workflow.md) e [como configurar o registo para essa aplicação lógica](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

* Uma conta de integração que está configurada com a monitorização e registo. Saiba mais [como criar uma conta de integração](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) e [como configurar a monitorização e registo para essa conta](../logic-apps/logic-apps-monitor-b2b-message.md).

* Se ainda não o fez, [publicar dados de diagnóstico para o Log Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

> [!NOTE]
> Depois de cumprir os requisitos anteriores, deve ter uma área de trabalho do Log Analytics. Deve usar a mesma área de trabalho para rastrear a comunicação de B2B no Log Analytics. 
>  
> Se não tiver uma área de trabalho do Log Analytics, saiba [como criar uma área de trabalho do Log Analytics](../log-analytics/log-analytics-quick-create-workspace.md).

## <a name="add-the-logic-apps-b2b-solution-to-log-analytics"></a>Adicionar a solução de aplicações lógicas B2B para o Log Analytics

Para que o Log Analytics monitorizar mensagens B2B para a aplicação lógica, tem de adicionar o **Logic Apps B2B/EDI** solução para o portal do OMS. Saiba mais sobre [adicionar soluções ao Log Analytics](../log-analytics/log-analytics-quick-create-workspace.md).

1. Na [portal do Azure](https://portal.azure.com), escolha **todos os serviços**. Procure "log analytics" e, em seguida, escolha **do Log Analytics** conforme mostrado aqui:

   ![Encontrar o Log Analytics](media/logic-apps-track-b2b-messages-omsportal/browseloganalytics.png)

2. Sob **do Log Analytics**, localize e selecione a sua área de trabalho do Log Analytics. 

   ![Selecione a sua área de trabalho do Log Analytics](media/logic-apps-track-b2b-messages-omsportal/selectla.png)

3. Sob **gerenciamento**, escolha **descrição geral**.

   ![Escolher o portal do Log Analytics](media/logic-apps-track-b2b-messages-omsportal/omsportalpage.png)

4. Depois de abrir a home page, escolha **adicionar** para instalar a solução de aplicações lógicas B2B.    
   ![Selecione a Galeria de soluções](media/logic-apps-track-b2b-messages-omsportal/add-b2b-solution.png)

5. Sob **soluções de gestão**, localize e crie **aplicações lógicas B2B** solução.     
   ![Escolher aplicações lógicas B2B](media/logic-apps-track-b2b-messages-omsportal/create-b2b-solution.png)

   Na home page, no mosaico relativo **mensagens de B2B de aplicações lógicas** aparece agora. 
   Este mosaico é atualizada a contagem de mensagens quando as mensagens B2B são processadas.

<a name="message-status-details"></a>

## <a name="track-message-status-and-details-in-log-analytics"></a>Controlar o estado de mensagem e os detalhes no Log Analytics

1. Depois das mensagens B2B são processadas, pode ver o estado e os detalhes para essas mensagens. Na página Descrição geral, escolha o **mensagens de B2B de aplicações lógicas** mosaico.

   ![Contagem de mensagens atualizado](media/logic-apps-track-b2b-messages-omsportal/b2b-overview-tile.png)

   > [!NOTE]
   > Por predefinição, o **mensagens de B2B de aplicações lógicas** mosaico mostra os dados com base num único dia. Para alterar o âmbito de dados para um intervalo diferente, escolha o controlo de âmbito na parte superior da página:
   > 
   > ![Alterar o âmbito de dados](media/logic-apps-track-b2b-messages-omsportal/server-filter.png)
   >

2. Após a mensagem de estado é apresentado o dashboard, pode ver mais detalhes sobre um tipo de mensagem específica, que mostra os dados com base num único dia. Escolha o mosaico da **AS2**, **X12**, ou **EDIFACT**.

   ![Ver o estado de mensagem](media/logic-apps-track-b2b-messages-omsportal/omshomepage5.png)

   É apresentada uma lista de mensagens para o seu mosaico escolhido. 
   Para saber mais sobre as propriedades para cada tipo de mensagem, consulte estas descrições de propriedade da mensagem:

   * [Propriedades da mensagem AS2](#as2-message-properties)
   * [Propriedades da mensagem X12](#x12-message-properties)
   * [Propriedades da mensagem EDIFACT](#EDIFACT-message-properties)

   Por exemplo, aqui está uma lista de mensagens AS2 possível aparência:

   ![Ver mensagens AS2](media/logic-apps-track-b2b-messages-omsportal/as2messagelist.png)

3. Para ver ou exportar as entradas e saídas de mensagens específicas, selecione essas mensagens e escolha **transferir**. Quando lhe for pedido, guarde o ficheiro. zip no computador local e, em seguida, extrair esse arquivo. 

   A pasta extraída inclui uma pasta para cada mensagem selecionada. 
   Se configurou as confirmações, a pasta de mensagem também inclui arquivos com detalhes de confirmação. 
   Cada pasta de mensagem tem, pelo menos, estes ficheiros: 
   
   * Ficheiros legível por humanos com o payload de entrada e os detalhes do payload de saída
   * Ficheiros codificados com as entradas e saídas

   Para cada tipo de mensagem, pode encontrar a pasta e formatos de nome de ficheiro aqui:

   * [Formatos de nome de pasta e ficheiro de AS2](#as2-folder-file-names)
   * [X12 formatos de nome de pasta e ficheiro](#x12-folder-file-names)
   * [Formatos de nome de pasta e ficheiro EDIFACT](#edifact-folder-file-names)

   ![Transferir ficheiros de mensagens](media/logic-apps-track-b2b-messages-omsportal/download-messages.png)

4. Para ver todas as ações que têm o mesmo ID de execução, diante da **pesquisa de registos** página, selecione uma mensagem a partir da lista de mensagem.

   Pode ordenar estas ações por coluna ou pesquisar resultados específicos.

   ![Ações com o mesmo ID de execução](media/logic-apps-track-b2b-messages-omsportal/logsearch.png)

   * Para pesquisar resultados com consultas pré-criadas, escolha **Favoritos**.

   * Saiba mais [como criar consultas, adicione filtros](logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md). 
   Ou obtenha mais informações [como encontrar dados com pesquisas de registos no Log Analytics](../log-analytics/log-analytics-log-searches.md).

   * Para alterar a consulta na caixa de pesquisa, atualize a consulta com as colunas e os valores que pretende utilizar como filtros.

<a name="message-list-property-descriptions"></a>

## <a name="property-descriptions-and-name-formats-for-as2-x12-and-edifact-messages"></a>Descrições das propriedades e formatos de nome de AS2, X12 e mensagens EDIFACT

Para cada tipo de mensagem, aqui estão as descrições de propriedade e formatos de nome para ficheiros de mensagens transferido.

<a name="as2-message-properties"></a>

### <a name="as2-message-property-descriptions"></a>Descrições de propriedade de mensagem AS2

Aqui estão as descrições de propriedade para cada mensagem AS2.

| Propriedade | Descrição |
| --- | --- |
| Remetente | O parceiro convidado especificado na **receber definições**, ou o parceiro do anfitrião especificado na **enviar definições** para um contrato de AS2 |
| Recetor | O parceiro do anfitrião especificado na **receber definições**, ou o parceiro convidado especificadas **enviar definições** para um contrato de AS2 |
| Aplicação Lógica | A aplicação lógica em que são configuradas as ações de AS2 |
| Estado | O estado de mensagem AS2 <br>Êxito = recebida ou enviada uma mensagem AS2 válida. Não existem MDN está definida. <br>Êxito = recebida ou enviada uma mensagem AS2 válida. MDN foi definida e recebido ou MDN é enviada. <br>Falha ao = recebida uma mensagem AS2 inválida. Não existem MDN está definida. <br>Pendente = recebida ou enviada uma mensagem AS2 válida. MDN está configurado e é esperado o MDN. |
| ACK | O estado da mensagem MDN <br>Aceite = recebidos ou enviados um MDN positivo. <br>Pendente = esperando para receber ou enviar um MDN. <br>Rejeitado = recebidos ou enviados um MDN negativo. <br>Não é necessário = MDN não está definido no contrato. |
| Direção | A direção de mensagem AS2 |
| ID de Correlação | O ID que correlaciona todos os acionadores e ações numa aplicação lógica |
| ID da mensagem | O ID da mensagem AS2 dos cabeçalhos de mensagem AS2 |
| Carimbo de data/hora | A hora quando a ação de AS2 processado a mensagem |
|          |             |

<a name="as2-folder-file-names"></a>

### <a name="as2-name-formats-for-downloaded-message-files"></a>Formatos de nome de AS2 para ficheiros de mensagens transferido

Aqui estão os formatos de nome para cada pasta de mensagem AS2 transferida e ficheiros.

| Ficheiro ou pasta | Formato de nome |
| :------------- | :---------- |
| Pasta de mensagem | [sender]\_[receiver]\_AS2\_[correlation-ID]\_[message-ID]\_[timestamp] |
| Entrada, saída e se a configurar, arquivos de confirmação | **Payload de entrada**: [remetente]\_[recetor]\_AS2\_[ID de correlação]\_input_payload.txt </p>**Payload de saída**: [remetente]\_[recetor]\_AS2\_[ID de correlação]\_saída\_payload.txt </p></p>**Entradas**: [remetente]\_[recetor]\_AS2\_[ID de correlação]\_inputs.txt </p></p>**Saídas**: [remetente]\_[recetor]\_AS2\_[ID de correlação]\_outputs.txt |
|          |             |

<a name="x12-message-properties"></a>

### <a name="x12-message-property-descriptions"></a>Descrições das propriedades da mensagem X12

Aqui estão as descrições de propriedade para cada X12 mensagem.

| Propriedade | Descrição |
| --- | --- |
| Remetente | O parceiro convidado especificado na **receber definições**, ou o parceiro do anfitrião especificado na **enviar definições** para um X12 contrato |
| Recetor | O parceiro do anfitrião especificado na **receber definições**, ou o parceiro convidado especificadas **enviar definições** para um X12 contrato |
| Aplicação Lógica | A aplicação lógica em que as ações são configuradas de X12 |
| Estado | O estado da mensagem X12 <br>Êxito = recebidos ou enviados um X12 válido mensagem. Não existem ack funcional é configurado. <br>Êxito = recebidos ou enviados um X12 válido mensagem. Ack funcional foi definida e recebido, ou um ack funcional é enviado. <br>Falha ao = recebidos ou enviados um X12 inválido mensagem. <br>Pendente = recebidos ou enviados um X12 válido mensagem. Ack funcional é configurado e prevê-se uma ack funcional. |
| ACK | Estado funcional do Ack (997) <br>Aceite = recebidos ou enviados uma ACK positivo. funcionais <br>Rejeitado = recebidos ou enviados uma ACK negativo. funcionais <br>Pendente = esperando um ack funcional, mas não foi recebido. <br>Pendente = gerado um ack funcional, mas não é possível enviar para o parceiro. <br>Não é necessário = funcional ack não está configurado. |
| Direção | A direção da mensagem X12 |
| ID de Correlação | O ID que correlaciona todos os acionadores e ações numa aplicação lógica |
| Tipo de mensagem | O tipo de mensagem 12 X de EDI |
| ICN | O número de controlo de intercâmbio de X12 mensagem |
| TSCN | O número de controlo do transação definido para o X12 mensagem |
| Carimbo de data/hora | A hora quando o X12 ação processado a mensagem |
|          |             |

<a name="x12-folder-file-names"></a>

### <a name="x12-name-formats-for-downloaded-message-files"></a>Formatos de nome de X12 para ficheiros de mensagens transferido

Aqui estão os formatos de nome para cada transferido X12 pasta e ficheiros de mensagens.

| Ficheiro ou pasta | Formato de nome |
| :------------- | :---------- |
| Pasta de mensagem | [sender]\_[receiver]\_X12\_[interchange-control-number]\_[global-control-number]\_[transaction-set-control-number]\_[timestamp] |
| Entrada, saída e se a configurar, arquivos de confirmação | **Payload de entrada**: [remetente]\_[recetor]\_X12\_[número de controlo de intercâmbio]\_input_payload.txt </p>**Payload de saída**: [remetente]\_[recetor]\_X12\_[número de controlo de intercâmbio]\_saída\_payload.txt </p></p>**Entradas**: [remetente]\_[recetor]\_X12\_[número de controlo de intercâmbio]\_inputs.txt </p></p>**Saídas**: [remetente]\_[recetor]\_X12\_[número de controlo de intercâmbio]\_outputs.txt |
|          |             |

<a name="EDIFACT-message-properties"></a>

### <a name="edifact-message-property-descriptions"></a>Descrições de propriedade de mensagem EDIFACT

Aqui estão as descrições de propriedade para cada mensagem EDIFACT.

| Propriedade | Descrição |
| --- | --- |
| Remetente | O parceiro convidado especificado na **receber definições**, ou o parceiro do anfitrião especificado na **enviar definições** para um contrato EDIFACT |
| Recetor | O parceiro do anfitrião especificado na **receber definições**, ou o parceiro convidado especificadas **enviar definições** para um contrato EDIFACT |
| Aplicação Lógica | A aplicação lógica em que são configuradas as ações de EDIFACT |
| Estado | O estado de mensagem EDIFACT <br>Êxito = recebida ou enviada uma mensagem EDIFACT válida. Não existem ack funcional é configurado. <br>Êxito = recebida ou enviada uma mensagem EDIFACT válida. Ack funcional foi definida e recebido, ou um ack funcional é enviado. <br>Falha ao = recebida ou enviada uma mensagem EDIFACT inválida <br>Pendente = recebida ou enviada uma mensagem EDIFACT válida. Ack funcional é configurado e prevê-se uma ack funcional. |
| ACK | Estado funcional do Ack (997) <br>Aceite = recebidos ou enviados uma ACK positivo. funcionais <br>Rejeitado = recebidos ou enviados uma ACK negativo. funcionais <br>Pendente = esperando um ack funcional, mas não foi recebido. <br>Pendente = gerado um ack funcional, mas não é possível enviar para o parceiro. <br>Não é necessário = Ack funcional não está configurado. |
| Direção | A direção de mensagem de EDIFACT |
| ID de Correlação | O ID que correlaciona todos os acionadores e ações numa aplicação lógica |
| Tipo de mensagem | O tipo de mensagem EDIFACT |
| ICN | O número de controlo de intercâmbio para a mensagem EDIFACT |
| TSCN | O número de controlo do transações definido para a mensagem EDIFACT |
| Carimbo de data/hora | A hora quando a ação de EDIFACT processado a mensagem |
|          |               |

<a name="edifact-folder-file-names"></a>

### <a name="edifact-name-formats-for-downloaded-message-files"></a>Formatos de nome EDIFACT para ficheiros de mensagens transferido

Aqui estão os formatos de nome para cada pasta de mensagem EDIFACT transferida e ficheiros.

| Ficheiro ou pasta | Formato de nome |
| :------------- | :---------- |
| Pasta de mensagem | [sender]\_[receiver]\_EDIFACT\_[interchange-control-number]\_[global-control-number]\_[transaction-set-control-number]\_[timestamp] |
| Entrada, saída e se a configurar, arquivos de confirmação | **Payload de entrada**: [remetente]\_[recetor]\_EDIFACT\_[número de controlo de intercâmbio]\_input_payload.txt </p>**Payload de saída**: [remetente]\_[recetor]\_EDIFACT\_[número de controlo de intercâmbio]\_saída\_payload.txt </p></p>**Entradas**: [remetente]\_[recetor]\_EDIFACT\_[número de controlo de intercâmbio]\_inputs.txt </p></p>**Saídas**: [remetente]\_[recetor]\_EDIFACT\_[número de controlo de intercâmbio]\_outputs.txt |
|          |             |

## <a name="next-steps"></a>Passos Seguintes

* [Consulta para mensagens B2B no Log Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md)
* [Esquemas de controlo de AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [Esquemas de controlo de X12](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Esquemas de controlo personalizado](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)