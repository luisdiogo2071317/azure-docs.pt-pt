---
title: Mensagens de processamento de EDI do batch como um grupo ou uma coleção - Azure Logic Apps | Documentos da Microsoft
description: Enviar mensagens EDI para processamento em lote em aplicações lógicas
keywords: a processar do batch, batch, a codificação do lote
author: divswa
manager: jeconnoc
editor: ''
services: logic-apps
documentationcenter: ''
ms.assetid: ''
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/21/2017
ms.author: LADocs; estfan; divswa
ms.openlocfilehash: fb15688968cb29039fc669ed6b8685ba64df9e81
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39432138"
---
# <a name="send-x12-messages-in-batch-to-trading-partners"></a>Mensagens X12 de envio em lote para parceiros comerciais

Em cenários de empresa-empresa (B2B), parceiros, muitas vezes, trocam mensagens em grupos ou lotes. Para enviar mensagens em grupos ou lotes para parceiros comerciais, pode criar um lote com vários itens e, em seguida, utilize o X12 a ação de batch para processar esses itens como um lote.


Criação de batches de X12 mensagens, como outras mensagens, utiliza um acionador de lote e a ação. Também para X12, o batch passa por um X12 Encode passo antes de ir para o parceiro ou qualquer outro tipo de destino. Para obter mais informações sobre o acionador de lote e a ação, veja [mensagens de processamento de Batch](logic-apps-batch-process-send-receive-messages.md).

