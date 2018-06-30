---
title: Ligar a sistemas SAP - Azure Logic Apps | Microsoft Docs
description: Como aceder e gerir recursos SAP ao automatizar fluxos de trabalho com Azure Logic Apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 05/31/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, divswa, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 3837896911b92361e0a6d0a7166a1b17651d6fe3
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37112865"
---
# <a name="connect-to-sap-systems-from-azure-logic-apps"></a>Ligar a sistemas SAP a partir do Azure Logic Apps

Este artigo mostra como pode acederem aos recursos SAP da dentro de uma aplicação lógica, utilizando os conectores de servidor de aplicações SAP e SAP mensagem de servidor. Dessa forma, pode automatizar tarefas, processos e fluxos de trabalho que gerem os seus dados SAP e recursos através da criação de aplicações lógicas.

Este exemplo utiliza uma aplicação lógica que podem acionar com um pedido de HTTP. A aplicação lógica envia um documento intermédias (IDoc) para um servidor SAP e devolve uma resposta para o requerente que chamar a aplicação lógica.
Os conectores SAP atuais tem ações, mas não a acionadores, pelo que este exemplo utiliza o [acionador de pedido HTTP](../connectors/connectors-native-reqres.md) como o primeiro passo no fluxo de trabalho da aplicação lógica. Para informações técnicas específicas do conector SAP, consulte estes artigos de referência: 

* <a href="https://docs.microsoft.com/connectors/sapapplicationserver/" target="blank">Conector de servidor de aplicações SAP</a>
* <a href="https://docs.microsoft.com/connectors/sapmessageserver/" target="blank">Conector de mensagem de servidor do SAP</a>

Se não tiver uma subscrição do Azure ainda, <a href="https://azure.microsoft.com/free/" target="_blank">inscrever-se numa conta do Azure gratuita</a>.

## <a name="prerequisites"></a>Pré-requisitos

Para seguir este artigo, terá destes itens:

