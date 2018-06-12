---
title: Ligar ao armazenamento de Blobs do Azure - Azure Logic Apps | Microsoft Docs
description: Criar e gerir os blobs no armazenamento do Azure com Azure Logic Apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 05/21/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 49d08135dee4568d1a9d65ec2d22d17ee3bda2ea
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294684"
---
# <a name="create-and-manage-blobs-in-azure-blob-storage-with-azure-logic-apps"></a>Criar e gerir os blobs no armazenamento de Blobs do Azure com Azure Logic Apps

Este artigo mostra como pode aceder e gerir ficheiros armazenados como os blobs na sua conta do storage do Azure de dentro de uma aplicação lógica com o conector do Blob Storage do Azure. Dessa forma, pode criar as logic apps que automatizem tarefas e fluxos de trabalho para gerir os seus ficheiros. Por exemplo, pode criar as logic apps que criarem, obterem, atualizarem e eliminar ficheiros na sua conta de armazenamento.

Suponha que tem uma ferramenta que obtém atualizada um web site do Azure. que atua como o acionador para a sua aplicação lógica. Quando este evento ocorre, pode ter a sua aplicação lógica atualizar alguns ficheiros no contentor de armazenamento de blob, que é uma ação na sua aplicação lógica. 

Se não tiver uma subscrição do Azure, <a href="https://azure.microsoft.com/free/" target="_blank">inscreva-se para obter uma conta do Azure gratuita</a>. Se estiver familiarizado com as logic apps, reveja [que é o Azure Logic Apps](../logic-apps/logic-apps-overview.md) e [início rápido: criar a sua primeira aplicação de lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).
Para obter informações técnicas específicas do conector, consulte o <a href="https://docs.microsoft.com/connectors/azureblobconnector/" target="blank">referência do Blob Storage do Azure conector</a>.

## <a name="prerequisites"></a>Pré-requisitos

* Um [conta de armazenamento do Azure e um contentor de armazenamento](../storage/blobs/storage-quickstart-blobs-portal.md)

* A aplicação lógica em que precisa de acesso à sua conta de armazenamento de Blobs do Azure. Para começar a sua aplicação lógica com um acionador de Blob Storage do Azure, é necessário um [aplicação lógica em branco](../logic-apps/quickstart-create-first-logic-app-workflow.md). 

<a name="add-trigger"></a>

## <a name="add-blob-storage-trigger"></a>Adicionar o acionador de armazenamento de BLOBs