Este tópico mostra-lhe como pode processar X12 mensagens como um lote ao realizar estas tarefas:
* [Criar uma aplicação lógica que recebe itens e cria um lote](#receiver). Esta aplicação lógica do "destinatário" executa estas ações:
 
   * Especifica os critérios de nome e versão do batch para cumprir antes de liberar os itens como um lote.

   * Processos ou codifica os itens no batch com o X12 especificado identidades de contrato ou de parceiro.

* [Criar uma aplicação lógica que envia itens para um lote](#sender). Esta aplicação lógica do "remetente" Especifica onde enviar itens para processamento em lote, que tem de ser uma aplicação de lógica de destinatário existente.


## <a name="prerequisites"></a>Pré-requisitos

Para seguir este exemplo, precisa destes itens:

* Uma subscrição do Azure. Se não tiver uma subscrição, pode [começar com uma conta do Azure gratuita](https://azure.microsoft.com/free/). Caso contrário, pode [inscrever-se numa subscrição Pay As You Go](https://azure.microsoft.com/pricing/purchase-options/).

* Uma [conta de integração](logic-apps-enterprise-integration-create-integration-account.md) que já definida e associada à sua subscrição do Azure

* Pelo menos dois [parceiros](logic-apps-enterprise-integration-partners.md) que definiu na sua conta de integração. Certifique-se de que cada parceiro usa X12 (código de alfa de operadora padrão) qualificador nas propriedades do parceiro como uma identidade de negócio.

* Uma [X12 contrato](logic-apps-enterprise-integration-x12.md) que já está definido na sua conta de integração

<a name="receiver"></a>

## <a name="create-a-logic-app-that-receives-x12-messages-and-creates-a-batch"></a>Criar uma lógica de aplicação que recebe X12 mensagens e cria um lote

Antes de pode enviar mensagens para um lote, primeiro tem de criar uma aplicação de lógica do "destinatário" com o **Batch** acionador. Dessa forma, pode selecionar esta aplicação de lógica de destinatário ao criar a aplicação de lógica do remetente. Para o receptor, especifique o nome do lote, versão critérios, X12 contrato e outras definições. 


1. Na [portal do Azure](https://portal.azure.com), criar uma aplicação lógica com este nome: "BatchX12Messages".

1. No estruturador de aplicações lógicas, adicione a **Batch** acionador, que inicia o fluxo de trabalho de aplicação lógica. Na caixa de pesquisa, introduza "batch" como o filtro. Selecione este acionador: **Batch – mensagens de Batch**

   ![Adicionar o acionador de lote](./media/logic-apps-scenario-EDI-send-batch-messages/add-batch-receiver-trigger.png)

1. Forneça um nome para o batch e especificar critérios para liberar o batch, por exemplo:

   * **Nome do batch**: O nome utilizado para identificar o batch, o que é "TestBatch" neste exemplo.

   * **Critérios de versão**: os critérios de versão do batch, que podem ser baseados na contagem de mensagens, agenda ou ambos.
   
     ![Fornecer detalhes do acionador de lote](./media/logic-apps-batch-process-send-receive-messages/receive-batch-release-criteria.png)

   * **Contagem de mensagens**: O número de mensagens para armazenar como um lote antes do lançamento para processamento, o que é "5", neste exemplo.

     ![Fornecer detalhes do acionador de lote](./media/logic-apps-batch-process-send-receive-messages/receive-batch-count-based.png)

   * **Agenda**: A Cronograma de lançamento do batch para processamento, o que é "a cada 10 minutos" neste exemplo.

     ![Fornecer detalhes do acionador de lote](./media/logic-apps-scenario-EDI-send-batch-messages/receive-batch-schedule-based.png)


1. Adicione outra ação que codifica o agrupados ou lotes de mensagens e cria um X12 batches criados de mensagem. 

   a. Escolher **+ novo passo** > **adicionar uma ação**.

   b. Na caixa de pesquisa, introduza "12 batch X" como o filtro e selecione uma ação para **X12-codificação do lote**. Como o X12 codificar o conector, há diversas variações para o batch em ação de codificação. Pode selecionar um deles.

   ![Selecione X12 ação de codificação do lote](./media/logic-apps-scenario-EDI-send-batch-messages/add-batch-encode-action.png)
   
1. Defina as propriedades para a ação que acabou de adicionar.

   * Na **nome de X12 contrato** caixa, selecione o contrato da lista pendente. Se a lista estiver vazia, certifique-se de que criou a ligação à sua conta de integração.

   * Na **BatchName** caixa, selecione a **nome do lote** campo na lista de conteúdo dinâmica.
   
   * Na **PartitionName** caixa, selecione a **nome da partição** campo na lista de conteúdo dinâmica.

   * Na **itens** caixa, selecione a **itens em lotes** na lista de conteúdo dinâmica.

   ![Detalhes da ação Encode batch](./media/logic-apps-scenario-EDI-send-batch-messages/batch-encode-action-details.png)

1. Para fins de teste, adicionar uma ação de HTTP para enviar a mensagem em lote para [serviço de pedido Bin](https://requestbin.fullcontact.com/). 

   1. Introduza "HTTP" como o filtro na caixa de pesquisa. Selecione a ação: **HTTP - HTTP**
    
      ![Selecione a ação de HTTP](./media/logic-apps-scenario-EDI-send-batch-messages/batch-receive-add-http-action.png)

   1. Partir do **método** lista, selecione **POST**. Para o **Uri** caixa, gerar um URI para o contentor de pedido e introduza esse URI. Na **corpo** caixa, quando abre a lista de dinâmico, selecione a **corpo** no **codificação do lote por nome do contrato** secção. Se não vir **corpo**, escolha **ver mais** junto a **codificação do lote por nome do contrato**.

      ![Fornecer detalhes da ação de HTTP](./media/logic-apps-scenario-EDI-send-batch-messages/batch-receive-add-http-action-details.png)

1.  Agora que criou uma aplicação de lógica de destinatário, guarde a aplicação lógica.

    ![Guardar a aplicação lógica](./media/logic-apps-scenario-EDI-send-batch-messages/save-batch-receiver-logic-app.png)

    > [!IMPORTANT]
    > Uma partição tem um limite de 5.000 mensagens ou 80 MB. Se a condição for cumprida, o batch pode ser liberado, mesmo quando não for cumprida a condição definida pelo utilizador.

<a name="sender"></a>

## <a name="create-a-logic-app-that-sends-x12-messages-to-a-batch"></a>Criar uma aplicação lógica que envia X12 mensagens para um lote

Agora, crie um ou mais aplicações lógicas que enviam os itens para o batch definido pela aplicação de lógica de recetor. Para o remetente, especifique a aplicação de lógica de destinatário e nome do lote, conteúdo da mensagem e quaisquer outras definições. Opcionalmente, pode fornecer uma chave de partição única para dividir o batch em subconjuntos para recolher os itens com essa chave.

Aplicações lógicas de remetente precisam saber para onde enviar itens, enquanto as aplicações lógicas de destinatário não precisam saber nada sobre os remetentes.


1. Criar outra aplicação de lógica com este nome: "X12MessageSender". Adicionar este acionador à sua aplicação lógica: **pedido / resposta - pedido** 
   
   ![Adicionar o acionador de pedido](./media/logic-apps-scenario-EDI-send-batch-messages/add-request-trigger-sender.png)

1. Adicione um novo passo para enviar mensagens para um lote.

   1. Escolher **+ novo passo** > **adicionar uma ação**.

   1. Na caixa de pesquisa, introduza "batch" como o filtro. 

1. Selecione a ação: **enviar mensagens do batch – escolha um fluxo de trabalho do Logic Apps com o acionador de lote**

   ![Selecione "Enviar mensagens para o lote"](./media/logic-apps-scenario-EDI-send-batch-messages/send-messages-batch-action.png)

1. Agora, selecione a aplicação de lógica de "BatchX12Messages" que criou anteriormente, que agora aparece como uma ação.

   ![Selecione a aplicação de lógica do "destinatário do batch"](./media/logic-apps-scenario-EDI-send-batch-messages/send-batch-select-batch-receiver.png)

   > [!NOTE]
   > A lista mostra também a outras aplicações lógicas com acionadores de lote.

1. Defina as propriedades de batch.

   * **Nome do batch**: O nome do lote definido pela aplicação de lógica receptor, que é "TestBatch" neste exemplo e é validada no tempo de execução.

     > [!IMPORTANT]
     > Certifique-se de que não altere o nome do batch, o que deve corresponder ao nome do lote especificado pela aplicação de lógica de recetor.
     > Alterar o nome do lote faz com que o remetente aplicação lógica falhar.

   * **Conteúdo da mensagem**: O conteúdo da mensagem que pretende enviar para o batch
   
   ![Definir as propriedades do batch](./media/logic-apps-scenario-EDI-send-batch-messages/send-batch-select-batch-properties.png)

1. Guarde a aplicação lógica. A aplicação de lógica de remetente é agora semelhante a este exemplo:

   ![Guarde a aplicação de lógica de remetente](./media/logic-apps-scenario-EDI-send-batch-messages/send-batch-finished.png)

## <a name="test-your-logic-apps"></a>Testar as suas aplicações lógicas

Para testar a sua solução de criação de batches, post nas mensagens X12 para a sua aplicação remetente do [Postman](https://www.getpostman.com/postman) ou ferramenta semelhante. Em breve, deve iniciar a obtenção de mensagens X12, como um lote de itens de cinco ou a cada 10 minutos, de seu contentor de pedido - tudo com a mesma chave de partição.

## <a name="next-steps"></a>Passos Seguintes

* [Processar mensagens como lotes](logic-apps-batch-process-send-receive-messages.md) 
* [Criar uma aplicação sem servidor no Visual Studio com o Azure Logic Apps e funções](../logic-apps/logic-apps-serverless-get-started-vs.md)
* [Registo de erros para o logic apps e manipulação de exceção](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