* A aplicação de lógica de onde pretende aceder ao seu sistema SAP e um acionador que inicia o fluxo de trabalho da sua aplicação lógica. Os conectores SAP atualmente fornecem apenas ações. Se estiver familiarizado com as logic apps, reveja [que é o Azure Logic Apps](../logic-apps/logic-apps-overview.md) e [início rápido: criar a sua primeira aplicação de lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* O <a href="https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server" target="_blank">servidor de aplicações SAP</a> ou <a href="https://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm" target="_blank">servidor SAP mensagens em fila</a>

* Transfira e instale a versão mais recente [gateway de dados no local](https://www.microsoft.com/download/details.aspx?id=53127) em qualquer computador no local. Certifique-se de que configurou o gateway no portal do Azure antes de continuar. O gateway ajuda-o em segurança aceder a dados e recursos estão no local. Para obter mais informações, consulte [instalação local gateway de dados para o Azure Logic Apps](../logic-apps/logic-apps-gateway-install.md).

* Transferir e instalar a biblioteca de cliente SAP mais recente, o que está atualmente <a href="https://softwaredownloads.sap.com/file/0020000000086282018" target="_blank">SAP conector (NCo) 3.0.20.0 para o Microsoft .NET Framework 4.0 e o Windows de 64 bits (x64)</a>, no mesmo computador como gateway de dados no local. Instalar esta versão ou posterior para estes motivos:

  * Versões anteriores do SAP NCo podem tornar-se a estão bloqueadas quando são enviadas mensagens de IDoc mais do que um ao mesmo tempo. 
  Esta condição bloqueia todas as mensagens posteriores que são enviadas para o destino de SAP, fazendo com que as mensagens para o tempo limite.

  * O gateway de dados no local só é executada em sistemas de 64 bits. 
  Caso contrário, receberá um erro de "imagem incorreto" porque o serviço de anfitrião de gateway de dados não suporta as assemblagens de 32 bits.

  * O serviço de anfitrião de gateway de dados e o adaptador de SAP da Microsoft utilizam o .NET Framework 4.5. O NCo SAP para o .NET Framework 4.0 funciona com processos que utilizam o tempo de execução de .NET 4.0 para 4.7.1. 
  O NCo SAP para o .NET Framework 2.0 funciona com processos que utilizam o tempo de execução de .NET 2.0 para 3.5 e já não funciona com o gateway de dados no local mais recente.

* Conteúdo da mensagem que pode enviar para o servidor do SAP, tal como um ficheiro de IDoc de exemplo. Este conteúdo tem de estar no formato XML e incluir o espaço de nomes para a ação de SAP que pretende utilizar.

<a name="add-trigger"></a>

## <a name="add-http-request-trigger"></a>Adicionar o acionador de pedido HTTP

No Azure Logic Apps, cada aplicação lógica tem de começar com uma [acionador](../logic-apps/logic-apps-overview.md#logic-app-concepts), que é desencadeado quando um evento específico acontece ou quando for cumprida uma condição específica. Sempre que o acionador é desencadeado, o motor de Logic Apps cria uma instância de aplicação lógica e inicia a executar o fluxo de trabalho da sua aplicação.

Neste exemplo, criar uma aplicação lógica com um ponto final no Azure para que possa enviar *pedidos de HTTP POST* à sua aplicação lógica. Quando a sua aplicação lógica recebe estes pedidos HTTP, o acionador desencadeado e executa o passo seguinte no fluxo de trabalho.

1. No portal do Azure, crie uma aplicação de lógica em branco, o qual abre o Designer de aplicação lógica. 

2. Na caixa de pesquisa, introduza "pedido de http" como o filtro. Na lista de acionadores, selecione este acionador: **pedido - pedido de HTTP de quando é recebido**

   ![Adicionar o acionador de pedido de HTTP](./media/logic-apps-using-sap-connector/add-trigger.png)

3. Guarde agora a sua aplicação lógica para pode gerar um URL de ponto final para a sua aplicação lógica.
Na barra de ferramentas do estruturador, escolha **Guardar**. 

   O ponto final do URL aparece o acionador, por exemplo:

   ![Gerar o URL para o ponto final](./media/logic-apps-using-sap-connector/generate-http-endpoint-url.png)

<a name="add-action"></a>

## <a name="add-sap-action"></a>Adicionar ação SAP

No Azure Logic Apps, um [ação](../logic-apps/logic-apps-overview.md#logic-app-concepts) é um passo no fluxo de trabalho que se segue um acionador ou outra ação. Se ainda não adicionou um acionador à sua aplicação lógica e pretender a seguir neste exemplo, [adicionar o acionador descrito nesta secção](#add-trigger).

1. No Designer de aplicação lógica, sob o acionador, escolha **novo passo** > **adicionar uma ação**.

   ![Adicionar uma ação](./media/logic-apps-using-sap-connector/add-action.png) 

2. Na caixa de pesquisa, introduza "servidor sap" como o filtro. Na lista de ações, selecione a ação para o servidor do SAP: 

   * **Servidor de aplicações SAP - enviar para SAP**
   * **Servidor de mensagens SAP - enviar para SAP**

   Este exemplo utiliza esta ação: **servidor de aplicações SAP - enviar para SAP**

   ![Selecione "Servidor de aplicações SAP" ou "Servidor de mensagens SAP"](media/logic-apps-using-sap-connector/select-sap-action.png)

3. Se lhe for pedida para detalhes de ligação, crie a ligação de SAP agora. Caso contrário, se a ligação já existir, continue com o passo seguinte para que possa definir a ação de SAP. 

   **Criar a ligação de SAP no local**

   1. Para **Gateways**, selecione **ligar através do gateway de dados no local** , de modo a que são apresentadas as propriedades de ligação no local.

   2. Forneça as informações de ligação para o servidor do SAP. 
   Para o **gateway** propriedade, selecione o gateway de dados que criou no portal do Azure para a sua instalação do gateway, por exemplo:

      **Servidor de aplicações SAP**

      ![Criar a ligação ao servidor de aplicações SAP](./media/logic-apps-using-sap-connector/create-SAP-app-server-connection.png)  

      **Servidor de mensagens SAP**

      ![Criar a ligação ao servidor SAP mensagem](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png) 

   2. Quando tiver terminado, escolha **Create** (Criar).

      As Logic Apps configura e testa a ligação, certificando-se de que a ligação funciona corretamente.

4. Agora, localize e selecione uma ação do servidor SAP. 

   1. No **ação SAP** caixa, escolha o ícone da pasta. 
   Na lista de pasta, localize e selecione a ação que pretende utilizar. 

      Neste exemplo seleciona o **IDOC** categoria para a ação de IDoc. 

      ![Localize e selecione a ação de IDoc](./media/logic-apps-using-sap-connector/SAP-app-server-find-action.png)

      Se não conseguir encontrar a ação que pretende, pode introduzir manualmente um caminho, por exemplo:

      ![Fornecer manualmente o caminho para a acção de IDoc](./media/logic-apps-using-sap-connector/SAP-app-server-manually-enter-action.png)

      Para obter mais informações sobre as operações de IDoc, consulte [esquemas para operações de IDOC da mensagem](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations)

   2. Clique no interior do **mensagem de entrada** caixa, de modo a que é apresentada a lista de conteúdo dinâmica. 
   Nessa lista em **quando HTTP de pedido é recebido**, selecione o **corpo** campo. 

      Este passo inclui o conteúdo do corpo do seu acionador de pedido HTTP e envia que devolvem ao seu servidor SAP.

      ![Selecione o campo "Corpo"](./media/logic-apps-using-sap-connector/SAP-app-server-action-select-body.png)

      Quando tiver terminado, a ação de SAP aspeto neste exemplo:

      ![Ação de SAP concluída](./media/logic-apps-using-sap-connector/SAP-app-server-complete-action.png)

6. Guarde a aplicação lógica. Na barra de ferramentas do estruturador, escolha **Guardar**.

<a name="add-response"></a>

## <a name="add-http-response-action"></a>Adicionar ação de resposta HTTP

Agora, adicionar uma ação de resposta ao fluxo de trabalho da sua aplicação lógica e incluir o resultado da ação SAP. Dessa forma, a aplicação lógica devolve os resultados do seu servidor SAP para o requerente original. 

1. No Designer de aplicação lógica, sob a ação de SAP, escolha **novo passo** > **adicionar uma ação**.

2. Na caixa de pesquisa, introduza "resposta" como o filtro. Na lista de ações, selecione esta ação: **pedido - resposta**

3. Clique no interior do **corpo** caixa, de modo a que é apresentada a lista de conteúdo dinâmica. Essa lista em **enviar para SAP**, selecione o **corpo** campo. 

   ![Ação de SAP concluída](./media/logic-apps-using-sap-connector/select-sap-body-for-response-action.png)

4. Guarde a aplicação lógica. 

## <a name="test-your-logic-app"></a>Testar a sua aplicação lógica

1. Se a sua aplicação lógica já não está ativada, no menu aplicação lógica, escolha **descrição geral**. Na barra de ferramentas, escolha **ativar**. 

2. Na barra de ferramentas Designer de aplicação lógica, escolha **executar**. Este passo manualmente inicia a sua aplicação lógica.

3. Acionar a sua aplicação lógica, enviando um pedido POST de HTTP para o URL na sua acionador de pedido HTTP e incluem a sua mensagem de conteúdo com o seu pedido. Enviar o pedido, pode utilizar uma ferramenta como [Postman](https://www.getpostman.com/apps). 

   Para este artigo, o pedido envia um ficheiro de IDoc, que tem de estar no formato XML e incluir o espaço de nomes para a ação de SAP que utilizar, por exemplo: 

   ``` xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <Send xmlns="http://Microsoft.LobServices.Sap/2007/03/Idoc/3/ORDERS05//620/Send">
      <idocData>
         <...>
      </idocData>
   </Send>
   ```

4. Depois de enviar o pedido HTTP, aguarde que a resposta da sua aplicação lógica.

> [!NOTE]
> A aplicação lógica poderá o limite de tempo se a todos os passos necessários para a resposta não concluída dentro do [limite de tempo limite do pedido](./logic-apps-limits-and-config.md). Se esta condição ocorre, poderão obter bloqueados pedidos. Para ajudar a diagnosticar problemas, saiba como pode [Verifique e monitorizar as logic apps](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Parabéns, criou agora uma aplicação lógica que possa comunicar com o seu servidor SAP. Agora que configurou uma ligação de SAP para a sua aplicação lógica, pode explorar outras ações de SAP disponíveis, como BAPI e RFC.

## <a name="connector-reference"></a>Referência do conector

Para obter detalhes técnicos sobre o conector conforme descrito pelo ficheiros de Swagger dos conectores, consulte estes artigos de referência: 

* [Servidor de aplicações SAP](/connectors/sapapplicationserver/)
* [Servidor de mensagens SAP](/connectors/sapmessageserver/)

## <a name="get-support"></a>Obter suporte

* Relativamente a dúvidas, visite o [fórum do Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para submeter ou votar em ideias para funcionalidades, visite o [site de comentários dos utilizadores do Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Passos Seguintes

* [Ligar a sistemas no local](../logic-apps/logic-apps-gateway-connection.md) partir das logic apps
* Saiba como validar, transformar e outras operações de mensagem com o [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md)
* Saiba mais sobre outros [conectores Logic Apps](../connectors/apis-list.md)