No Azure Logic Apps, cada aplicação lógica tem de começar com uma [acionador](../logic-apps/logic-apps-overview.md#logic-app-concepts), que é desencadeado quando um evento específico acontece ou quando for cumprida uma condição específica. Sempre que o acionador é desencadeado, o motor de Logic Apps cria uma instância de aplicação lógica e inicia a executar o fluxo de trabalho da sua aplicação.

Este exemplo mostra como pode iniciar um fluxo de trabalho de aplicação lógica com o **Blob Storage do Azure - quando um blob é adicionado ou modificados (apenas propriedades)** acionador quando as propriedades de um blob obtém adicionados ou atualizados no contentor de armazenamento. 

1. No portal do Azure ou Visual Studio, crie uma aplicação de lógica em branco, o qual abre o Designer de aplicação lógica. Este exemplo utiliza o portal do Azure.

2. Na caixa de pesquisa, introduza "blob do azure" como o filtro. Na lista de acionadores, selecione o acionador que quiser.

   Este exemplo utiliza este acionador: **Blob Storage do Azure - quando um blob é adicionado ou modificados (apenas propriedades)**

   ![Selecione o acionador](./media/connectors-create-api-azureblobstorage/azure-blob-trigger.png)

3. Se lhe for pedida para detalhes de ligação, [criar a ligação de armazenamento de BLOBs agora](#create-connection). Em alternativa, se a ligação já existir, forneça as informações necessárias para o acionador.

   Neste exemplo, selecione o contentor e a pasta que pretende monitorizar.

   1. No **contentor** caixa, selecione o ícone de pasta.

   2. Na lista de pasta, escolha a abertura de ângulo Reto ( **>** ) e, em seguida, navegue até localizar e selecione a pasta que pretende. 

      ![Selecione a pasta](./media/connectors-create-api-azureblobstorage/trigger-select-folder.png)

   3. Selecione o intervalo de frequência e para a frequência com que pretende que o acionador para verificar a pasta para as alterações.

4. Quando tiver terminado, na barra de ferramentas estruturador, escolha **guardar**.

5. Agora continue a adicionar uma ou mais ações à sua aplicação lógica para as tarefas que pretende executar com os resultados de Acionador.

<a name="add-action"></a>

## <a name="add-blob-storage-action"></a>Adicionar ação do armazenamento de BLOBs

No Azure Logic Apps, um [ação](../logic-apps/logic-apps-overview.md#logic-app-concepts) é um passo no fluxo de trabalho que se segue um acionador ou outra ação. Neste exemplo, a aplicação lógica começa com o [acionador de recorrência](../connectors/connectors-native-recurrence.md).

1. No portal do Azure ou Visual Studio, abra a aplicação de lógica no Designer de aplicação lógica. Este exemplo utiliza o portal do Azure.

2. No Designer de aplicação lógica, sob o acionador ou ação, escolha **novo passo** > **adicionar uma ação**.

   ![Adicionar uma ação](./media/connectors-create-api-azureblobstorage/add-action.png) 

   Para adicionar uma ação entre passos existentes, mova o rato sobre a seta para a ligação. 
   Escolha o sinal de adição (**+**) que é apresentado e, em seguida, escolha **adicionar uma ação**.

3. Na caixa de pesquisa, introduza "blob do azure" como o filtro. Na lista de ações, selecione a ação que pretende.

   Este exemplo utiliza esta ação: **o Blob Storage do Azure - obter conteúdo de blob**

   ![Selecione a ação](./media/connectors-create-api-azureblobstorage/azure-blob-action.png) 

4. Se lhe for pedida para detalhes de ligação, [criar a ligação de armazenamento de Blobs do Azure agora](#create-connection). Em alternativa, se a ligação já existir, forneça as informações necessárias para a ação. 

   Neste exemplo, selecione o ficheiro que pretende.

   1. Do **Blob** caixa, selecione o ícone de pasta.
  
      ![Selecione a pasta](./media/connectors-create-api-azureblobstorage/action-select-folder.png)

   2. Localize e selecione o ficheiro que pretende com base no blob **Id** número. Pode encontrar isto **Id** número nos metadados do blob que é devolvido pelo acionador de armazenamento de BLOBs descrito anteriormente.

5. Quando tiver terminado, na barra de ferramentas estruturador, escolha **guardar**.
Para testar a sua aplicação lógica, certifique-se de que a pasta selecionada contém um blob.

Neste exemplo apenas obtém o conteúdo para um blob. Para ver os conteúdos, adicione outra ação que cria um ficheiro com o blob utilizando outro conector. Por exemplo, adicione uma ação de OneDrive que cria um ficheiro com base no conteúdo de blob.

<a name="create-connection"></a>

## <a name="connect-to-storage-account"></a>Ligar à conta de armazenamento

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

[!INCLUDE [Create a connection to Azure blob storage](../../includes/connectors-create-api-azureblobstorage.md)]

## <a name="connector-reference"></a>Referência do conector

Para obter detalhes técnicos, como acionadores, ações e limites, conforme descrito pelo ficheiro Swagger do conector, consulte o [página de referência do conector](/connectors/azureblobconnector/). 

## <a name="get-support"></a>Obter suporte

* Relativamente a dúvidas, visite o [fórum do Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para submeter ou votar em ideias para funcionalidades, visite o [site de comentários dos utilizadores do Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Passos seguintes

* Saiba mais sobre outros [conectores Logic Apps](../connectors/apis-list.md)
